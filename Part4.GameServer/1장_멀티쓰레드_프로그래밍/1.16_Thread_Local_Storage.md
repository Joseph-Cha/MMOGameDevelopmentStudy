기본 개념

전역변수인데 쓰레드마다 고유하게 접근할 수 있는 전역변수

왜 필요할까?

스토리

![image](https://user-images.githubusercontent.com/75019048/131054658-14ce9790-2da1-467d-bc3b-cd2ff2ac7956.png)

각 쓰레드를 각 프로세스에 할당해서 해당 프로세스 작업만 하는 것이 아니라

위 이미지와 같이 다양한 프로세스를 이곳 저곳 옮겨 다니면서 작업을 하게 된다.

![image](https://user-images.githubusercontent.com/75019048/131054673-8918d9fe-9056-4d69-9381-bc05936dd393.png)

그래서 경합이 발생될 만한 곳에 락을 생성하는 방법을 생각할 수도 있다.

일감 분배를 어떻게 해야하는가 병렬 프로그래밍의 핵심이다.

![image](https://user-images.githubusercontent.com/75019048/131054680-91346dc6-3b01-48f7-af76-1e4b409aa01a.png)

2명의 직원이 한 테이블에 몰리는 것은 말이 안됨?

정해진 방법이 없기 때문에 그저 최선의 방법을 찾아나가는 방법 밖에는 없다.

![image](https://user-images.githubusercontent.com/75019048/131054701-0e9160f3-f231-47c7-9171-b051d75aa1e0.png)

아이템을 강화 → DB에 저장 → 성공 → 클라이언트 세션

위와같이 모든 로직들이 다 얽혀 있게 된다.

이런 식으로 락을 모든 부분에 락을 배치하면 해결이 될 것 같지만 그렇게 되면 치명적인 문제가 발생한다.

바로 한쪽에 쓰레드가 몰리는 경우가 발생한다.

예를들어 Wow에서 몇 백명이 모여서 전쟁을 펼칠 때

클라이언트 세션에서 같은 공간으로 패킷을 쏘게 될 것이다.

락은 상호 배타적이기 때문에 쓰레드가 아무리 한 곳에 몰린다고 해도 한번에 일감을 하나씩 밖에 처리를 못하게 된다.

그래서 멀티쓰레드를 활용해서 결국 쓰레드를 많이 고용해도 결국 lock 때문에(동기화 때문에) 빠른 속도로 일감을 처리하지 못한다.

이런 경우는 오히려 멀티쓰레드로 처리하는 게 더 안좋다. 왜냐면 위와 같이 쓰레드가 한 곳에 쏠리는 현상도 발생하지만 

그래서 결론은 멀티쓰레드 환경에서 락만 걸고 일단 최선의 방법이라고 장담할 수가 없다.

언리얼 엔진 기반 학원에서 서버는 별거 없을 것 같다는 이야기를 하면서 lock만 걸면 되는 거 아니냐란 이야기를 들은 적이 있다.

하지만 당연히 안된다. 어떤 게임을 만들지에 따라 다르다.

wow와 같은 게임의 서버를 만들 때 위와 같이 쓰레드가 한 곳에 몰리는 현상을 처리할 수가 없게 된다.

![image](https://user-images.githubusercontent.com/75019048/131054713-36e071e7-ffec-4276-b8c8-fb4d0c52ca52.png)

그래서 알아볼 방법이 Thread Local Storage이다.

이 방법을 통해서 일감을 잘 나누는 방법을 한번 알아보자

식당의 예를 들어서

손님들이 한식 메뉴를 동시에 주문을 했다고 가정(한곳에 일감이 몰림).

그래서 직원들이 서빙을 할 때 반찬 하나만 들고 손님들 자리에 갖다 놓고 또 반찬 하나만 들고 자리에 갖다 놓고를 반복하면 굉장히 비효율적이다.

그래서 서빙을 할 때 보면 큰 쟁반 위에 반찬을 올려놓고 한번에 서빙을 하게 된다.

한 번에 일감을 많이 가져가서 분배를 손님 테이블에 분배를 하는 방법이 유용하게 사용이 됨

각 쓰레별로 힙 영역에 접근할 수 있는 별도의 전역 공간이 있으면 좋겠다는 아이디어에서 착안함

일감이 많이 몰려서 힙 영역에서 데이터를 하나씩 가지고 가는 것이 아니라 

한 번에 TLS로 많이 가지고 가서 쪼금씩 까먹는 방법이다.

실습 예제

```csharp
using System.Threading.Tasks;
using System.Threading;

namespace ServerCore
{
    class Program
    {
        // 그냥 전역 변수로 쓰레드 이름을 선언하면 
        // 모든 쓰레드에서 접근이 가능하고 변경 시 다른 쓰레드에도 영향을 주게 된다.
        // 따라서 TLS 영역으로 보관
        // 아래와 같이 래핑을 해서 사용을 하면 된다.
        // 쓰레드 마다 TLS에 접근을 하면 자신만의 공간에 저장이 되기 때문에 
        // 특정 쓰레드에서 쓰레드 이름을 고친다고 해도 다른 쓰레드에는 영향을 주지 않게 된다.
        // 즉, 쓰레드 마다 고유의 영역이 생겼다고 생각하면 된다.
        static ThreadLocal<string> ThreadName = new ThreadLocal<string>();

        static void WhoAmI()
        {
            ThreadName.Value = $"My Name is {Thread.CurrentThread.ManagedThreadId}";
            // 다른 쓰레드들이 이름을 고쳤을 때 영향을 주는지 확인
            Thread.Sleep(1000);
            System.Console.WriteLine(ThreadName.Value);
        }

        static void Main(string[] args)
        {
            // Parallel Library?
            // Invoke()를 사용하면 Action들 만큼 Task를 생성해서 실행시켜줌
            // 즉, ThreadPool에 있는 Thread들을 하나씩 꺼내서 사용함
            Parallel.Invoke(WhoAmI, WhoAmI, WhoAmI, WhoAmI, WhoAmI, WhoAmI);
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
        // 그냥 전역 변수로 쓰레드 이름을 선언하면 
        // 모든 쓰레드에서 접근이 가능하고 변경 시 다른 쓰레드에도 영향을 주게 된다.
        // 따라서 TLS 영역으로 보관
        // 아래와 같이 래핑을 해서 사용을 하면 된다.
        // 쓰레드 마다 TLS에 접근을 하면 자신만의 공간에 저장이 되기 때문에 
        // 특정 쓰레드에서 쓰레드 이름을 고친다고 해도 다른 쓰레드에는 영향을 주지 않게 된다.
        // 즉, 쓰레드 마다 고유의 영역이 생겼다고 생각하면 된다.
        static ThreadLocal<string> ThreadName = new ThreadLocal<string>(()=> 
        { 
            // 쓰레드가 새로 실행될 때마다 100프로 확률로 TLS를 생성하는 것이 아니라
            // 상황에 따라 쓰레드 네임의 밸류가 없을 때 생성?
            ThreadName.Value = $"My Name is {Thread.CurrentThread.ManagedThreadId}";
        });
        static void WhoAmI()
        {
            bool isRepeat = ThreadName.IsValueCreated;
            if (isRepeat)
                System.Console.WriteLine(ThreadName.Value + " (repeat)");
            else
                System.Console.WriteLine(ThreadName.Value);
            // repeat으로 출력이 되는 의미는
            // 이미 생성된 쓰레드에서 해당 일감(여기서는 WhoAmI 메서드)을 또 다시 처리한다는 의미
            // 그래서 재사용을 한다고 생각하면 됨
            // 좀더 세부적으로 
            // ThreadName.Value이 null일 때 Action이 콜백되면서 ThreadName.Value 값이 할당됨
        }
        static void Main(string[] args)
        {
            // Parallel Library?
            // Invoke()를 사용하면 Action들 만큼 Task를 생성해서 실행시켜줌
            // 즉, ThreadPool에 있는 Thread들을 하나씩 꺼내서 사용함
            Parallel.Invoke(WhoAmI, WhoAmI, WhoAmI, WhoAmI, WhoAmI, WhoAmI);
            // 모든 사용이 끝나면 폐기
            ThreadName.Dispose();
        }

        // 응용 방법?
        // 일감들이 어마어마하게 많게 큐에 저장이 되어 있다면
        // 큐에서 하나씩만 꺼내서 처리하는 것이 아니라 
        // 100개씩 한 뭉텅이씩 자신의 공간에 넣고(TLS)
        // 필요할 때마다 하나씩 꺼내서 사용하면 됨
        // 즉 JobQueue에 진입 후 lock을 건 상태에서 최대한 일감을 많이 가지고 와서
        // TLS에 저장하면 좀더 경합을 줄일 수 있어서 부하를 낮출 수 있음
        // 이런 상황이 아니라도 다양한 상황에서 사용이 됨
        // 위의 예와 같이 ThreadName이든 Thread의 고유 ID를 만들든
    }
}
```

![image](https://user-images.githubusercontent.com/75019048/131054770-d294b809-0e7c-4667-83ce-acde099cc0ba.png)
