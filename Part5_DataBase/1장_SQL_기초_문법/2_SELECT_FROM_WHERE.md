## SELECT FROM WHERE

질의를 하는 가장 간단한 방법은

 Select를 하는 것이다.

Select가 중요하긴 하지만 어디서 가져와야하는지도 알아야 한다.

가장 기본 형태는

SELECT *
FROM

(* ⇒ 모든 열의 정보를 가지고 옴)

```sql
SELECT nameFirst, nameLast, birthYear
FROM players
WHERE birthYear = 1866
```

SELECT ⇒ 원하는 열의 정보를 가지고 올 수 있다.

AS ⇒ nameFirst의 별명

WHERE ⇒ 조건(if 문)

- = 동일
- ! = 다름

SELECT ⇒ FROM ⇒ WHERE 순으로 신행해야 한다.

로직으로 생각해보면

FROM 책상에서

WHERE 빨간색 물체를

SELECT 가지고 와주세요

SELECT 구문의 함정 

나중에 온갖 정보가 다 추가 조건으로 붙을 텐데 여기서 중요한 것은 SQL 구문은 영어로 만들었다는 것을 염두해야 한다.

우리가 C#코드나 다른 코드를 보면 맨처음부터 순서대로 실행을 했다.

SQL의 로직은 그렇지 않다.

한국어로 생각을 해보면

어떤 공간에서 어떤 물체를 가지고 와주세요

~에서 가 먼저고 ~을 가지고 가 다음이지만

SQL에서는 영어와 비슷하게 동사가 먼저 오고 그 다음에 장소가 나온다.

하지만 실제로 SQL을 분석을 할 때는 반대로 분석을 하는 것이 더 간편하다.

즉 FROM ⇒ SELECT순으로 분석하는 게 좀 더 좋다.

구문이 끝날 때 ;(세미콜론)을 붙여주자

특정 구문을 드래그를 한 후 F5를 누르면 해당 구문만 실행이 된다.

```sql
SELECT nameFirst, nameLast, birthYear, birthCountry
FROM players
WHERE birthYear = 1974 AND birthCountry != 'USA'
```

1974년 생**이고** 미국에서 태어나지 않는 사람의 정보

```sql
 SELECT nameFirst, nameLast, birthYear, birthCountry
 FROM players
 WHERE birthYear = 1974 OR birthCountry != 'USA'
```

1974년 생**이거나** 미국에서 태어나지 않는 사람의 정보

```sql
 SELECT nameFirst, nameLast, birthYear, birthCountry
 FROM players
 WHERE birthYear = 1974 OR (birthCountry != 'USA' AND weight > 185);
```

AND 문의 우선순위가 OR문 보다 높다 ⇒ 이렇게 괄호로 깜싸주는 것과 동일

NULL의 의미?

`없다`는 의미

Table Design에 들어가면 해당 여부를 체크 할 수 있다.

```sql
SELECT nameFirst, nameLast, birthYear, birthCountry
FROM players
WHERE deathYear IS NULL;
```

NULL 값 체크 ⇒ IS, IS NOT

```sql
SELECT birthCity
FROM players
WHERE birthCity LIKE 'NEW%';

SELECT birthCity
FROM players
WHERE birthCity LIKE 'NEW_';

SELECT birthCity
FROM players
WHERE birthCity LIKE 'NEW Yor_';
```

LIKE : 패턴 매칭

패턴에는 크게 2가지를 활용한다

% : 조커 카드 느낌? 임의의 문자열 모두 가능

_ : 임의의 문자 딱 한가지만 가능

캐릭터를 삭제 했을 때 

임시로 특정 이름으로 (Ex. 이름 앞에 Delect를 붙여준다) 바꿔주고 나중에 LIKE를 활용해서 Delect가 포함된 이름을 검색하는 방식으로 사용하게 된다.