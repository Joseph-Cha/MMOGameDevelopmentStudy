## TRANSACTION

규모가 있는 게임 회사에서 주로 사용한다.

```sql
USE GameDB;

SELECT *
FROM accounts;

DELETE accounts;

-- BEGIN TRAN;
-- COMMIT;
-- ROLLBACK;

-- 메일 BEGIN TRAN
	-- 보낼 것인가 COMMIT
	-- 취소할 것인가 ROLLBACK

-- TRANSACTION이란?
-- DB를 다룰 때 분산을 시켜서 데이터를 관리를 하고 있는데
-- 상황에 따라 하나의 테이블만 갱신을 하는 것이 아니라 2개의 테이블을 동시에 갱신을 할 수도 있다.

-- ex)
-- 두 플레이어가 거래
-- A의 인벤토리에서 아이템 제거 => 실패
-- B의 인벤토리에다가 아이템 추가 => 추가
-- A의 골드 감소
-- 이렇게 되면 아이템 복사가 되는 상황

-- 강화
-- 강화 주문서 제거 => 성공
-- 강화 +1 => 실패 

-- 원자성, 즉 모든 것이 한번에 작업이 되어야 한다.
-- ALL or NOTHING 둘중 하나만 성공
-- 이런 상황을 해결하기 위한 방법이 TRANSACTION이다.

-- TRAN 명시하지 않으면, 자동으로 COMMIT 
INSERT INTO accounts VALUES(1, 'rookiss', 100, GETUTCDATE());

-- 성공/실패 여부에 따라 COMMIT (= COMMIT을 수동으로 하겠다)
BEGIN TRAN
	INSERT INTO accounts VALUES(2, 'rookiss2', 100, GETUTCDATE())
ROLLBACK; -- 적용 x

BEGIN TRAN
	INSERT INTO accounts VALUES(2, 'rookiss2', 100, GETUTCDATE())
COMMIT; -- 적용 o

-- 응용
BEGIN TRY
		BEGIN TRAN
				INSERT INTO accounts VALUES(1, 'rookiss', 100, GETUTCDATE())
				INSERT INTO accounts VALUES(3, 'rookiss3', 100, GETUTCDATE())
		COMMIT
END TRY
BEGIN CATCH
		IF @@TRANCOUNT > 0 -- 현재 활성화된 트랜잭션 수를 반환
				ROLLBACK
		print('ROLLBACK 했음')
END CATCH

-- 언제 실패?
-- 연결에 실패했다거나 구문에 문제가 있다던가

-- 대부분 테이블이 분리되어 있는 상황에서 동시에 갱신을 해야하는 상황에서 많이 쓰인다.

-- TRAN 사용할 때 주의할 사항
-- TRAN 안에는 꼭 !! 원자적으로 실행될 애들만 넣자(한번에 실행)
-- 성능적으로 문제가 있기 때문이다.
-- C# List<Player> List<Salary> 원자적으로 수정 
-- -> 둘다 동시에 실행이 될 때 둘다 성공하거나 둘다 실패해야 함
-- -> 일반적으로는 lock을 잡고 실행 -> writelock (상호배타적인 락) readlock(공유 락)
-- 결국에는 파일에 접근해서 한번에 무엇인가를 수정을 해야 한다면 남들은 접근을 못하도록 막아야 한다.
-- 동시 다발적으로 무엇인가가 수정을 해야하는 데 TRAN를 잡아놓게 되면 힘든 일이 될 것이다.
-- TRAN이 일종의 lock과 같은 개념

BEGIN TRAN
		INSERT INTO accounts VALUES(1, 'rookiss1', 100, GETUTCDATE())

ROLLBACK;
```

실험

![image](https://user-images.githubusercontent.com/75019048/138374032-5f1e67b5-19a2-4fe8-9ad8-3dfc0316ae59.png)

![image](https://user-images.githubusercontent.com/75019048/138374041-f8e29216-19c5-4aea-aebd-270114e14dc3.png)

위와 같이 TRAN만 잡아 놓고 다른 곳에서 account를 접근하는 쿼리를 날리면 아래와 같이 계속 작업 중인 표시가 뜬다.

마치 lock을 잡고 안풀어줬을 때와 같은 상황

![image](https://user-images.githubusercontent.com/75019048/138374053-a762ab34-bd32-4ac4-9870-389c5d90854e.png)

저러고 있을 대 ROLLBACK을 실행을 해주면 다른 곳에서 account를 접근하는 쿼리가 정상적으로 완료가 된다.

따라서 TRAN을 잡고 처리하는 작업이 매우 길면 성능적으로 많은 문제를 일으킬 수가 있게 된다.

결론

동시다발적으로 DB에 접근을 해야하는 상황일 때 TRAN을 생각해보자
