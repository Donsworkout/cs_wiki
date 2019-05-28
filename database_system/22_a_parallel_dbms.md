# 22-a) Parallel DBMS
### Parallelism은 자연스러운 DBMS의 처리이다.
- Pipeline parallelism : 순차 처리를 여러 스텝으로 나누고 파이프라이닝 하는 처리
- Partition parallelism : 서로 다른 머신이 같은 것을 나누어 처리하는 것

### Parallelism 의 지향
- 대역폭 중가로 같은 시간에 많은 데이터 처리 (Speed Up)
- 데이터 사이즈 증가가 있어도 Transaction당 처리시간 같음 (Scale Up)

### 구조적 이슈

<img width="865" alt="스크린샷 2019-05-28 오후 4 46 43" src="https://user-images.githubusercontent.com/26560119/58460259-378bc700-8168-11e9-8d8e-f7cfbfacf1e5.png">

- Shared Memory 나 Disk 는 프로그래밍이 쉽지만, Scale-up 이 힘들다.
- Shared Nothing은 머신 간 분리가 되어 있어 프로그래밍이 어렵지만, Easy Scale up이 가능하다.

### Different Types of DBMS
- Intra-operator parallelism  
모든 machine 들이 같은 operation 에 대해 병렬적으로 컴퓨팅  
- Inter-operator parallelism  
각 operator 들이 각각 다른 site에서 작업함 (파이프 라이닝)
- Inter-query parallelism  
다른 사이트에서 각각 다른 쿼리를 실행하는 것 


### 데이터 파티셔닝의 종류 
<img width="1046" alt="스크린샷 2019-05-28 오후 4 56 56" src="https://user-images.githubusercontent.com/26560119/58460968-a0277380-8169-11e9-9a4c-9c926b007065.png">

- Range 파티셔닝이 제일 무난하다. 
- 그러나 로드 밸런싱을 중요시한다면 Hash나 Round Robin 방식이 더 좋다 (데이터가 균등하게 나누어 지도록 함)

### 병렬 Scan, Sorting
1) Parallel Scan  
병렬적으로 각각 local site 에서 scan 하고 merge 해서 최종 스캔본을 가져다 준다.

2) Parallel Sorting  
병렬적으로 스캔하고, 이를 range 로 분할하여 local site 에서 소팅한다. 결과 데이터는 자연스럽게 range partitioned 되어 있으며, 정렬이 되어 있을 것이다. 대신 균등분포가 아닌 skew 상태가 될 수 있다. 

### 병렬 join
1. Nested Loop Join  
- 조인칼럼을 기준으로 range 파티셔닝을 하면 조인이 쉽다. 조인할 rows 는 같은 사이트에 있을 것이기 떄문이다. 그 경우 아니면 다른 사이트를 돌아야 할 것이다.

2. Sort - Merge Join
- 소팅&머지 기법으로 소팅하면 자연스레 range partitioning 되고, 각 로컬 사이트에서 병렬 조인하고 나중에 합치면 된다.

3. Parallel Hash Join
- 파티션을 지정된 키에 따라 여러 사이트로 분베하고, 조인하는데 당연히 조인할때 지정된 키를 외래키로 썼다면, 같은 사이트에 분류되어 있게 되어 합칠 수 있다.

### 최고의 serial plan != 최고의 parallel plan
<img width="1016" alt="스크린샷 2019-05-29 오전 1 06 04" src="https://user-images.githubusercontent.com/26560119/58493479-00d89f80-81ae-11e9-9664-cba7fe25189b.png">

- 그림에서 좌측은 selectivity 가 높아서 테이블 스캔이 더 효율적이고, 우측인 인덱스를 쓰는 것이 더욱 효율적이다. 
- 이처럼 한꺼번에 테이블을 스캔할때와 최적의 스캔 방법이 다르다.

### 참고 : Sharding
> Horizontal Partitioning이 곧 ***Sharding*** 이다. 따라서 관건도 Shard Key 를 적절히 조절해서 skew 되어 성능이 떨어지는 문제를 피해야 한다.

- 해시 샤딩  
데이터베이스 id를 해싱하여 들어갈 버킷 (사이트) 를 결정한다. 단,클러스터 내 Node(Site)를 확장하려면 해시 함수를 재구성 해야한다 (re-sharding 필요)

- 다이내믹 샤딩  

<img width="400" src="https://nesoy.github.io/assets/posts/20180530/2.png">
  
- 위 그림과 같이 range 에 따라 파니셔닝 하는것과 비슷하다. 클러스터의 노드를 추가해도 샤드키만 Locator Service에 추가한다면, 동적으로 확장이 가능하다.