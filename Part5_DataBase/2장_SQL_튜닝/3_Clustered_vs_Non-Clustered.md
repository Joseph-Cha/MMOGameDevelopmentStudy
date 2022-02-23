## Clustered vs Non-Clustered

```sql
USE Northwind;

-- Clustered(영한 사전) vs Non-Clustered(색인)

-- Clustered
		-- Leaf Page = Data Page
		-- 즉 잎사귀(트리 구조 속 가장 하단에) 실제 데이터가 들어가 있다.
		-- 데이터는 Clustered Index 키 순서로 정렬

-- Non-Clustered ? (사실 Clustered Index 유무에 따라서 다르게 동작)
-- 1) Clustered Index가 없는 경우
		-- Clustered Index가 없으면 데이터는 Heap Table이라는 곳에 저장
		-- Heap RID -> Heap Table에 접근 데이터 추출
-- 1) Clustered Index가 있는 경우
		-- Heap Table이 없음. Leaf Table에 실제 데이터가 있다.
		-- Clustered Index의 실제 키 값을 들고 있다.
		-- 해당 키 값으로 데이터를 서치함

-- 임시 테스트 테이블을 만들고 데이터 복사
SELECT *
INTO TestOrderDetails
FROM [Order Details];

SELECT *
FROM TestOrderDetails;

-- Non-Clustered인덱스 추가
CREATE INDEX Index_OrderDetails
ON TestOrderDetails(OrderID, ProductID);

-- 인덱스 정보
EXEC sp_helpindex 'TestOrderDetails';

-- 인덱스 번호 찾기
SELECT index_id, name
FROM sys.indexes
WHERE object_id = object_id('TestOrderDetails');

-- 조회
-- PageType 1(DATA PAGE) 2(INDEX PAGE)
DBCC IND('Northwind', 'TestOrderDetails', 2);

--					896
-- 824 848 849 850 851 852
-- Heap RID ([페이지 주소(4)][파일ID(2)][슬롯(2)] Row)
-- Heap Table [{Page} {Page} {Page} {Page}]
DBCC PAGE('Northwind', 1, 824, 3);

-- Clustered 인덱스 추가
CREATE CLUSTERED INDEX Index_OrderDetails_Clustered
ON TestOrderDetails(OrderID);

-- 이렇게 Clustered 인덱스를 추가하면 기존에 Non-Clustered 인덱스에 변화가 오는데
-- PageID가 우선 변한다.
-- 왜냐하면 애당초 새로운 정렬 방식 즉 Clustered 인덱스를 쓰면 물리적으로 위치가 변하기 때문에
-- 기존에 있던 Page 정보들도 전부 변하게 된다.

DBCC PAGE('Northwind', 1, 936, 3);

-- 우선 Heap RID가 사라졌다.
-- UNIQUIFIER ?
-- PRIMARY KEY와 다르게 Clustered Index는 같은 동일한 값에도 Clustered를 걸 수 있다.
-- 즉, OrderID에다가 Clustered를 걸었는데 OrderID가 동일한 상황일 때도 내부적으로는
-- 값을 구분을 해줘야 해서 UNIQUIFIER를 통해 값을 구분해준다.

-- 조회
-- PageType 1 = DATA PAGE, 진짜 데이터를 담고 있는 페이지
-- PageType 2 = INDEX PAGE, 길을 안내해주는 페이지
DBCC IND('Northwind', 'TestOrderDetails', 1);

--									928
-- 872 888 889 890 891 892 893 894 895 904
```