## CASE

```sql
USE BaseballData;

-- 맨 끝에 어느 계절에 태어났는지 추가
-- 태어난 월에 따라 계절이 정해진다고 가정
SELECT *,
		CASE birthMonth
				WHEN 1 THEN N'겨울'
				WHEN 2 THEN N'겨울'
				WHEN 3 THEN N'봄'
				WHEN 4 THEN N'봄'
				WHEN 5 THEN N'봄'
				WHEN 6 THEN N'여름'
				WHEN 7 THEN N'여름'
				WHEN 8 THEN N'여름'
				WHEN 9 THEN N'가을'
				WHEN 10 THEN N'가을'
				WHEN 11 THEN N'가을'
				WHEN 12 THEN N'겨울'
				ELSE N'몰라요'
		END AS birthSeason
FROM players;
```

굳이 SELECT 문 말고도 다양하게 사용 할 수 있다.

```sql
SELECT *,
		CASE
				WHEN birthMonth <= 2 THEN N'겨울'
				WHEN birthMonth <= 5 THEN N'봄'
				WHEN birthMonth <= 8 THEN N'여름'
				WHEN birthMonth <= 11 THEN N'가을'
				ELSE N'겨울'
		END AS birthSeason
FROM players;
```

이렇게 조건을 걸어줘서 만들 수 있다.

주의 사항

ELSE문이 없을 때 포함되는 값이 안들어오면 birthSeason이 NULL이 된다. 

따라서 안전 빵으로 ELSE문을 넣어주는 게 좋다.

NULL 체크 ⇒ 실제로 해본 적은 없음

```sql
SELECT *,
		CASE
				WHEN birthMonth <= 2 THEN N'겨울'
				WHEN birthMonth <= 5 THEN N'봄'
				WHEN birthMonth <= 8 THEN N'여름'
				WHEN birthMonth <= 11 THEN N'가을'
				WHEN birthMonth IS NULL THEN N'몰라요'
				ELSE N'겨울'
		END AS birthSeason
FROM players;
```