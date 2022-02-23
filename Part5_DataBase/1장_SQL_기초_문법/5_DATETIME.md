## DATETIME

```sql
-- 문자열을 시간으로 변경
SELECT CAST('20200425 05:03' AS DATETIME)
SELECT CAST('20200425 05:03:22' AS DATETIME)

-- 현재 시간
-- T-SQL
SELECT GETDATE();
-- 범용적
SELECT CURRENT_TIMESTAMP
```

산입

![image](https://user-images.githubusercontent.com/75019048/138373825-04f545e5-ec7e-4c03-be08-9d441f569a62.png)

시간 산입 

```sql
USE [BaseballData]
GO

INSERT INTO [dbo].[DateTimeTest]
           ([time])
     VALUES
           ('20200425 05:03' AS DATETIME)
GO
```

```sql
USE [BaseballData]
GO

INSERT INTO [dbo].[DateTimeTest]
           ([time])
     VALUES
           (CAST('20200425 05:03' AS DATETIME))
GO
```

```sql
USE [BaseballData]
GO

INSERT INTO [dbo].[DateTimeTest]
           ([time])
     VALUES
           (CURRENT_TIMESTAMP)
GO
```

시간 비교

```sql
SELECT *
FROM dateTimeTest
WHERE time >='20100101 05:03'
--WHERE time >=CAST('20100101 05:03' AS DATETIME)
```

 

표준 시간

```sql
-- 해외에서 서비스 할 것을 대비해서 아싸리 표준 시간을 저장
-- 현재의 UTC 시간 => GMT 표준 시계
SELECT GETUTCDATE();
```

기타 연산

```sql
-- 시간과 관련한 연산
	SELECT DATEADD(YEAR, 1, '20200426')
	SELECT DATEADD(DAY, 5, '20200426')
	SELECT DATEADD(SECOND, 123123, '20200426')

	-- 과거로
	SELECT DATEADD(SECOND, -123123, '20200426')
	
	-- 시간 차이
	-- 오른쪽에서 왼쪽을 뺀다
	SELECT DATEDIFF(SECOND, '20200826', '20200503')

	-- 특정 시간에서 연, 월, 일 중 하나를 뽑아올 때 사용
	SELECT DATEPART(DAY, '20200826')
	SELECT YEAR('20200826')
	SELECT MONTH('20200826')
	SELECT DAY('20200826')
```
