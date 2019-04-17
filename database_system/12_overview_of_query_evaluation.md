# 12) Query Evaluation

### 1. Overview of Query Evaluation
- Plan : 관계 연산자 트리에서 각 관계연산을 위한 알고리즘의 선택들

#### 1-1) 쿼리 최적화에서 관심있는 2가지
1) 주어진 Query 에서 어떤 Plan 들이 고려되어야 하는가?  
*RXS (Cross Product) 같은 잡놈들은 고려도 안한다.*
2) 각 Plan의 cost 는 얼마인지
> 쿼리 최적화는 최고의 플랜을 찾아내는 것이 목적이 아니라, 차선의 플랜을 찾아내는 것이 목적  
메타 데이터는 실시간 업데이트가 아니라, 주기적 업데이트기 때문이다.

#### 1-2) 관계연산자를 평가하는 일반적인 테크닉
1. Indexing  
Selection 이나 Join 조건이 명시되면, 해당 조건을 만족하는 투플들만 (최대한 로드 줄이겠다) 검사하기 위해 Index를 활용한다.

2. Iteration  
입력 테이블들의 투플을 말그대로 iterate 하는 것, 당연히 인덱스 엔트리만 탐색할 수도 있다. (Full Scan)

3. Partitioning  
Sort 와 Hashing 등으로, 비용이 덜 들 수 있도록 한 연산을 여러 연산들로 쪼갤 수 있다.

#### 1-3) Statistics and Catalogs  
- relations 와 indexes 에 대한 정보를 열람할 수 있다
1) 각 relation의 tuple 과 page 의 개수
2) 각 index의 중복없는 키 값의 개수와 페이지 수
3) index 크기 / low,high key values


### 2. Access Paths 
> 접근 경로는 tuple 을 검색 해 오는 방법이다.  
당연히 file scan 이나 index 를 이용한 검색 방법이 있다.
1) Tree Index 이용한 접근 경로  
- 트리 인덱스 Search Key들의 prefix에 해당하는 attr 에 대해서만 match 한다.  
ex) <a,b,c> 인덱스를 가지고 있다면, a > 10 and b = 1 등의 연산은 가능하지만, prefix 가 아닌 b = 10 등에 대해서는 match 시킬 수 없다.

2) Hash Index 이용한 접근 경로  
- 모두 equality 연산이고, 모든 Search Key 집합에 대한 조건이 있을때 match  
ex) <a,b,c> 인덱스를 가지고 있다면, **a = 10 and b = 1 and c= 2**   
연산은 가능하지만, **a = 2 and b = 3** 등의 연산은 불가능하다.

### 3. 관계 연산을 위한 알고리즘  
#### 3-1) Selections
- 예를 들어 10% (100page, 10000 tuples) 의 RF 를 가지고 있으며 ***Clustered Index*** 가 존재한다면, 100번의 입출력이면 검색 가능하지만,  
만약, ***Unclustered Index*** 라면, 각 투플이 한개당 한페이지 가지고 오다가 10000번의 입출력이 발생할 가능성이 있다.
- 통상적으로, RF가 5% 이상일 경우, Unclustered Index 보다 차라리 Full scan 이 낫다.

#### 3-2) Projections  
> SELECT DISTINCT R.sid, R.bid FROM Reserves R 
- 가장 비싼 연산은 removing duplicate 이다. **(DISTINCT 키워드)** 

**Sorting Approach**  
먼저 <sid, bid> 로 정렬하고, 중복을 제거한다.

**Hashing Approach**  
해시 기반으로 모아서 분할한다.  

- 만약 인덱스가 있다면, 데이터 엔트리 소팅이 더 쉽다.

> *여기서 잠깐,  
**Reserves** 테이블은 100 tuples per page, 1000 pages 이고,  
**Sailors** 테이블은 80 tuples per page, 500 pages 이다.*
#### 3-3) Join
아래에서 나오는 예는 R⋈S 나 S⋈R 조인 이야기이다.  
1. 쌩 조인일 경우 cost, R ⋈ S  
(100 * 1000) * (80 * 500) 의 I/O 발생

2. 페이지 단위 조인일 경우 cost
100 * 500

3. 블록 단위 조인일 경우 cost  
만약, 1 block = 10 pages 라면, 10 * 50 I/O

4. 인덱스를 사용하는 조인인 경우 **(index Nested Loop)**
> hash index 를 이용하여 검색하는데, 한 튜플당 1.2 I/O  
> B+ tree index 를 이용하여 검색하는데, 한 튜플당 2-4 I/O  
(당연히 위는 Clustered 버전)  

