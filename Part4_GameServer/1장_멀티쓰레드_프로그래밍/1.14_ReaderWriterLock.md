```csharp
using System.Threading.Tasks;
using System.Threading;

namespace ServerCore
{
    class Program
    {
        // 1. 근성 => 계속 트리아
        // 2. 양보 => 자리로 돌아갔다가 복귀
        // 3. 갑질 => 운영체제한테 맡기기

        // 각자 장단점이 존재함
        // 무엇이 더 좋다고 할 수는 없음

        // 라이브러리에서 여러가지 방법을 혼합해서 만드는 경우가 많음

        // Monitor

        static object _lock = new object();
        
        // 기본적으로 계속 반복을 하다가
        // 도저히 안되겠을 때 소유권을 양도하는 방법을 사용
        // 1,2번 방법을 혼용해서 사용함
        static SpinLock _lock2 = new SpinLock();
        // 별도의 프로그램에서 순서를 맞추는 동기화 작업을 할 수 있는 장점이 있음
        // MMORPG 환경에서는 프로그램 안에서 멀티 쓰레드 환경에서 돌아가기 때문에
        // 프로그램 사이에 Mutex로 동기화 작업을 하는 장점이 그리 크지가 않다.
        static Mutex _lock3 = new Mutex();

        // 직접 만들기
        // 처음 배울 때는 직접 만드는 걸로
        // 후에 있는 것을 가져다 사용    

        // 결국 상황에 따라 어떤 방법을 사용할지 결정을 해야한다.
        // 즉, 직접 이것 저것 테스트를 해보는 방법을 사용
        // 게임을 서버에 올린 다음에 lock 획득이 너무 오래 걸리는지 테스트

        // lock과 SpinLock은 결국 내부적인 구현 및 실행 속도는 차이가 있지만 
        // 한번에 한놈만 들여보내는 컨셉 
        // 상호 배제가 기본 철학 => 나만 들어갈 것이다

        // 보상 획득
        // [ ] [ ] [ ]
        // 추가 이벤트 보상을 더 달 수 있음
        // 즉 멀티 쓰레드를 활용해서 추가 보상 정보를 write해야하는 경우가 발생
        // Reward 정보를 획득할 때는 누구든 접근할 수 있도록 하다가
        // 추가 Reward 정보 써야할 때는 lock을 걸어서 들어오지 못하게 막는 방법을 사용
        class Reward
        {

        }
        // RWLock ReaderWriterLock
        // ReaderWriterLockSlim가 최신버전
        static ReaderWriterLockSlim _lock4 = new ReaderWriterLockSlim();

        // id를 통해 어떤 보상을 줘야하는지를 확인하는 함수
        // 여러 쓰레드가 동시 다발적으로 해당 함수에 들어왔다고 가정했을 때
        // 아무도 WriterLock을 잡고 있지 않다고 한다면 
        // 마치 Lock이 없는 것처럼 자유롭게 들어올 수 있음
        static Reward GetRewardById(int id)
        {
            // 읽을 때
            _lock4.EnterReadLock();

            _lock4.ExitReadLock();

            return null;
        }

        static void AddReward(Reward reward)
        {
            // WriteLock을 잡고 있다면 ReadLock을 잡지 못함
            _lock4.EnterWriteLock();

            _lock4.ExitWriteLock();
        }

        static void Main(string[] args)
        {
            lock (_lock)
            {

            }
            bool lockTaken = false;
            // lock을 enter하는 도중에 exception 발생 시
            // 정상적으로 처리가 안될 경우
            // lockTaken에 값을 넣어줌
            try
            {
                _lock2.Enter(ref lockTaken);

            }
            finally
            {
                if (lockTaken)
                {
                    _lock2.Exit();
                }
            }

        }
        // 서버를 계속 쌓다보면 결국 멀티쓰레드를 많이 사용해야 하는데 
        // 핵심적인 코어부분만 멀티쓰레드로 만들 것이냐
        // 아니면 게임 콘텐츠도 멀티쓰레드로 만들 것이냐는 별개의 문제
        // 게임 콘텐츠도 멀티쓰레드 환경에서 도는 것을 만들게 되면(모든 코드가 멀티쓰레드로 돌아감)
        // 난이도가 기하급수적으로 올라감
        // 단 심리스MMORPG를 만들 때 굉장히 큰 장점이 있음 
        // 특정 영역 안에서 동작을 하는 게임이면(ex. 바람의 나라) 
        // 특정 공간 안에서 동작하는 콘텐츠는 싱글 쓰레드로 동작하게 하는 것이 훨씬 더 생각하기 쉽고
        // 버그를 줄일 수 있다.
        // 결국 또 선택의 문제
    }
}
```