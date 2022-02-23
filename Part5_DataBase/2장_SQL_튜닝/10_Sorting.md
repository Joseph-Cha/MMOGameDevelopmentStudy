## Sorting

```sql
USE BaseballData;

-- 1) 생략
-- 2) ORDER BY

-- 정렬이 되는 원인) ORDER BY 순서대로 정렬을 해야 하니깐
SELECT *
FROM players
ORDER BY college;

-- INDEX가 잡혀 있는 순서대로 정렬을 하면 실제로는 Sorting을 하지 않는다
SELECT *
FROM batting
ORDER BY playerID, yearID; 

-- 3) GROUP BY
-- 원인) 집게를 하기 위해 정렬을 먼저 실행
SELECT college, COUNT(college)
FROM players
WHERE college LIKE 'C%'
GROUP BY college;

SELECT playerID, COUNT(playerID)
FROM players
WHERE playerID LIKE 'C%'
GROUP BY playerID; -- playerID가 INDEX로 잡혀 있기 때문에 Sorting을 안한다.

-- 4) DISTINCT
-- 원인) 중복을 제거하기 위해서 정렬
-- 정말 중복 제거가 필요 없는 경우에는 굳이 쓸 필요가 없다.
SELECT DISTINCT college
FROM players
WHERE college LIKE 'C%';

-- 5) UNION
-- 원인) 두 데이터를 합칠 때 중복된 데이터가 있을 때는 한 번만 나오게 하기 위해서 
-- => 중복 제거 DISTINCT
-- UNION의 경우 중복이 일어나지 않을 것이란 확신이 있다면(ex. 아래 경우와 같이 B, C로 시작하면 중복x)
-- UNION ALL을 사용해서 Sorting을 안할 수도 있다.
-- 즉 중복이 없다면 UNION ALL을 사용하면 성능을 향상 시킬 수 있다.
SELECT college
FROM players
WHERE college LIKE 'B%'
UNION ALL
SELECT college
FROM players
WHERE college LIKE 'C%';

-- 6) 순위 윈도우 함수
-- 원인) 집게를 하기 위해서
SELECT ROW_NUMBER() OVER (ORDER BY college)
FROM players;

SELECT ROW_NUMBER() OVER (ORDER BY playerId)
FROM players;

-- 오늘의 결론 --
-- Sorting (정렬)을 줄이자!

-- O(N * LogN) -> DB는 데이터가 어마어마하게 많다.
-- 너무 용량이 커서 가용 메모리로 커버가 안 되면 -> 디스크까지 찾아간다.
-- 따라서 디스크까지 가서 Sorting을 하게 되면 너무 느리기 때문에
-- Sorting이 언제 일어나는지 파악하고 있어야 함

-- Sorting이 일어날 때
-- 1) SORT MERGE JOIN
		-- 원인) 알고리즘 특성상 Merge하기 전에 Sort를 해야 함
-- 2) ORDER BY
-- 3) GROUP BY
-- 4) DISTINCT
-- 5) UNION
-- 6) RANKING WINDOWS FUNCTION
-- 7) MIN MAX

-- INDEX를 잘 활용하면, Sorting을 굳이 하지 않아도 된다 --
-- 또는 Sorting 자체가 정말 필요한지 고민을 해봐야 한다.
```