> **Cost 구하는 법:**  
Cost = M + ((M*Pr) * S tuple 찾는데 걸리는 cost)  
M = # of pages (R)  
Pr = # for tuples per page (R)

***Sailors 의 sid에 대한 index 를 이용한다면,***  
R⋈S 이기 때문에,  
1000 + (100 * 1000)(1.2 + 1) = 221000 I/O

***Reserves 의 sid에 대한 index 를 이용한다면,***  
S⋈R 이기 때문에,  
500 + (500 * 80)(1.2 + 1) 

-> 잘 보면, 항상 Outer 기준으로 계산이 이루어 지는 것을 알 수 있다.

5. Sort - Merge 기법
- 조인 칼럼 기준으로 R, S를 먼저 정렬해서 모으고 Merge 하는방식

<img width="471" alt="스크린샷 2019-04-17 오후 4 57 53" src="https://user-images.githubusercontent.com/26560119/56270821-01ebcb00-6132-11e9-9aae-f85042439293.png">

이런식으로 정렬하고, Merge 하게 된다.  
따라서, 각 테이블 Sorting Cost 와 Merge Cost 가 발생하게 된다.

- Sorting Cost  
2 x N x # of pages  
N = # of pass  

- pass를 2라고 했을때, 위의 공식대로 R,S merge-sort 하면 총 Cost는,  
(2 x 2 x 1000) + (2 x 2 x 500) + 1000 + 500 = 7500 I/O 이다.  
마지막 1000 + 500 은 Merge 비용이다.

### 4. System R Optimizer
- 10개 이하의 조인에 적합 (10! 넘어가는건 오바)
- System 카탈로그를 이용하여 차선의 해를 구함
- 카티시안 곱은 무시
- left deep plan 만 취급함 

### 5. Size Estimation / Reduction Factors(RF)  
- 결과의 크기를 예측할 수 있는데, 이를 cardinality라고 한다.  
> cardinality = Max # tuples * product of all RFs  
예를 들어 SELECT * FROM (Term1) and (Term2) 일 경우, 각 RF가 0.5, 0.3 이라면, cardinality 는 총 10만개 퓨플이 있다고 했을때, 100,000 * 0.5 * 0.3 이다.
- RF는 몇놈이 걸릴까를 예측하는 것임, 각 Term 과 관련되어 있음
- Term col = value 일 경우, RF = 1/Nkeys(I) [키값 개수 중 하나]
- Term col1 = col2 일 경우, RF = 1/MAX(NKeys(I1), NKeys(I2))
- Term col > value 일 경우, RF = 범위 중 속하는 놈들 / 전체 범위 

### 6. 조인 여러 방법에 대한 예제
#### 1. 쌩 조인 (index 사용 x)
<img width="649" alt="스크린샷 2019-04-17 오후 9 42 20" src="https://user-images.githubusercontent.com/26560119/56288502-e399c580-6159-11e9-8a88-da3b5ed9e771.png">
  
- 그림을 보면 알겠지만, selection 을 먼저 하지 않고 바로 조인을 때려버리고 있다.
- 그러면 헤비한 테이블 두개가 조인 되버린다.
- 이때 최소로 하려고 S를 outer R을 inner 로 하여 join을 한다고 가정하자 (S ⋈ R)  
- 500 + (500 * 1000) I/Os 가 나온다.
- 500은 R 페이지들 먼저 가져와서 세팅하는 것이고,   
500 * 1000은 tuples 곱하려고 page 가져오는 시간이다.

#### 2. 인덱스 안쓰고 조인 (selection 먼저)
<img width="651" alt="스크린샷 2019-04-17 오후 9 52 26" src="https://user-images.githubusercontent.com/26560119/56289103-2a3bef80-615b-11e9-889d-61f7390e4035.png">

- 먼저 selection 했으므로, Reserves 는 100개중 1개 뽑았다고 가정하면,  
1000 page 중, 10 page 라고 할 수 있고
- Sailors 는 10개 rating 중 5개를 포함하므로 500 page 중 250 page로 RF를 감소시킬 수 있다.
- 어쩃든 index 를 쓰지 않으므로, sort-merge 를 해야 한다.
- Reserves 쪽 2 * 2 * 10 + Sailors 쪽 2 * 3 * 250 과  
 머지비용(10 + 250) 더하면 cost 구할 수 있다 (1800 I/Os).

 #### 3, 드디어 인덱스와 함께라면!
 <img width="652" alt="스크린샷 2019-04-17 오후 9 52 36" src="https://user-images.githubusercontent.com/26560119/56289104-2a3bef80-615b-11e9-88ca-9af8c535a619.png">

 - R ⋈ S 일때 10 + 10 * 100(1.2 + 1) = 1210 I/Os (가장 저렴이다)