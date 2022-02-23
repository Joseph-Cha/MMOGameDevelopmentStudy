## Join

데이터베이스에서 주로 받는 질문이 INDEX, Join에 대한 질문을 많이 받는다.

 CROSS JOIN

```sql
-- Join (결합)
-- 말 그대로 정보를 결합하는 것
-- 정보가 분산이 되어 있기 때문에 결합하는 개념도 많이 사용하게 된다.

USE GameDB;

CREATE TABLE testA
(
		a INTEGER
)
CREATE TABLE testB
(
		b VARCHAR(10)
)

-- A(1, 2, 3)
INSERT INTO testA VALUES(1);
INSERT INTO testA VALUES(2);
INSERT INTO testA VALUES(3);

INSERT INTO testB VALUES('A');
INSERT INTO testB VALUES('B');
INSERT INTO testB VALUES('C');

-- CROSS JOIN (교차 결합)
SELECT *
FROM testA
		CROSS JOIN testB

SELECT *
FROM testA,testB;
```

- 교차 결합 결과
    
![image](https://user-images.githubusercontent.com/75019048/138373982-01b8d682-2bf7-4337-8eb0-4c10ffb818ad.png)    

INNER JOIN

```sql
USE BaseballData;

SELECT *
FROM players
ORDER BY playerID;
SELECT *
FROM salaries
ORDER BY playerID;

-- 가장 자주 사용하고 중요하고 효율이 좋은 기능
-- INNER JOIN (두 개의 테이블을 가로로 결합 + 결합 기준을 ON으로)
-- 열 데이터의 이름이 겹치기 때문에 AS를 활용해준다.
-- playerID가 players, salaries 양쪽에 다 있고 일치하는 애들을 결합
SELECT *
FROM players AS p
		INNER JOIN salaries AS s
		ON p.playerID = s.playerID;

-- playerID가 한쪽에만 있다면 데이터가 추출이 되지 않는다.

-- OUTER JOIN (외부 결합)
		-- LEFT / RIGHT
		-- 어느 한쪽에만 존재하는 데이터 -> 정책?

-- LEFT JOIN (두 개의 테이블을 가로로 결합 + 결합 기준을 ON으로)
-- + 한쪽에만 있을 경우
-- playerID가 왼쪽(players)에 있으면 무조건 표시, 오른쪽(salaries)에 없으면 오른쪽 정보는 NULL로 채움
SELECT *
FROM players AS p
		LEFT JOIN salaries AS s
		ON p.playerID = s.playerID;

-- RIGHT JOIN (두 개의 테이블을 가로로 결합 + 결합기준을 ON으로)
-- + 한쪽에만 있을 경우
-- playerID가 오른쪽(salaries)에 있으면 무조건 표시, 왼쪽(players)에 없으면 왼쪽 정보는 NULL로 채움
SELECT *
FROM players AS p
		RIGHT JOIN salaries AS s
		ON p.playerID = s.playerID;
```
