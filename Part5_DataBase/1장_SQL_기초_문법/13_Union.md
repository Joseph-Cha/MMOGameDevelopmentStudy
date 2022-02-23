## Union

```sql
USE BaseballData;

-- RDBMS (Relational 관계형)
-- 데이터를 집합으로 간주하다
-- 즉 테이블끼리 연관이 되어 있다.

-- 복수의 테이블을 다루는 방법
-- A, B집합의 합, 교집합, 차를 배워보자

-- 커리어 평균 연봉이 3000000 이상인 선수들의 playerID
SELECT playerID, AVG(salary)
FROM salaries
GROUP BY playerID
HAVING AVG(salary) >= 3000000

-- 12월에 태어난 선수들의 playerID
SELECT playerID, birthMonth
FROM players
WHERE birthMonth = 12;

-- [커리어 평균 연봉이 3000000 이상] || [12월에 태어난 선수들의 playerID]
-- UNION (중복 제거)
SELECT playerID
FROM salaries
GROUP BY playerID
HAVING AVG(salary) >= 3000000
UNION
SELECT playerID
FROM players
WHERE birthMonth = 12;

-- 합칠 때 열의 정보가 어느정도 일치는 해야 한다.
-- 즉 string || int => x

-- 중복 제거의 의미
-- 따로 추출을 한 다음에 합칠 때 중복되는 데이터가 발생을 할 것이다.
-- 기본적인 UNION은 DISTINT가 포함되어 있다.
-- 만일 중복 제거를 하지 않을 때는 UNION ALL을 사용해야 한다.

-- UNION ALL (중복 허용)
SELECT playerID
FROM salaries
GROUP BY playerID
HAVING AVG(salary) >= 3000000
UNION ALL
SELECT playerID
FROM players
WHERE birthMonth = 12
ORDER BY playerID;

-- UNION을 했을 경우 ORDER BY는 가장 마지막에 써줘야 한다.

-- [커리어 평균 연봉이 3000000 이상] && [12월에 태어난 선수들의 playerID] 교집합
-- INTERSECT (교집합)
SELECT playerID
FROM salaries
GROUP BY playerID
HAVING AVG(salary) >= 3000000
INTERSECT
SELECT playerID
FROM players
WHERE birthMonth = 12
ORDER BY playerID;

-- 사후 처리 즉 문제가 발생했을 때 특정 조건이 붙어서 보상을 지급을 해야 하는데
-- 이때 UNION ALL을 사용하면 중복이 허용되어서 2번 보상을 지급하는 문제가 발생하게 된다.

-- [커리어 평균 연봉이 3000000 이상] - [12월에 태어난 선수들의 playerID]
-- [커리어 평균 연봉이 3000000 이상]이지만 [12월에 태어난 선수]는 제외
-- EXCEPT (차집합)
SELECT playerID
FROM salaries
GROUP BY playerID
HAVING AVG(salary) >= 3000000
EXCEPT
SELECT playerID
FROM players
WHERE birthMonth = 12
ORDER BY playerID;
```