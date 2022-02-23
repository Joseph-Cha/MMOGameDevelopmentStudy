## Nested Loop 조인

RDBMS의 특성상 다양한 데이터 테이블이 서로 연결되어서 데이터가 구성이 되게 된다.

MMORPG에서보면 player, account, 아이템 정보 등 처럼 분산이 되어 있을 것이다.

```csharp
using System;
using System.Collections.Generic;

class Program
{
    class Player
    {
        public int playerId;
        // ...
    }
    class Salary
    {
        public int playerId;
        // ...
    }
    static void Main()
    {
        Random rand = new Random();

        // N
        List<Player> players = new List<Player>();
        for (int i = 0; i < 1000; i++)
        {
            if (rand.Next(0,2) == 0)
                continue;
            players.Add(new Player() { playerId = 1} );
        }

        // N
        // List<Salary> salaries = new List<Salary>();
        Dictionary<int, Salary> salaries = new Dictionary<int, Salary>();
        for (int i = 0; i < 1000; i++)
        {
            if (rand.Next(0,2) == 0)
                continue;
            salaries.Add(i, new Salary() { playerId = 1} );
        }

        // Q) ID가 players에도 있고, salaries에도 있는 정보 추출?

        // Nested(내포하는) Loop(루푸) -> 중첩 루프
        // 하나의 테이블을 돌면서 다른 테이블의 값을 찾는 과정
        List<int> result = new List<int>();

        // List 방식 => O(N^2)
        // Dic 방식 => O(N) Dic는 해쉬 방식이기 때문에
        // result를 그냥 아무거나 최대 5개만 찾아줘!
        foreach(Player p in players)
        {
            // foreach(Salary s in salaries)
            // {
            //     if (s.playerId == p.playerId)
            //     {
            //         result.Add(p.playerId);
            //         break;
            //     }
            // }
            Salary s = null;
            if (salaries.TryGetValue(p.playerId, out s))
            {
                result.Add(p.playerId);
                if (result.Count == 5)  // 이렇게 원하는 데이터 갯수를 정해주면 훨씬 더 빠르게 찾을 수 있다.
                    break;
            }
        }

        // 여기서 알 수 있는 사실
        // 외부(Player)의 데이터가 어떻게 되어 있는지는 중요하지 않지만
        // 내부(Salary)의 데이터는 어떻게 되어 있는지가 중요하게 된다.
    }
}
```

NL는 위와 같이 이중 For문을 도는 느낌으로 외부에서 내부로 하나씩 서치를 하는 느낌으로 동작하고 내부에 있는 테이블을 빠르게 서치할 수 있도록 해주면 성능이 향상된다.

더 나아가 갯수 제한이 있으면 훨씬 더 유용한 방법이다.

```sql
SELECT TOP 5 *
FROM players AS p
		INNER JOIN salaries AS s
		ON p.playerID = s.playerID;
```

![image](https://user-images.githubusercontent.com/75019048/138374150-6ab20f15-8c0b-4cf4-95af-c5ac5568db9b.png)

Clustered Scan이 처음으로 뜨는 이유는 위와 같이 외부에서 먼저 값을 하나씩 조회해 나아가기 때문이다(여기서는 salaries에 해당)

그리고 Index Seek가 뜨는데 위와 같이 내부는 Dic로 조회를 해서 성능을 높였던 것처럼 여기서도 Index Seek를 통해 내부는 좀더 빠르게 값을 찾을 수 있는 방식으로 동작을 하게 된다.

NL을 왼쪽편에서 한번 더 해주고 있는데 그 이유는 더이상 테이블을 참조한 것이 아니라 Key Lookup을 했다. 왜냐면 Clustered의 경우 Leaf에 진짜 데이터가 들어가 있었지만 NonClustered의 경우는 key의 값을 가지고 데이터를 찾아야 하기 때문에 Key Lookup을 해주게 된 것이다.

즉 salaries에 대한 정보(Clustered)는 모두 추출 했지만 players에 대한 정보(NonClustered)는 아직 추출이 다 끝나지 않았다.

여기서 신기한 점은

```sql
SELECT TOP 5 *
FROM players AS p
		INNER JOIN salaries AS s
		ON p.playerID = s.playerID
		OPTION(LOOP JOIN);
```

위와 같이 player를 먼저하고 INNER JOIN으로 salaries를 추가 했음에도 salaries를 먼저 서치하고(외부) players를 서치한 점이다(내부).

DB차원에서 이렇게 하는 것이 더 빠르게 동작하니깐 알아서 해준 것인데 이것 또한 순서를 강제시킬 수가 있다. ⇒ `FORCE ORDER`

```sql
SELECT TOP 5 *
FROM players AS p
		INNER JOIN salaries AS s
		ON p.playerID = s.playerID
		OPTION(FORCE ORDER, LOOP JOIN);
```

```sql
SELECT TOP 5 *
FROM players AS p
		INNER JOIN salaries AS s
		ON p.playerID = s.playerID;
```

이 방식에서는 왜 NL이 뜬걸까?

답은 바로 TOP 5에 있다.

위에서 언급 했듯이 원하는 데이터 개수가 정해져 있을 때 NL 전략이 우수하게 동작을 하게 된다.

⇒ 이런 방식을 `부분 범위`라고 한다.

### 오늘의 결론

NL의 특징

먼저 엑세스한 (OUTER) 테이블의 로우를 하나씩 서치를 하면서 ⇒ (INNER) 테이블에 랜덤 엑세스를 하게 된다.

INNER 테이블에 인덱스가 없으면 노답이다.

부분 범위 처리에 좋다.
