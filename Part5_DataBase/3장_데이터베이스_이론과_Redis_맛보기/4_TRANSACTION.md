## TRANSACTION

생각보다 매우 대단한(?) 작업이기 때문에 어떻게 동작하는지 간단하게 집고 넘어가보자

TRANSACTION의 ACID 특성

1) A (Atomicity)

원자성

ALL or Nothing = 애매하게 반만 되는 경우는 없다.

2) C (Consistency)

일관성

데이터 간의 일관성 보장 (ex. 데이터와 인덱스 간 불일치 등)

3) I (Isolation)

고립성

트랜잭션을 단독으로 실행하나, 다른 트랜잭션과 함께 실행하나 똑같다.

4) D (Durability)

지속성

장애가 발생하더라도 데이터는 반드시 복구 가능

결론

1) 실제 데이터를 바로 하드디스크에 반영하지 않고 로그를 이용한다.

2) 로그에 있는 두가지 기능 

⇒ REDO(Before → After) : 아직 하드에 반영이 안되어 있다면 저장된 로그를 통해 업뎃

⇒ Undo(After → Before) : 상황에 따라 과거로 돌아감

3) 미래로 간다 (Roll Forward)

4) 과거로 간다 (Roll Back)

**5) 따라서 데이터 베이스에 장애가 발생하더라도 로그를 이용해 복부/롤백**