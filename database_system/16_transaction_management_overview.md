# (16) Transaction Management Overview
### 1. 트랜잭션(Transaction) 이란?
- read write 순서를 가진 프로그램에 대한 DBMS의 추상적 관점
-  데이터베이스의 상태를 변환시키는 하나의 논리적인 작업 단위
- 프로그램은 많은 데아터 오퍼레이션을 담고 있지만
- DBMS가 집중하는 것은 READ 되었는지 WRITTEN 되었는지 그 자체
- 유저는 각 트랜잭션이 그 단위 자체로 실행되는것처럼 생각이 들게 함

### 2. 트랜잭션 스케줄링
1. Serial Schedule 
- 아예 인터리빙 안하고 트랜잭션 각각 진행
2. Equivalent Schedule 
- 첫번쨰 스케줄과 두번째 스케줄이 가져오는 굘과가 같음 
3. Serializable Schedule
- 시리얼 트랜잭션과 같은 효과가 나는 스케줄 

### 3. Lock Based **Concurrency Control**
#### 3.1 Strict 2 phase locking  
> reading 하기전에 Shared Lock, writing 하기전에 eXclusive Lock
- 트랜잭션이 완료되고 락을 풀 수 있음
- Non strict 2PL은 중간에 락을 풀순 있지만 풀고나면 다시 락 못걸음 
- 2PL 의미가 진행중 페이스 / end 페이스 (commit / rollback)
### 4. Aborting a Transaction
- abort 되면 transaction undo
- commit time 후에만 락을 풀게해서 cascading abort 를 막는다
- undo 를 위해 트랜잭션은 write, record log를 찍는다 

### 5. LOG - for crash recovery
- disk 반영 되기전에 log 부터 쓴다.
- 로그 기록은 트랜잭션 ID로 체인 되어 있어서 undo 하기 쉽다.
- 로그 기록은 안정적인 조건에서 저장어야 한다. 

### 6. Recovering from the crash
> 리커버리 알고리즘은 3 페이스가 있다.
1. Analysis  
최근 체크포인트에서 부터 탐색하면서, 살아있는 트랜잭션을 탐색하고 충돌 시간에 버퍼풀에 기록된 dirty read를 탐색한다. 

2. Redo  
버퍼 풀의 dirty pages 의 모든 업데이트를 Redo (바뀐 값으로 바꿈) 한다. 쓰기 전에 미리 읽었기 때문이다. 업데이트 되었던 모든 로그 기록을 추적하여 Redo 한다.

3. Undo  
말그대로 예전의 값으로 rollback  하는 것

