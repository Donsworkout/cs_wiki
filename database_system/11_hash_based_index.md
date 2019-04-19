# 11) Hash Based Index
> Hash Index는 범위질의에 대해 지원하지 않는다라는 사실을 알고 가자

### 1. 해시 인덱스 종류
* Static Sturucture : Static Hashing
* Dynamic Structure : Extendible Hash / Linear Hash

### 2. Static Hashing (static)
<img width="465" alt="스크린샷 2019-04-19 오후 2 15 58" src="https://user-images.githubusercontent.com/26560119/56408066-b1997800-62ad-11e9-8d19-3339f369709a.png">

* ***h(k) mod N*** 이 Skey인 k가 있는 버켓이다.  (N = # of buckets)
  * 여기서 당연히 bucket_num은 1 ~ N 이다.
* 얘도 ISAM 과 비슷하게 버켓이 넘친다면 overflow chain 을 추가한다.

### 3. Extendible Hashing (dynamic)
> 생각해 보면, 그냥 버킷 차면 버킷을 doubling 하면 되지 않느냐 물어볼 수 있다.  
그러나 그러기엔 모든 페이지를 reading / writing 하는 것은 너무 비싸다. 

**그렇다면 바로 bucket 대신,  
bucket 으로 가는 포인터가 담긴 directory를 사용하는것이 어떨까?**  
A) *좋다, doubling 할 때, directory 만 doubling 하면 되겠네!*  
디렉토리는 file 보다 훨씬 작아서 비용이 적게 들 것이다.  

#### 3-1) 삽입 해보며 doubling 과정을 지켜보자  
<img width="329" alt="스크린샷 2019-04-19 오후 2 23 50" src="https://user-images.githubusercontent.com/26560119/56408305-c6c2d680-62ae-11e9-9056-2ff70a4f0668.png">

> 여기서 20을 삽입한다고 가정해보자
1. 20은 이진수로 10100이므로 **A bucket**에 들어가면 되지만, 꽉 찬 상태다.
2. 따라서 doubling 하고 기존 꽉 찬 bucket을 split 하자!
3. Directory가 2배로 늘어나고 Global Depth 는 3이 된다.
4. Directory의 pointers는 뒷 세글자를 보고 Bucket을 포인팅 하는데,  
대신 Local Depth가 3 미만인 Bucket에 대해서는 뒷 두글자를 보고 pointing 한다.

> 항상 Bucket full => doubling은 아니다.  
이미 doubling 되어 있다면 새로운 bucket만 만들어 주면 된다.  
삽입 후는 아래와 같다. (지역 깊이와 전역 깊이가 같은 버킷 분할시 디렉터리 더블링)

<img width="575" alt="스크린샷 2019-04-19 오후 2 32 37" src="https://user-images.githubusercontent.com/26560119/56408551-0211d500-62b0-11e9-8189-eeda4b2e5357.png">

> 그리고 더블링 할 시 앞 3글자를 사용할 수 있지만, 뒤 세글자를 사용한다.  
단순히 doubling 할 때, 복사만 하면 되기 때문이다.

#### 3-2) Extendible Hash 에서 삭제
1. 데이터를 찾아서 삭제한다.
2. 그 버킷이 비게 되면, 그 버킷의 분할 이미지와 합병될 수 있다.
3. 그러면 지역 깊이나 전역 깊이가 줄어들 수 도 있다.

### 4. Linear Hashing (dynamic)
> 얘는 overflow라고 global level 높이지 않고, 임시로 overflow chain 구성했다가, 자신의 라운드가 오면 그때 분할하는 가법이따.

#### 4-1) INSERT
<img width="570" alt="스크린샷 2019-04-19 오후 4 06 18" src="https://user-images.githubusercontent.com/26560119/56411844-17413080-62bd-11e9-8e10-a87bc87d48f9.png">

0) h(level) / h(level+1) 가지고 버켓을 찾는다.
1) bucket에 overflow page를 넣는다 (꽉 찬 상황)
2) Next 지정된 Bucket을 split 하고 Next를 증가 시킨다.  
  split 하며 재분배를 해 준다.
3) 다 돌면 Level UP 하고 Next 는 1로 하고다시 돈다.
> 어차피 동등하게 돌면서 하나하나 split 해서 지나치게 긴 overflow chain 걱정은 없다.

