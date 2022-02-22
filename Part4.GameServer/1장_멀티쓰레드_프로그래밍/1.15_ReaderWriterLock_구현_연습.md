```csharp
using System;
using System.Collections.Generic;
using System.Text;
using System.Threading;

namespace ServerCore
{
    // lock 생성 정책
    // 재귀적 락을 허용할지 (No) 
    // => WriteLock을 Acquire한 상태에서 또 다시 재귀적으로 같은 쓰레드에서 Acquire를 시도하는 것을 허용할지 여부를 결정
    // 허용을 안하는 것이 좀더 쉬움
    
    // SpinLock 정책 (5000번 -> Yield) 
    // Yield : 자신의 제어권을 양보 
    class Lock
    {
        // [Unused(0)] [WriteThreadId(15)] [ReadCount(16)]
        // 0000 0000 0000 0000 0000 0000 0000 0000
        // 가장 왼쪽 0 : Unused(0)
        // 가장 왼쪽 1 ~ 15 : WriteThreadId(15)
        // 가장 왼쪽 16 ~ 32 : ReadCount(16)
        // 꿀팁 : 16진수 : 2진수 = F : 11111
        const int EMPTY_FLAG = 0x00000000;
        const int WRITE_MASK = 0x7FFF0000;
        const int READ_MASK = 0x0000FFFF;
        const int MAX_SPIN_COUNT = 5000;

        // ReadCount : ReadLock을 획득했을 때 여러 쓰레드에서 Read를 잡을 수 있음 -> 그것을 카운팅
        // WriteThreadId : WriteThread를 잡고 있는 한개의 쓰레드
        int _flag = EMPTY_FLAG;

        public void WriteLock()
        {
            // 아무도 WriteLock or ReadLock을 획득하고 있지 않을 때, 경합해서 소유권을 얻는다.
            int desired = (Thread.CurrentThread.ManagedThreadId << 16) & WRITE_MASK;
            while(true)
            {
                for (int i = 0; i < MAX_SPIN_COUNT; i++)
                {
                    // 시도를 해서 성공하면 return
                    if(Interlocked.CompareExchange(ref _flag, desired, EMPTY_FLAG) == EMPTY_FLAG)
                    {
                        return;
                    }

                    // if (_flag == EMPTY_FLAG)
                    // {
                    //     _flag = desired;
                    //      return;
                    // }
                }
                Thread.Yield();
            }
        }
        public void WriteUnlock()
        {
            // 초기 상태로 변경
            Interlocked.Exchange(ref _flag, EMPTY_FLAG);
        }
        public void ReadLock()
        {
            // 아무도 WriteLock을 획득하고 있지 않으면, ReadCount를 1 늘린다.
            // ReadLock 같은 경우는 누구나 접근이 가능하기 때문에 쿨하게 1씩 늘려준다.
            while (true)
            {
                for (int i = 0; i < MAX_SPIN_COUNT; i++)
                {
                    // Lock Free Programming 기초

                    // 만약 누군가 lock을 잡고 있다면(WriteLock) 
                    // expected 값이 내가 원하는 값이 아닐 테니깐  
                    // 아래 if 문에서 실패를 하게 될 것이다.
                    int expected = (_flag & READ_MASK); // READ_MASK 부분만 추출

                    
                    // 대가 예상한 값은 expected고 기대한 값은 expected + 1인데 
                    // 아래 if 문이 성공했다는 의미는 (_flag & WRITE_MASK) == 0과 동일
                    // 즉 flag과 expected가 동일 => flag값을 1 더해줌
                    // 즉 ReadLock을 성공하고 더이상 시도하지 않음
                    if(Interlocked.CompareExchange(ref _flag, expected + 1, expected) == expected)
                    {
                        return;
                    }
                    // 체크를 하고 1을 늘리는 상황에서 다른 쓰레드에서 접근을 하면 문제가 발생할 수 있음
                    // if ((_flag & WRITE_MASK) == 0)
                    // {
                    //     _flag = _flag + 1;
                    //     return;
                    // }
                }
                Thread.Yield();
            }

            // 위의 예제를 이해하기 위한 시나리오
            // 두개의 쓰레드가 거의 동시에 ReadLock에 진입을 했고 A가 B보다 먼저 진입을 했다면
            // A expected : 0
            // B expected : 0
            // flag == 0 ? A => 1
            // flag == 0 ? B => 1 
            // 이렇게 경합을 하게 됨
            // A가 먼저 실행되었다고 가정하면
            // flag는 1로 바뀜 => A는 성공
            // 그 다음 B는 flag가 이미 바뀌었기 때문에 실패 => 다시 재시도

        }
        public void ReadUnlock()
        {
            Interlocked.Decrement(ref _flag);
        }
    }
}
```

