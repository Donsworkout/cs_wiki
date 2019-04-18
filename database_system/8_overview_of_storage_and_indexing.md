# (8) Overview of Storage and Indexing

### 1. Alternative File Organizations
> 파일을 조직하는 데에, 상황마다 최적의 방법이 있음
1. Heap (random ordered)  
정렬 기준이 없는, 말그대로 random ordered 파일 구조  
2. Sorted Files  
레코드들이 특정한 기준에 의해 정렬되어 있음  
3. Indexes *(인덱스 말고 Data FIle 말함)*  
Tree, Hash 형태로 레코드들을 조직하는 Data Structure  
**search key** 기반으로 record subset 검색 속도가 빠름  
Update는 Sorted File에 비해 훨씬 빠름 

### 2. Indexes
> 인덱스를 사용해 검색하였을때,  
비 단말 노드를 **index entry** 라고 하고, 단말 노드를 **data entry** 라고 한다.  
트리 기준 index entry 에는 (SKey / Pointer)  
단말 노드(data entry)는 여러 형태가 있다.  

#### 2-1) B+ Tree Index 
<img width="603" alt="스크린샷 2019-04-18 오후 2 06 19" src="https://user-images.githubusercontent.com/26560119/56338028-2e135480-61e3-11e9-969e-e255affdd63e.png">

#### 2-2) Hash Based Index  
- Equality Selection 에 유리함 : h(r) = leaf 이런 느낌  
- 이 친구는 해시함수에 바로 대입하고 결과 (data entry) 로 **BUCKET** 을 뱉어준다.
- 따라서 index entry (트리의 비단말 노드) 가 필요없다.  

### 3. Data Entry에 들어갈 수 있는 정보  
**ALTERNATIVE 1 :**   
search key k를 포함하는 실제 데이터 레코드 (인덱스 파일 구성)  
- heap, sorted 파일 구조 대신 쓰기도 한다.  

**ALTERNATIVE 2 :**  
<k, rid of data> : 유일키일 경우 하나의 rid  
Primary, Unique index 라고 부른다

**ALTERNATIVE 3 :**  
<k, list of rids of data> : 유일키가 아닐경우 rid list  
Secondary index 라고 부른다

> ALT1 인덱스 파일 구성은 자주 쓰지 않음  

### 4. Clustered **vs** Unclustered Index
<img width="611" alt="스크린샷 2019-04-18 오후 2 24 32" src="https://user-images.githubusercontent.com/26560119/56338629-b692f480-61e5-11e9-9bb9-02b311931d69.png">


> Data entries 와 Data records 가 같은 기준으로 소팅이 되어 있는 것
- 하나의 Data Entry로 최대한 많은 records 를 끌고 오는 것이 목적이니,  
 당연히 hash index 에게 해당하는 얘기는 아님  
- 1개 테이블에 여러가지 Clustered Index는 적용 불가 (너무 커짐)
- Unclustered는 data entry를 찾았어도,  
 여러 pages 에서 record 를 메모리로 끌어 올려야 되어 큰 I/O 시간이 발생함 

 ### 5. 파일 구조 별 검색 / 업데이트 비용 비교  
#### 5-0) Cost 단위 
* B : 총 페이지 수
* D : I/O 시간
* R : 레코드 수

#### 5-1) 비교 대상  
Heap Files  
Sorted Files  
Clustered B+ tree file (ALT1)  
Heap Files with unclustereed B+ tree index  
Heap Files with unclustereed HASH index 

#### 5-2) 가정
- Index data entry 는 data record 의 10% 사이즈
- Hash 에서 80% page occupancy (따라서 데이터 사이즈 1.25배)
- Tree 에서 67% page occupancy (따라서 데이터 사이즈 1.5배)

#### 5-3) 비교할 Operations  
Scan / Equality Search / Range Selection / Insert / Delete

#### 5-4) 비교 테이블
<img width="587" alt="스크린샷 2019-04-18 오후 2 40 57" src="https://user-images.githubusercontent.com/26560119/56339161-01157080-61e8-11e9-805b-cf977567f45e.png">

