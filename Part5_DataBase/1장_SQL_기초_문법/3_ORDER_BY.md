## ORDER BY

정렬에 대해서

```sql
SELECT *
FROM players
WHERE birthYear IS NOT NULL
ORDER BY birthYear ASC;
```

그냥 ORDER BY를 하면 NULL도 포함해서 뜨기 때문에 IS NOT NULL을 추가해줘야 한다.

ASC : 작은 것부터 큰거 ⇒ 오름차순(생략 가능)

```sql
SELECT *
FROM players
WHERE birthYear IS NOT NULL
ORDER BY birthYear DESC;
```

DESC : 큰거부터 작은거 ⇒ 내림차순

```sql
SELECT *
FROM players
WHERE birthYear IS NOT NULL
ORDER BY birthYear DESC, birthMonth DESC, birthDay DESC;
```

1차 birthYear  ⇒ 2차(1차가 같으면) birthMonth ⇒ 3차(2차가 같으면) birthDay 

정렬은 랭킹 시스템에서 사용

이렇게만 하면 좀 아쉬운데(왜냐하면 모든 정보를 다 출력하기 때문

랭킹 시스템은 TOP 100 정도만 제한된 인원수만 출력

즉 Sort한 후에 필터를 둬서 몇명을 한정할지를 지정해보자

한정을 하는 것은 SQL이 아니라 T-SQL에서 제공해주는 기능을 활용

괄호( )는 생략 가능

```sql
SELECT TOP (10) * 
FROM players
WHERE birthYear IS NOT NULL
ORDER BY birthYear DESC, birthMonth DESC, birthDay DESC;
```

TOP 10 ⇒ 10명만 뽑기

```sql
SELECT TOP 1 PERCENT * 
FROM players
WHERE birthYear IS NOT NULL
ORDER BY birthYear DESC, birthMonth DESC, birthDay DESC;
```

1퍼센트만 뽑기

TOP의 단점 

T-SQL에서만 사용 가능

좀 더 치명적인 문제는 100번부터 200번을 보여주는 방법은 마땅히 없다 ⇒ 중간에 건너뛰는 방법 다르다

```sql
USE BaseballData;

-- 100 ~ 200
SELECT *
FROM players
WHERE birthYear IS NOT NULL
ORDER BY birthYear DESC, birthMonth DESC, birthDay DESC
OFFSET 100 ROWS FETCH NEXT 100 ROWS ONLY;
```

OFFSET 100 ROWS FETCH NEXT 100 ROWS ONLY; ⇒ SQL 정식 문법

OFFSET 100 ROWS : 100개의 행들은 건너뛰어달라

FETCH NEXT 100 ROWS ONLY : 다음 100개의 행들만 추출해달라

실제로 사용할 일은 거의 없다.