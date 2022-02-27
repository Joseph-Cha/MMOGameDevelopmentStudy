# C#과 유니티로 만드는 MMORPG 게임 개발 시리즈 강의 노트
Rookiss님의 MMO 게임 개발 강의 시리즈를 들으면서 직접 기록한 강의 노트입니다.

로드맵 강의 링크 : [MMORPG 게임 개발, 켠김에 끝판왕까지!](https://www.inflearn.com/roadmaps/355)

## 강의 리스트
- [[C#과 유니티로 만드는 MMORPG 게임 개발 시리즈] Part4: 게임 서버](https://github.com/Joseph-Cha/MMOGameDevelopmentLectureNote#c%EA%B3%BC-%EC%9C%A0%EB%8B%88%ED%8B%B0%EB%A1%9C-%EB%A7%8C%EB%93%9C%EB%8A%94-mmorpg-%EA%B2%8C%EC%9E%84-%EA%B0%9C%EB%B0%9C-%EC%8B%9C%EB%A6%AC%EC%A6%88-part4-%EA%B2%8C%EC%9E%84-%EC%84%9C%EB%B2%84)
- [[C#과 유니티로 만드는 MMORPG 게임 개발 시리즈] Part5: 데이터베이스](https://github.com/Joseph-Cha/MMO_GameDevelopment_LectureNote#c%EA%B3%BC-%EC%9C%A0%EB%8B%88%ED%8B%B0%EB%A1%9C-%EB%A7%8C%EB%93%9C%EB%8A%94-mmorpg-%EA%B2%8C%EC%9E%84-%EA%B0%9C%EB%B0%9C-%EC%8B%9C%EB%A6%AC%EC%A6%88-part5-%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4)
- [[C#과 유니티로 만드는 MMORPG 게임 개발 시리즈] Part7: MMO 컨텐츠 구현 (Unity + C# 서버 연동 기초)](https://github.com/Joseph-Cha/MMO_GameDevelopment_LectureNote#part7-mmo-컨텐츠-구현-unity--c-서버-연동-기초)
- [[C#과 유니티로 만드는 MMORPG 게임 개발 시리즈] Part8: Entity Framework Core](https://github.com/Joseph-Cha/MMO_GameDevelopment_LectureNote#part8-entity-framework-core)
- [[C#과 유니티로 만드는 MMORPG 게임 개발 시리즈] Part9: MMO 컨텐츠 구현 (DB연동 + 대형 구조 + 라이브 준비)](https://github.com/Joseph-Cha/MMO_GameDevelopment_LectureNote#part9-mmo-컨텐츠-구현-db연동--대형-구조--라이브-준비)

## [C#과 유니티로 만드는 MMORPG 게임 개발 시리즈] Part4: 게임 서버

### 1장. 멀티쓰레드 프로그래밍

- [1.1 서버OT](Part4_GameServer/1장_멀티쓰레드_프로그래밍/1.1_서버_OT.md)
- [1.2 멀티쓰레드 입문](Part4_GameServer/1장_멀티쓰레드_프로그래밍/1.2_멀티쓰레드_입문.md)
- [1.3 쓰레드 생성](Part4_GameServer/1장_멀티쓰레드_프로그래밍/1.3_쓰레드_생성.md)
- [1.4 컴파일러 최적화](Part4_GameServer/1장_멀티쓰레드_프로그래밍/1.4_컴파일러_최적화.md)
- [1.5 캐쉬 이론](Part4_GameServer/1장_멀티쓰레드_프로그래밍/1.5_캐쉬_이론.md)
- [1.6 메모리 배리어](Part4_GameServer/1장_멀티쓰레드_프로그래밍/1.6_메모리_배리어.md)
- [1.7 Interlocked](Part4_GameServer/1장_멀티쓰레드_프로그래밍/1.7_Interlocked.md)
- [1.8 Lock 기초](Part4_GameServer/1장_멀티쓰레드_프로그래밍/1.8_Lock_기초.md)
- [1.9 DeadLock](Part4_GameServer/1장_멀티쓰레드_프로그래밍/1.9_DeadLock.md)
- [1.10 Lock 구현이론](Part4_GameServer/1장_멀티쓰레드_프로그래밍/1.10_Lock_구현이론.md)
- [1.11 SpinLock](Part4_GameServer/1장_멀티쓰레드_프로그래밍/1.11_SpinLock.md)
- [1.12 ContextSwitching](Part4_GameServer/1장_멀티쓰레드_프로그래밍/1.12_ContextSwitching.md)
- [1.13 AutoResetEvent](Part4_GameServer/1장_멀티쓰레드_프로그래밍/1.13_AutoResetEvent.md)
- [1.14 ReaderWriterLock](Part4_GameServer/1장_멀티쓰레드_프로그래밍/1.14_ReaderWriterLock.md)
- [1.15 ReaderWriterLock 구현 연습](Part4_GameServer/1장_멀티쓰레드_프로그래밍/1.15_ReaderWriterLock_구현_연습.md)
- [1.16 Thread Local Storage](Part4_GameServer/1장_멀티쓰레드_프로그래밍/1.16_Thread_Local_Storage.md)

### 2장. 네트워크 프로그래밍

- [2.1 네트워크 기초](Part4_GameServer/2장_네트워크_프로그래밍/2.1_네트워크_기초.md)
- [2.2 통신 모델](Part4_GameServer/2장_네트워크_프로그래밍/2.2_통신_모델.md)
- [2.3 소켓 프로그래밍 입문 #1](Part4_GameServer/2장_네트워크_프로그래밍/2.3_소켓_프로그래밍_입문_1.md)
- [2.4 소켓 프로그래밍 입문 #2](Part4_GameServer/2장_네트워크_프로그래밍/2.4_소켓_프로그래밍_입문_2.md)
- [2.5 Listener](Part4_GameServer/2장_네트워크_프로그래밍/2.5_Listener.md)
- [2.6 Session #1](Part4_GameServer/2장_네트워크_프로그래밍/2.6_Session_1.md)
- [2.7 Session #2](Part4_GameServer/2장_네트워크_프로그래밍/2.7_Session_2.md)
- [2.8 Session #3](Part4_GameServer/2장_네트워크_프로그래밍/2.8_Session_3.md)
- [2.9 Session #4](Part4_GameServer/2장_네트워크_프로그래밍/2.9_Session_4.md)
- [2.10 Connector](Part4_GameServer/2장_네트워크_프로그래밍/2.10_Connector.md)
- [2.11 TCP vs UDP](Part4_GameServer/2장_네트워크_프로그래밍/2.11_TCP_vs_UDP.md)
- [2.12 RecvBuffer](Part4_GameServer/2장_네트워크_프로그래밍/2.12_RecvBuffer.md)
- [2.13 SendBuffer](Part4_GameServer/2장_네트워크_프로그래밍/2.13_SendBuffer.md)
- [2.14 PacketSession](Part4_GameServer/2장_네트워크_프로그래밍/2.14_PacketSession.md)

### 3장. 패킷 직렬화

- [3.1 Serialization #1](Part4_GameServer/3장_패킷_직렬화/3.1_Serialization_1.md)
- [3.2 Serialization #2](Part4_GameServer/3장_패킷_직렬화/3.2_Serialization_2.md)
- [3.3 UTF-8 vs UTF-16](Part4_GameServer/3장_패킷_직렬화/3.3_UTF-8_vs_UTF-16.md)
- [3.4 Serialization #3](Part4_GameServer/3장_패킷_직렬화/3.4_Serialization_3.md)
- [3.5 Serialization #4](Part4_GameServer/3장_패킷_직렬화/3.5_Serialization_4.md)
- [3.6 Packet Generator #1](Part4_GameServer/3장_패킷_직렬화/3.6_Packet_Generator_1.md)
- [3.7 Packet Generator #2](Part4_GameServer/3장_패킷_직렬화/3.7_Packet_Generator_2.md)
- [3.8 Packet Generator #3](Part4_GameServer/3장_패킷_직렬화/3.8_Packet_Generator_3.md)
- [3.9 Packet Generator #4](Part4_GameServer/3장_패킷_직렬화/3.9_Packet_Generator_4.md)

### 4장. Job Queue

- [4.1 채팅 테스트 #1](Part4_GameServer/4장_Job_Queue/4.1_채팅_테스트_1.md)
- [4.2 채팅 테스트 #2](Part4_GameServer/4장_Job_Queue/4.2_채팅_테스트_2.md)
- [4.3 커맨드 패턴](Part4_GameServer/4장_Job_Queue/4.3_커맨드_패턴.md)
- [4.4 Job Queue #1](Part4_GameServer/4장_Job_Queue/4.4_Job_Queue_1.md)
- [4.5 Job Queue #2](Part4_GameServer/4장_Job_Queue/4.5_Job_Queue_2.md)
- [4.6 패킷 모아 보내기](Part4_GameServer/4장_Job_Queue/4.6_패킷_모아_보내기.md)
- [4.7 Job Timer](Part4_GameServer/4장_Job_Queue/4.7_Job_Timer.md)

### 5장. 유니티 연동

- [5.1 유니티 연동 #1](Part4_GameServer/5장_유니티_연동/5.1_유니티_연동_1.md)
- [5.2 유니티 연동 #2](Part4_GameServer/5장_유니티_연동/5.2_유니티_연동_2.md)
- [5.3 유니티 연동 #3](Part4_GameServer/5장_유니티_연동/5.3_유니티_연동_3.md)
- [5.4 유니티 연동 #4](Part4_GameServer/5장_유니티_연동/5.4_유니티_연동_4.md)

## [C#과 유니티로 만드는 MMORPG 게임 개발 시리즈] Part5: 데이터베이스

### 1장 SQL 기초 문법

- [1.1 SSMS 입문](Part5_DataBase/1장_SQL_기초_문법/1_SSMS입문.md)
- [1.2 SELECT FROM WHERE](Part5_DataBase/1장_SQL_기초_문법/2_SELECT_FROM_WHERE.md)
- [1.3 ORDER BY](Part5_DataBase/1장_SQL_기초_문법/3_ORDER_BY.md)
- [1.4 수치와 문자열](Part5_DataBase/1장_SQL_기초_문법/4_수치와_문자열.md)
- [1.5 DATETIME](Part5_DataBase/1장_SQL_기초_문법/5_DATETIME.md)
- [1.6 CASE](Part5_DataBase/1장_SQL_기초_문법/6_CASE.md)
- [1.7 집계](Part5_DataBase/1장_SQL_기초_문법/7_집계.md)
- [1.8 연습문제](Part5_DataBase/1장_SQL_기초_문법/8_연습문제.md)
- [1.9 INSERT DELETE UPDATE](Part5_DataBase/1장_SQL_기초_문법/9_INSERT_DELETE_UPDATE.md)
- [1.10 데이터베이스 작성](Part5_DataBase/1장_SQL_기초_문법/10_데이터베이스_작성.md)
- [1.11 정규화](Part5_DataBase/1장_SQL_기초_문법/11_정규화.md)
- [1.12 인덱스](Part5_DataBase/1장_SQL_기초_문법/12_인덱스.md)
- [1.13 Union](Part5_DataBase/1장_SQL_기초_문법/13_Union.md)
- [1.14 Join](Part5_DataBase/1장_SQL_기초_문법/14_Join.md)
- [1.15 TRANSACTION](Part5_DataBase/1장_SQL_기초_문법/15_TRANSACTION.md)
- [1.16 변수와 흐름제어](Part5_DataBase/1장_SQL_기초_문법/16_변수와_흐름_제어.md)
- [1.17 윈도우 함수](Part5_DataBase/1장_SQL_기초_문법/17_윈도우_함수.md)

### 2장 SQL 튜닝

- [2.1 인덱스 분석](Part5_DataBase/2장_SQL_튜닝/1_인덱스_분석.md)
- [2.2 복합 인덱스](Part5_DataBase/2장_SQL_튜닝/2_복합_인덱스.md)
- [2.3 Clustered vs Non-Clustered](Part5_DataBase/2장_SQL_튜닝/3_Clustered_vs_Non-Clustered.md)
- [2.4 Index Scan vs Index Seek](Part5_DataBase/2장_SQL_튜닝/4_Index_Scan_vs_Index_Seek.md)
- [2.5 북마크 룩업](Part5_DataBase/2장_SQL_튜닝/5_북마크_룩업.md)
- [2.6 인덱스 칼럼 순서](Part5_DataBase/2장_SQL_튜닝/6_인덱스_칼럼_순서.md)
- [2.7 Nest Loop 조인](Part5_DataBase/2장_SQL_튜닝/7_Nested_Loop_조인.md)
- [2.8 Merge 조인](Part5_DataBase/2장_SQL_튜닝/8_Merge_조인.md)
- [2.9 Hash 조인](Part5_DataBase/2장_SQL_튜닝/9_Hash_조인.md)
- [2.10 Sorting](Part5_DataBase/2장_SQL_튜닝/10_Sorting.md)

### 3장 데이터베이스 이론과 Redis 맛보기

- [3.1 데이터베이스 원리](Part5_DataBase/3장_데이터베이스_이론과_Redis_맛보기/1_데이터베이스원리.md)
- [3.2 쓰레드와 캐시](Part5_DataBase/3장_데이터베이스_이론과_Redis_맛보기/2_쓰레드와_캐시.md)
- [3.3 대기와 락](Part5_DataBase/3장_데이터베이스_이론과_Redis_맛보기/3_대기와_락.md)
- [3.4 TRANSACTION](Part5_DataBase/3장_데이터베이스_이론과_Redis_맛보기/4_TRANSACTION.md)
- [3.5 Redis 맛보기 #1](Part5_DataBase/3장_데이터베이스_이론과_Redis_맛보기/5_Redis_맛보기_1.md)
- [3.6 Redis 맛보기 #2](Part5_DataBase/3장_데이터베이스_이론과_Redis_맛보기/6_Redis_맛보기_2.md)
- [3.7 Redis 맛보기 #3](Part5_DataBase/3장_데이터베이스_이론과_Redis_맛보기/7_Redis_맛보기_3.md)

## Part7: MMO 컨텐츠 구현 (Unity + C# 서버 연동 기초)

### 1장 서버 연동

- [1.1 멀티플레이 환경](Part7_MMOContent(Server_Connection)/1장_서버연동/1.1_멀티플레이_환경.md)
- [1.2 게임 입장](Part7_MMOContent(Server_Connection)/1장_서버연동/1.2_게임_입장.md)
- [1.3 MyPlayer 분리](Part7_MMOContent(Server_Connection)/1장_서버연동/1.3_MyPlayer_분리.md)
- [1.4 이동 동기화 #1](Part7_MMOContent(Server_Connection)/1장_서버연동/1.4_이동_동기화_1.md)
- [1.5 이동 동기화 #2](Part7_MMOContent(Server_Connection)/1장_서버연동/1.5_이동_동기화_2.md)
- [1.6 스킬 #1, #2](Part7_MMOContent(Server_Connection)/1장_서버연동/1.6_스킬_1_2.md)
- [1.7 Hit 판정](Part7_MMOContent(Server_Connection)/1장_서버연동/1.7_Hit_판정.md)
- [1.8 화살 #1](Part7_MMOContent(Server_Connection)/1장_서버연동/1.8_화살_1.md)
- [1.9 화살 #2](Part7_MMOContent(Server_Connection)/1장_서버연동/1.9_화살_2.md)
- [1.10 Data & Config](Part7_MMOContent(Server_Connection)/1장_서버연동/1.10_Data_and_Config.md)
- [1.11 스탯](Part7_MMOContent(Server_Connection)/1장_서버연동/1.11_스탯.md)
- [1.12 데미지 판정](Part7_MMOContent(Server_Connection)/1장_서버연동/1.12_데미지_판정.md)
- [1.13 Search AI](Part7_MMOContent(Server_Connection)/1장_서버연동/1.13_Search_AI.md)
- [1.14 Skill AI](Part7_MMOContent(Server_Connection)/1장_서버연동/1.14_Skill_AI.md)

### 2장 구조 개선

- [2.1 Job Queue](Part7_MMOContent(Server_Connection)/2장_구조개선/2.1_JobQueue.md)
- [2.2 Job Timer](Part7_MMOContent(Server_Connection)/2장_구조개선/2.2_JobTimer.md)
- [2.3 마무리](Part7_MMOContent(Server_Connection)/2장_구조개선/2.3_마무리.md)

## Part8: Entity Framework Core

### 1장 데이터 쿼리

- [1.1 RelationShip  기초](Part8_EntityFrameworkCore/1장_데이터_쿼리/1.1_RelationShip_기초.md)
- [1.2 관련 데이터 로딩](Part8_EntityFrameworkCore/1장_데이터_쿼리/1.2_관련_데이터_로딩.md)
- [1.3 Data Transfer Object](Part8_EntityFrameworkCore/1장_데이터_쿼리/1.3_Data_Transfer_Object.md)

### 2장 데이터 수정

- [2.1 Entity 상태 관리](Part8_EntityFrameworkCore/2장_데이터_수정/2.1_Entity_상태_관리.md)
- [2.2 Update 기초](Part8_EntityFrameworkCore/2장_데이터_수정/2.2_Update_기초.md)
- [2.3 Connected vs Disconnected](Part8_EntityFrameworkCore/2장_데이터_수정/2.3_Connected_vs_Disconnected.md)
- [2.4 Foreign Key와 Nullable](Part8_EntityFrameworkCore/2장_데이터_수정/2.4_Foreign_Key와_Nullable.md)
- [2.5 Relationship Update](Part8_EntityFrameworkCore/3장_세부설정/3.2_Relationship_Config.md)
- [2.6 Delete](Part8_EntityFrameworkCore/2장_데이터_수정/2.6_Delete.md)

### 3장 세부 설정

- [3.1 Data Modeling Config](Part8_EntityFrameworkCore/3장_세부설정/3.1_Data_Modeling_Config.md)
- [3.2 Relationship Config](Part8_EntityFrameworkCore/3장_세부설정/3.2_Relationship_Config.md)
- [3.3 Shadow Property & Backing Field](Part8_EntityFrameworkCore/3장_세부설정/3.3_Shadow_Property_and_Backing_Field.md)
- [3.4 Entity Class & Table Mapping](Part8_EntityFrameworkCore/3장_세부설정/3.4_Entity_Class_and_Table_Mapping.md)
- [3.5 Backing Field + Relationship](Part8_EntityFrameworkCore/3장_세부설정/3.5_Backing_Field_and_Relationship.md)
- [3.6 User Defined Function](Part8_EntityFrameworkCore/3장_세부설정/3.6_User_Defined_Function.md)
- [3.7 Default Value](Part8_EntityFrameworkCore/3장_세부설정/3.7_Default_Value.md)

### 4장 Migration

- [4.1 Migration #1 (Code-First)](Part8_EntityFrameworkCore/4장_Migration/4.1_Migration_1_(Code-First).md)

### 5장 DbContext

- [5.1 Entity State & Relationship #1](Part8_EntityFrameworkCore/5장_DbContext/5.1_Entity_State_and_Relationship_1.md)
- [5.2 Entity State & Relationship #2](Part8_EntityFrameworkCore/5장_DbContext/5.2_Entity_State_and_Relationship_2.md)
- [5.3 State 조작](Part8_EntityFrameworkCore/5장_DbContext/5.3_State_조작.md)
- [5.4 SQL 호출](Part8_EntityFrameworkCore/5장_DbContext/5.4_SQL_호출.md)
- [5.5 마지막 (Logging)](Part8_EntityFrameworkCore/5장_DbContext/5.5_Logging.md)

## Part9: MMO 컨텐츠 구현 (DB연동 + 대형 구조 + 라이브 준비)

### 1장 DB 연동 실습

- [1.1 DB 연동](Part9_MMOContent(DB_Connection)/1장_DB_연동_실습/1.1_DB_연동.md)
- [1.2 접속](Part9_MMOContent(DB_Connection)/1장_DB_연동_실습/1.2_접속.md)
- [1.3 플레이어 연동 #1](Part9_MMOContent(DB_Connection)/1장_DB_연동_실습/1.3_플레이어_연동_1.md)
- [1.4 플레이어 연동 #2](Part9_MMOContent(DB_Connection)/1장_DB_연동_실습/1.4_플레이어_연동_2.md)
- [1.5 HP 연동 #1](Part9_MMOContent(DB_Connection)/1장_DB_연동_실습/1.5_HP_연동_1.md)
- [1.6 HP 연동 #2](Part9_MMOContent(DB_Connection)/1장_DB_연동_실습/1.6_HP_연동_2.md)
- [1.7 아이템 #1](Part9_MMOContent(DB_Connection)/1장_DB_연동_실습/1.7_아이템_1.md)
- [1.8 아이템 #2](Part9_MMOContent(DB_Connection)/1장_DB_연동_실습/1.8_아이템_2.md)
- [1.9 Reward #1](Part9_MMOContent(DB_Connection)/1장_DB_연동_실습/1.9_Reward_1.md)
- [1.10 Reward #2](Part9_MMOContent(DB_Connection)/1장_DB_연동_실습/1.10_Reward_2.md)
- [1.11 아이템 착용 #1](Part9_MMOContent(DB_Connection)/1장_DB_연동_실습/1.11_아이템_착용_1.md)
- [1.12 아이템 착용 #2](Part9_MMOContent(DB_Connection)/1장_DB_연동_실습/1.12_아이템_착용_2.md)
- [1.13 아이템 착용 #3](Part9_MMOContent(DB_Connection)/1장_DB_연동_실습/1.13_아이템_착용_3.md)
- [1.14 DB 연동 마무리](Part9_MMOContent(DB_Connection)/1장_DB_연동_실습/1.14_DB_연동_마무리.md)

### 2장 대형 구조 관리

- [2.1 Job 예약 취소](Part9_MMOContent(DB_Connection)/2장_대형_구조_관리/2.1_Job_예약_취소.md)
- [2.2 대형 구조 관리 이론](Part9_MMOContent(DB_Connection)/2장_대형_구조_관리/2.2_대형구조_관리_이론.md)
- [2.3 게임 구조 변경](Part9_MMOContent(DB_Connection)/2장_대형_구조_관리/2.3_게임_구조_변경.md)
- [2.4 A* 개선](Part9_MMOContent(DB_Connection)/2장_대형_구조_관리/2.4_Astar_개선.md)
- [2.5 Zone #1](Part9_MMOContent(DB_Connection)/2장_대형_구조_관리/2.5_Zone_1.md)
- [2.6 Zone #2](Part9_MMOContent(DB_Connection)/2장_대형_구조_관리/2.6_Zone_2.md)
- [2.7 Zone #3](Part9_MMOContent(DB_Connection)/2장_대형_구조_관리/2.7_Zone_3.md)
- [2.8 대형 구조 마무리](Part9_MMOContent(DB_Connection)/2장_대형_구조_관리/2.8_대형_구조_마무리.md)

### 3장 더미 테스트

- [3.1 DummyClient](Part9_MMOContent(DB_Connection)/3장_더미_테스트/3.1_DummyClient_1.md)
- [3.2 패킷 모아보내기 Final](Part9_MMOContent(DB_Connection)/3장_더미_테스트/3.2_패킷_모아보내기_Final.md)

### 4장 라이브 준비

- [4.1 취약점 공격 개론](Part9_MMOContent(DB_Connection)/4장_라이브_준비/4.1_취약점_공격_개론.md)
- [4.2 게임 해킹 개론](Part9_MMOContent(DB_Connection)/4장_라이브_준비/4.2_게임_해킹_개론.md)
- [4.3 암호학 개론](Part9_MMOContent(DB_Connection)/4장_라이브_준비/4.3_암호학_개론.md)
- [4.4 서버 구조 개론](Part9_MMOContent(DB_Connection)/4장_라이브_준비/4.4_서버_구조_개론.md)
- [4.5 Shared DB](Part9_MMOContent(DB_Connection)/4장_라이브_준비/4.5_Shared_DB.md)