* **Scan**
  * Heap, Sorted 가 제일 빠름   
  (오버플로우 방지 공간 부풀리기가 없고, 데이터 엔트리 자체가 레코드이기 때문)
* **Equality**  
  * Equality Search 는 그 누가 뭐래도 Hash (페이지 찾고 레코드 찾고 = 2D)
  * 그러나 Clustered 도 만만치 않다.  
  이친구는 Fanout 이 높으면 높을수록 빨라진다. 
* **Range**
  * Range 나온 순간 일단 Hash index 는 인덱스 안쓴거랑 똑같다.  
  * Range 의 TOP은 바로 Clustered 이다. Fanout 을 극대화 시켜서 페이지를 빨리 찾고, 거기서 매칭된 Pages 만 뒤지면 된다. 당연히 클러스터 되어 있기 때문에 갖고올 페이지 자체가 Unclustered 보다 훨씬 적다.
* **Insert**
  * 순서 정렬할 필요가 없는 heap이 top
  * 그리고 Clustered 나 Index 가 Sorted 보다 훨씬 빠른 것을 할 수 있다. 그냥 위치 찾고 넣으면 되는데, Sorted 는 모든 노드를 다 올겨야 한다.
* **Delete**
  * Heap 과 Clustered 가 가장 빠르다.
  * 찾고 그냥 꽂아 넣으면 된다. (재배열 할 필요가 없음)

### 6. Workload 에 대한 이해
> 대표적인 질의와 UPDATE 연산이 혼합된 문맥 
* 어떤 relation이 참여했는지
* 어떤 attr을 검색했는지
* 어떤 attr이 select 나 join 연산에 참여했고, 해당 연산이 얼마나 selective 한지
* 어떤 업데이트 연산을 사용했고, 어떤 attr에 영향을 미치는지

### 7. 어떤 인덱스를 쓸지 선택하기  
#### 예제 1
<img width="176" alt="스크린샷 2019-04-18 오후 4 39 48" src="https://user-images.githubusercontent.com/26560119/56344545-a3d5eb00-61f8-11e9-88c8-18a53580b2e4.png">  

- age > 40 인 사람이 그렇게 selective 하지 않다면, clustered 가 완전 이득이지만,  
- age > 40 인 사람이 전체 record 수와 맘먹는다면, Full Scan 이랑 별 차이 없다.

#### 예제 2
<img width="252" alt="스크린샷 2019-04-18 오후 4 39 54" src="https://user-images.githubusercontent.com/26560119/56344546-a3d5eb00-61f8-11e9-88a6-759a8bcf139f.png">

- E.age > 10 인 사람이 너무 selective 하다면, heap scan 이 더 빠를수도 있다.
- GROUP BY 키워드가 들어가므로, E.age index 를 사용하고 튜플들을 dno 로 정렬하는 것이 시간이 오래 걸릴 수 도 있다.
- E.age > 10 이 지나치게 선택적인것이 아니라면, Clustered E.dno index 로 정렬 때려놓고 나중에 selective 하는 것이 최상일 것으로 예상된다.

### 8. 복합 서치 키
<img width="681" alt="스크린샷 2019-04-18 오후 4 51 26" src="https://user-images.githubusercontent.com/26560119/56345249-3aef7280-61fa-11e9-9f51-2ced59fbf179.png">

* 4번째 문장의 뜻은  
***"복합 인덱스가 너무 커지면, 그만큼 한정된 용량 때문에 fanout이 작아지면서, 트리가 update(깊이의 변화) 되는 빈도가 더 늘어날 것이다."*** 라는 말이다.

### 9. Index Only Plans
> COUNT, MIN, AVG 같은 직계함수는 Data File을 보지 않고, Index File 만으로도 연산을 처리할 수 있다.

<img width="489" alt="스크린샷 2019-04-18 오후 5 00 34" src="https://user-images.githubusercontent.com/26560119/56345778-85252380-61fb-11e9-86de-e502fbaa67f8.png">
