---
description: '-'
---

# Index

* [https://github.com/google/leveldb](https://github.com/google/leveldb)
* Google에서 만든 키/값 저장소
* 문자열 키에서 문자열 값으로의 정렬된 매핑을 지원할 수 있음
* 핵심 스토리지 아키텍처
  * 쓰기 최적화된 B-트리 변형인 로그 구조 병합 트리 (LSM)
* 소규모 임의 쓰기와 달리 대규모 순차 쓰기에 최적화되어 있음

#### 역사

* 두 명의 Google 직원 Jeff Dean과 Sanjay Ghemawat는 BigTable 태블릿의 설계 체계에서 영감을 받음
* BigTable의 태블릿은 선택된 행을 따라 분할된 테이블의 세그먼트로 정의됨
  * BigTable 태블릿의 특성을 포함하는 하나의 오픈 소스 시스템을 구축하기를 원함

#### 체크포인트

* 작업 로깅 파일이 한도를 초과하면 체크포인트를 수행
* 데이터가 디스크로 flush 되고, 압축 방식이 호출됨
  * 따라서 데이터는 아래로 내려감
* 그 외에도 ⇒ leveldb는 새로운 사용을 위해 새로운 로깅 파일 & memtable을 생

#### 동시성 제어

* Leveldb는 한 번에 하나의 프로세스만 열 수 있도록 허용
* 운영체제는 잠금 체계를 사용해 동시 액세스를 방지
* 하나의 프로세스 내에서 Leveldb는 여러 스레드에서 액세스할 수 있음
* 다중 작성자의 경우
  * 첫 번째 작성자만 데이터베이스에 쓸 수 있으며 다른 작성자는 차단됨
  * read-write 충돌의 경우 독자는 쓰기 프로세스와 분리된 불변에서 데이터를 검색할 수 있음
  * 업데이트된 버전은 압축 프로세스에서 적용됨

#### 데이터 모델

* key/Value
* 키/값 저장소는 키에서 해당 값으로의 매핑을 지원
* SSTable에서 키와 값의 레이아웃은 인접한 문자열 시퀀스로 관리됨

#### Index

* `Skip List`
* `Log-Structured Merge Tree`
* MemTable의 `skip List`를 사용함
* LSM-tree는 key-value 쌍으로 구성된 쓰기 최적화 B-tree 변형의 한 유형
* LSM 트리는 삽입 및 삭제에 최적화된 영구 key-value 저장소
* LevelDB는 오픈 소스 LSM 트리 구현

#### **Isolation Levels ( 격리 수준 )**

* 주어진 시점에서 데이터베이스의 상태를 저장하고 이에 대한 참조를 지원
* 사용자는 스냅샷이 생성된 시점의 특정 스냅샷에서 데이터를 검색할 수 있음

#### Joins : 지원 안 됨

#### Logging

* 모든 삽입, 업데이트 또는 삭제 전에 시스템은 로그에 메시지를 추가해야 함
* 노드에 장애가 발생한 경우 커밋되지 않은 메시지를 검색하고 복구를 위해 다시 작업을 수행할 수 있음

#### Query Compliation(쿼리 컴파일) : 지원 안 됨

#### Query Execution(쿼리 실행) : 한 번에 튜플 모델

#### Query Interface(쿼리 인터페이스) : Custom API

* leveldb의 키와 값은 임의 길이의 바이트 배열
* `Put()`, `Get()`, `Delete()` 와 같은 기본 작업을 지원
* Batch 작업인 `Batch()` 도 지원
* 전체 작업 프로세스가 함께 실행되고 결과를 단일 Batch 작업으로 반환
* 단, SQL 타입의 데이터베이스가 아니기 때문
  * SQL 쿼리는 지원하지 않음
* `+` 인덱싱을 지원하지 않음

***

#### Storage Architecture

* `Disk-Oriented`
* 일시적으로 액세스한 데이터를 `MemTable`에 넣고 주기적으로 `MemTable`에서 `Immutable SSTable` 로 데이터를 이동합니다
  * 각 레벨에서 유효하지 않은 데이터를 줄이기 위해 압축을 채택한 후
    * 다음 레벨에서 하나의 새로운 블록을 생성
* 질의를 위해 `SSTable`을 읽기 위해 `mmap` 또는 기본 읽기 `syscall` 을 사용합니다
* OS는 LevelDB에 대해 SSTable을 캐시합니다
* LevelDB는 값을 압축하기 위해 snappy 사용을 지원
  * 각 쿼리에 대한 데이터 압축 해제를 방지하기 위해 캐시를 도입

#### Storage Model

* `N-ary Storage Model(Row/Record)` (N항 저장 모델?)
* SSTable은 NSM을 사용해 데이터를 정렬
* 임의의 정렬된 키-값 쌍 세트를 포함
* 블록의 끝에서 각 블록의 시작 오프셋과 키 값을 제공
  * 블룸 필터(Bloom Filter)를 사용해 대상 블록을 검색할 수 있음

#### Storage Organization

* `Log-structured`

#### Stored Procedures, Views : 지원 안 함

#### System Architecture

* `Embedded`
* leveldb에서 불변은 다른 클러스터 노드에서 공유할 수 있는 디스크에 저장됨
* 총 7개의 레벨과 최대 2개의 인메모리 테이블이 있음
* 절차는 먼저 시스템이 `MEMTable` 이라는 메모리 내 테이블에서 쓰기 작업을 버퍼링하고
  * 데이터가 가득 차면 데이터를 디스크로 플러시하는 것으로 설명할 수 있음
* 디스크에서 테이블은 레벨로 구성됨
  * 각 레벨에는 SSTable이라는 여러 테이블이 있음
  * 하위 레벨은 상위 레벨보다 더 큰 용량을 유지함
* 상위 수준이 가득차면 시스템은 여러 SSTable을 읽고 써야할 수 있는 하위 수준으로 데이터를 푸시해야 함
*
