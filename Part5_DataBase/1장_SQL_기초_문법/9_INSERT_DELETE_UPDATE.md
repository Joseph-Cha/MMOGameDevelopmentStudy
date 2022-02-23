## INSERT DELETE UPDATE

INSERT 할 때 중복되는 키 값으로 값을 넣을 수 없을 때

![image](https://user-images.githubusercontent.com/75019048/138373928-f974aa8b-d0d1-44eb-bbfd-e8bc9cdd86fb.png)

테이블 → 디자인 → Primary Key로 설정을 해놨기 때문에 중복이 안됨

```sql
USE BaseballData;

-- INSERT DELECT UPDATE

SELECT *
FROM salaries
ORDER BY yearID DESC;

-- INSERT INTO [테이블명] VALUES (값, ...)
INSERT INTO salaries
VALUES (2020, 'KOR', 'NL', 'rookiss', 90000000);

-- 데이터를 하나 빼먹으면? -> ERROR
INSERT INTO salaries
VALUES (2020, 'KOR', 'NL', 'rookiss2');

-- INSERT INTO [테이블명](열, ...) VALUES [값, ...]
INSERT INTO salaries(yearID, teamID, playerID, lgID, salary)
VALUES (2020, 'KOR', 'rookiss3', 'NL', 800000)

-- salary가 Allow NULL이기 때문에 값이 없어도 산입이 가능 
-- => 테이블 설정에 따라 모든 값을 입력하지 않아도 괜찮다.
INSERT INTO salaries(yearID, teamID, playerID, lgID)
VALUES (2020, 'KOR', 'rookiss3', 'NL')

-- DELETE FROM [테이블명] WHERE [조건]
-- DELETE문을 사용할 때는 조건을 꼭 달아줘서 모든 데이터를 삭제하지 않도록 조심해야 한다.
DELETE FROM salaries
WHERE playerID = 'rookiss3';

-- UPDATE [테이블명] SET [열 = 값, ..] WHERE [조건]
-- WHERE를 넣지 않으면 모든 값이 다 바뀌게 된다.
-- SET에서 =은 equal이 아니라 산입이다.
UPDATE salaries
SET salary = salary * 2, yearID = yearID + 1
WHERE teamID = 'KOR';

DELETE FROM salaries
WHERE yearID = 2021; 
```
