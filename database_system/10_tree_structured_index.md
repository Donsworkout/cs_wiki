# 10) Tree Structured Index
> B+ Tree Index 는 Range Query 에 강력하다는 것을 알고 가자

### 1. 트리 인덱스 종류
* Static Sturucture : ISAM 
- Dynamic Structure : B+ Tree

### 2. ISAM
> INSERT, DELETE 가 Leaf Node 에서만 변화를 일으킨다.  
빈 자리가 없을 시 리프 노드에 Overflow Chain 이 생기는데,  
만약에 트리 변화가 잦은것이 아니라면 ISAM의 Overflow Chain은 별 문제가 되지 않는다.  
오히려 leaf page만 수정된다는 것을 알고 있기 때문에,  
다른 요청에 있어서 non leaf pages를 잠금하지 않아도 되어 효율성이 좋다.

#### 2-1) ISAM - INSERT
> 추가 시 순서는 상관없이 Overflow pages 를 만든다.
- 삽입 전  
<img width="461" style="margin-top:10px;" alt="스크린샷 2019-04-19 오전 10 05 32" src="https://user-images.githubusercontent.com/26560119/56399716-c1539500-628a-11e9-9934-1d462f79572d.png">

- 삽입 후  
<img width="486" style="margin-top:10px;" alt="스크린샷 2019-04-19 오전 10 05 43" src="https://user-images.githubusercontent.com/26560119/56399717-c1ec2b80-628a-11e9-8218-326e3c7affb6.png">

#### 2-2) ISAM - DELETE
> 엔트리가 삭제되고 빈 페이지는 삭제된다.  
42*, 51* 삭제 시 아래와 같다.
- 삭제 후  
<img width="469" style="margin-top:10px;" alt="스크린샷 2019-04-19 오전 10 10 58" src="https://user-images.githubusercontent.com/26560119/56399847-725a2f80-628b-11e9-98b5-9e0e53daf5a0.png">

### 3. B + Tree
> ISAM 과는 달리 Balanced Tree 를 유지하고 싶어한다.  
* 최소 50% occupancy **(d <= m <= 2d entries)**
* 통상적으로 67% occupancy를 유지하고, 평균 fanout 은 133이다.
* INSERT / DELETE 시 페이지간 Split / Merge 를 한다. (비 리프 노트에도 영향 미침)
* LEVEL 4로 3억개 이상 엔트리들을 커버할 수 있다.

#### 3-1) B + Tree - INSERT
> 삽입 순서는 아래와 같다.  
그리고 엔트리 개수가 2d + 1 일때 Overflow 가 발생한다.

1. 트리 탐색에 따라 리프 L을 찾는다.
2. L에 공간 있으면 넣고, 아니면 L을 split 한다 **(L, L2로)**  
  (1) 엔트리들 재 분배하고, middle key를 ***copy-up*** 한다. (leaf 의 Skey를 지우면 안되기 때문)  
  (2) L2를 가리키고 있는 index entry를 L의 부모로 삽입한다. (부모 연결 처리)
3. 이것이 recursive 하게 일어난다. (root 까지 꽉 차다 보면 트리 크기가 커진다)
> 여기서 주의사항은 non leaf node 를 split 할 때는 ***copy-up*** 이 아니고 ***push-up*** 이라는 것이다.

* 삽입 전 
<img width="668" style="margin-top:10px;" alt="스크린샷 2019-04-19 오전 10 33 31" src="https://user-images.githubusercontent.com/26560119/56400389-ac790080-628e-11e9-8293-c0d9943cca4e.png">

* 8* 삽입 후  
<img width="678" style="margin-top:10px;" alt="스크린샷 2019-04-19 오전 10 33 40" src="https://user-images.githubusercontent.com/26560119/56400390-ac790080-628e-11e9-9d7f-5c9fcf9ca0b6.png">  

#### 3-2) B + Tree - DELETE
> 삭제는 underflow (50% 이하)를 체크해야 한다.  
즉, L이 d-1 이하의 엔트리 개수를 가지고 있을 때 문제가 된다.  
식제 순서는 아래와 같다.

1. 삭제 후 L이 50% 이상 채워져 있으면 OK
2. Underflow 라면, 형제 페이지간 재 분배를 한다 (parent entry 도 조절)
3. 재 분배 할 수 없다면 (형제도 모자름) L과 그 형제를 Merge 해 버린다.
4. Merge 가 일어난다면, L 형제를 가리키는 부모에서 entry를 삭제한다. ***(Pull Down)***
5. 이 과정도 역시 recursive 하게 일어난다.

* 삭제 전 
<img width="678" style="margin-top:10px;" alt="스크린샷 2019-04-19 오전 10 33 40" src="https://user-images.githubusercontent.com/26560119/56400390-ac790080-628e-11e9-9d7f-5c9fcf9ca0b6.png">  

* 19*, 20* 삭제 후
<img width="693" style="margin-top:10px;" alt="스크린샷 2019-04-19 오전 10 45 17" src="https://user-images.githubusercontent.com/26560119/56400708-49886900-6290-11e9-94ca-a2034d50ed27.png">

* 24* 삭제 후
<img width="672" style="margin-top:10px;" alt="스크린샷 2019-04-19 오전 10 45 17" src="https://user-images.githubusercontent.com/26560119/56400856-067ac580-6291-11e9-98ed-c025bc369606.png">

### 4. Prefix Key Compression
> Fanout 을 높이기 위해 key를 최대한 압축하는 것.

### 5. Bulk Loading
> 트리 초기 세팅시 밑에 소팅해서 쭉 깔아놓고 차근차는 올리는 것
- 초기 소팅 상태  
<img width="589" style="margin-top:10px;" alt="스크린샷 2019-04-19 오전 11 22 39" src="https://user-images.githubusercontent.com/26560119/56401792-8b67de00-6295-11e9-99e4-f95cfc7840e0.png">
- 벌크 로딩 후  
<img width="524" style="margin-top:10px;" alt="스크린샷 2019-04-19 오전 11 22 57" src="https://user-images.githubusercontent.com/26560119/56401791-8b67de00-6295-11e9-8040-ce52244179d1.png">