```csharp
using System;
using System.Collections.Generic;
using System.Text;
using System.Threading;

namespace ServerCore
{
    // lock 생성 정책
    // 재귀적 락을 허용할지 (Yes)  => WriteLock => WriteLock Ok, WriteLock => ReadLock Ok, ReadLock => WriteLock No
    
    // SpinLock 정책 (5000번 -> Yield) 
    // Yield : 자신의 제어권을 양보 
    class Lock
    {
        // [Unused(0)] [WriteThreadId(15)] [ReadCount(16)]
        // 0000 0000 0000 0000 0000 0000 0000 0000
        // 가장 왼쪽 0 : Unused(0)
        // 가장 왼쪽 1 ~ 15 : WriteThreadId(15)
        // 가장 왼쪽 16 ~ 32 : ReadCount(16)
        // 꿀팁 : 16진수 : 2진수 = F : 11111
        const int EMPTY_FLAG = 0x00000000;
        const int WRITE_MASK = 0x7FFF0000;
        const int READ_MASK = 0x0000FFFF;
        const int MAX_SPIN_COUNT = 5000;

        // ReadCount : ReadLock을 획득했을 때 여러 쓰레드에서 Read를 잡을 수 있음 -> 그것을 카운팅
        // WriteThreadId : WriteThread를 잡고 있는 한개의 쓰레드
        int _flag = EMPTY_FLAG;
        int _writeCount = 0;

        public void WriteLock()
        {

            // 동일 쓰레드가 WriteLock을 이미 획득하고 있는지 확인
            int lockThreadID = (_flag & WRITE_MASK) >> 16;
            if (Thread.CurrentThread.ManagedThreadId == lockThreadID)
            {
                _writeCount++;
                return;
            }
            // 아무도 WriteLock or ReadLock을 획득하고 있지 않을 때, 경합해서 소유권을 얻는다.
            int desired = (Thread.CurrentThread.ManagedThreadId << 16) & WRITE_MASK;
            while(true)
            {
                for (int i = 0; i < MAX_SPIN_COUNT; i++)
                {
                    // 시도를 해서 성공하면 return
                    if(Interlocked.CompareExchange(ref _flag, desired, EMPTY_FLAG) == EMPTY_FLAG)
                    {
                        _writeCount = 1;
                        return;
                    }

                    // if (_flag == EMPTY_FLAG)
                    // {
                    //     _flag = desired;
                    //      return;
                    // }
                }
                Thread.Yield();
            }
        }
        public void WriteUnlock()
        {
            int lockCount = --_writeCount;
            if (lockCount == 0)
            {
                // 초기 상태로 변경
                Interlocked.Exchange(ref _flag, EMPTY_FLAG);
            }

        }
        public void ReadLock()
        {

            int lockThreadID = (_flag & WRITE_MASK) >> 16;
            if (Thread.CurrentThread.ManagedThreadId == lockThreadID)
            {
                Interlocked.Increment(ref _flag);
                return;
            }
            // 아무도 WriteLock을 획득하고 있지 않으면, ReadCount를 1 늘린다.
            // ReadLock 같은 경우는 누구나 접근이 가능하기 때문에 쿨하게 1씩 늘려준다.
            while (true)
            {
                for (int i = 0; i < MAX_SPIN_COUNT; i++)
                {
                    // Lock Free Programming 기초

                    // 만약 누군가 lock을 잡고 있다면(WriteLock) 
                    // expected 값이 내가 원하는 값이 아닐 테니깐  
                    // 아래 if 문에서 실패를 하게 될 것이다.
                    int expected = (_flag & READ_MASK); // READ_MASK 부분만 추출

                    
                    // 대가 예상한 값은 expected고 기대한 값은 expected + 1인데 
                    // 아래 if 문이 성공했다는 의미는 (_flag & WRITE_MASK) == 0과 동일
                    // 즉 flag과 expected가 동일 => flag값을 1 더해줌
                    // 즉 ReadLock을 성공하고 더이상 시도하지 않음
                    if(Interlocked.CompareExchange(ref _flag, expected + 1, expected) == expected)
                    {
                        return;
                    }
                    // 체크를 하고 1을 늘리는 상황에서 다른 쓰레드에서 접근을 하면 문제가 발생할 수 있음
                    // if ((_flag & WRITE_MASK) == 0)
                    // {
                    //     _flag = _flag + 1;
                    //     return;
                    // }
                }
                Thread.Yield();
            }

            // 위의 예제를 이해하기 위한 시나리오
            // 두개의 쓰레드가 거의 동시에 ReadLock에 진입을 했고 A가 B보다 먼저 진입을 했다면
            // A expected : 0
            // B expected : 0
            // flag == 0 ? A => 1
            // flag == 0 ? B => 1 
            // 이렇게 경합을 하게 됨
            // A가 먼저 실행되었다고 가정하면
            // flag는 1로 바뀜 => A는 성공
            // 그 다음 B는 flag가 이미 바뀌었기 때문에 실패 => 다시 재시도

        }
        public void ReadUnlock()
        {
            Interlocked.Decrement(ref _flag);
        }
    }
}
```

```csharp
using System.Threading.Tasks;
using System.Threading;

namespace ServerCore
{
    class Program
    {
        // lock free programming 기법과 비슷
        static volatile int count = 0;
        static Lock _lock = new Lock();
        static void Main(string[] args)
        {
            Task t1 = new Task(delegate ()
            {
                for (int i = 0; i < 10000; i++)
                {
                    _lock.WriteLock();
                    _lock.WriteLock();
                    count++;
                    _lock.WriteUnlock();
                    _lock.WriteUnlock();
                }
            });
            Task t2 = new Task(delegate ()
            {
                for (int i = 0; i < 10000; i++)
                {
                    _lock.WriteLock();
                    count--;
                    _lock.WriteUnlock();
                }
            });
            t1.Start();
            t2.Start();

            Task.WaitAll(t1, t2);
            System.Console.WriteLine(count);
        }
    }
}
```