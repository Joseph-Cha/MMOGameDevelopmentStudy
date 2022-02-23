## Hash 조인

```csharp
using System;
using System.Collections.Generic;

class Program
{
    class Player
    {
        public int playerId;
    }
    class Salary
    {
        public int playerId;
    }

    // 41 % 10 = 1 (Key)
    // 51 % 10 = 1 (Key)
    // 41? -> 1 (Key) 
    // HashTable [0 List] [1 List] [2] [3] [4] [5] [6] [7] [8] [9]
    // 공간을 내주고, 속도를 얻는다

    // 동일한 값 => 동일한 Bucket(Key) (YES)
    // 동일한 Bucket(Key) => 동일한 값 (NO)

    class HashTable
    {
        int _bucketCount;
        List<int>[] _buckect;

        public HashTable(int bucketCount = 100)
        {
            _bucketCount = bucketCount;
            _buckect = new List<int>[bucketCount];
            for (int i = 0; i < bucketCount; i++)
                _buckect[i] = new List<int>();
        }

        public void Add(int value)
        {
            int key = value % _bucketCount; // Hash 공식(매우 간단 버전)
            _buckect[key].Add(value);
        }

        public bool Find(int value)
        {
            int key = value % _bucketCount;
            return _buckect[key].Contains(value);
        }
    }

    static void Main()
    {
        Random rand = new Random();
        
        List<Player> players = new List<Player>();
        for (int i = 0; i < 1000; i++)
        {
            if (rand.Next(0, 2) == 0)
                continue;
            players.Add(new Player() { playerId = i });
        }

        List<Salary> salaries = new List<Salary>();
        for (int i = 0; i < 1000; i++)
        {
            if (rand.Next(0, 2) == 0)
                continue;
            salaries.Add(new Salary() { playerId = i });
        }

        // Temp HashTable
        // 임시 해쉬 테이블을 만들어서 빨리 서치를 할 수 있도록 수정
        // Dictionary<int, Salary> hash = new Dictionary<int, Salary>();
        HashTable hash = new HashTable();
        foreach(Salary s in salaries)
            hash.Add(s.playerId);
        
        List<int> result = new List<int>();
        foreach(Player p in players)
        {
            if (hash.Find(p.playerId))
                result.Add(p.playerId);
        }
    }
}
```

![image](https://user-images.githubusercontent.com/75019048/138374260-8b38717e-fca6-4726-b994-ee7a023fa109.png)

```sql
USE Northwind;

-- Hash(해시) 조인

SELECT * INTO TestOrders FROM Orders;
SELECT * INTO TestCutomers FROM Customers;

SELECT * FROM TestOrders; -- 830
SELECT * FROM TestCutomers; -- 91

-- HASH
SELECT *
FROM TestOrders AS o
		INNER JOIN TestCutomers AS c
		ON o.CustomerID = c.CustomerID;

-- NL (inner 테이블에 인덱스가 없다)
SELECT *
FROM TestOrders AS o
		INNER JOIN TestCutomers AS c
		ON o.CustomerID = c.CustomerID
		OPTION (FORCE ORDER, LOOP JOIN);

-- Merge (outer, inner 모두 sort => many-to-many)
SELECT *
FROM TestOrders AS o
		INNER JOIN TestCutomers AS c
		ON o.CustomerID = c.CustomerID
		OPTION (FORCE ORDER, MERGE JOIN);

-- HASH
-- HASH 테이블 생성 기준? => 작은 테이블 or 큰 테이블
-- 작은 테이블!
-- 우선 Hash 테이블을 생성하는 부화도 만만치 않기 때문
SELECT *
FROM TestOrders AS o
		INNER JOIN TestCutomers AS c
		ON o.CustomerID = c.CustomerID;
-- 돌아가는 로직
-- TestCustomers를 기준으로 임시 hash table을 생성한 다음에
-- 해당 임시 테이블에서 TestOrders의 값이 있는지를 찾아서 합치는 과정으로 동작한다.

-- 오늘의 결론 --
-- Hash 조인
-- 1) 정렬이 필요하지 않다 -> 데이터가 너무 많아서 Merge가 부담스러울 때, Hash가 대안이 될 수 있음
-- 2) 인덱스 유무에 영향을 받지 않는다 (매우 중요!!!)
		-- NL/Merge에 비해 확실한 장점!!
		-- HashTable 만드는 비용을 무시하면 안 됨 (수행 빈도가 많은 쿼리라면 -> 결국 Index가 필요)
		-- 가끔 사용하는 거면 Index가 없어도 효율적으로 동작
-- 3) 랜덤 엑세스 위주로 수행되지 않는다
-- 4) 데이터가 적은 쪽을 HashTable로 만드는 것이 유리하다!
```
