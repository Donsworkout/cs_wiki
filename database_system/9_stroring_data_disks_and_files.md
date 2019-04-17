# (9) Storing Data: Disks and Files

### 1. Disks and Files
- DBMS 는 디스크에 정보를 저장함
- Read 는 데이터를 디스크에서 메인 메모리로 올리는 것
- Write 는 데이터를 메인 메모리에서 디스크로 쓰는 것

> *"왜 메인메모리에 데이터 저장 안하나?'*  
*'비싸고 휘발성 이니깐!'*

- 디스크는 RAM 과 다르게, 디스크 내 페이지 location에 따라 DBMS의 퍼포먼스가 결정됨. 즉, 물리적 거리가 영향을 미친다.

### 2. Component of a Disk
<img width="460" alt="스크린샷 2019-04-16 오후 5 50 29" src="https://user-images.githubusercontent.com/26560119/56195556-2da67c80-6070-11e9-9b1d-d39b8e17c7dd.png">

1. ARM 이 움직여서 트랙을 먼저 찾고 
2. 트랙 내에서 spindle이 움직이며 head 의 위치를 잡아둔다.

- 트랙에서 head 들이 얹어있는 지점들을 선으로 나타낸 것이 실린더이다.
- 동시에 한개의 head만 데이터를 읽어들인다.
- 그림에서 Sector 을 여러개 뭉친것이 Block 이다.
- 보통 같은 테이블은 같은 트랙이나 실린더에 넣고, 그래도 모자라면 adjacent track 으로

#### 2-1) IO 시간의 비밀
- Seek Time (ARM 을 움직여 트랙을 찾는 시간)
- Rotational Delay (회전지연, spindle이 움직여 트랙 내 head가 안착할 지점 찾는 것)
- Transfer Time (전송시간, 디스크 to 메모리 or 메모리 to 디스크)  
**[seek time 이나 회전지연을 줄이는 것이 I/O cost 를 낮추는 관건]**


#### 2-2) Arranging Pages on DIsk 
- **next block** 은 최대한 줄이는 방향으로
- 위에서 언급한 대로, 같은 트랙 같은 실린더에 최대한 모아놓을 수 있도록
- 순차 탐색을 해야 할때는 몇몇 페이지를 prefetching 하는것이이득

### 3. The 'RAID'
- 디스크 배열로, 여러개지만 하나의 큰 디스크 처럼 추상화 된다.
- Data Striping 을 통한 Performance 증대
- Redundancy 를 이용한 신뢰도 증가 

#### 3-1) RAID Levels
- **Level 0**  
**No redundancy**  
중복성이 없다, 저렴한 비용으로 대역폭을 극대화하나 신뢰도가 부족하다

- **Level 1**  
**Mirrored**  
즉 각 디스크는 check disk 를 가지고 있다. (스트라이핑 없이 걍 미러링) 따라서, 퍼포먼스 변화는 없고 신뢰도는 증가한다.  

- **Level 0+1**  
**Striping and Mirroring**  
얘는 미러링을 하면서도 스트라이핑을 하여, 신뢰도 뿐 아니라 대역폭 증가로 인한 퍼포먼스 이득을 보겠다는 것이다.  

- **Level 2**   
**Bit Striping**  
비트 스트라이핑을 하여 D개로 데이터를 스트라이핑 하는데, 대규모 요청에는 좋다, 왜냐하면 그만큼 여러 디스크에서 동시에 뽑아먹을 수 있기 때문이다. 
중복성은 해밍 코드를 사용한다.

- **Level 3**   
**Bit Interleaved Parity**  
level 2의 공간활용도를 업그레이드 한 버전이다.   
해밍 코드를 쓰지 않고 디스크 제어기를 이용한다.   
이는 패리티 정보를 가지는 단일 check disk 만 가지면 된다.  
디스크는 한번에 한 요청만 처리할 수 있고, read write 작업에 모든 디스크가 쓰인다. 

- **Level 4**  
**Block Interleaved Parity**  
Striping Unit 이 Block, 마찬가지로 하나의 Check Disk  
비교적 분할 단위가 적어서 데이터 규모가 작은 요청같은 경우에, 병렬로 처리가 가능하다  
그러나 큰 요청 같은 경우에는, 여전히 모든 대역폭을 사용해야 한다.

- **Level 5**  
**Block-Interleaved Distributed Parity**  
RAID Level 4 와 비슷하지만, parity block (한 check disk에 있던 놈들) 을 모든 디스크에 걸쳐 나누어 논 것이다. (병목 현상을 제거하기 위해)

### 4. Disk Space Management
- 디스크 공간을 관리하는 DBMS SW에서 가장 로우 레벨이다.  
- page 를 allocate/de-allocate 하고 read/write 한다.

<img width="605" alt="스크린샷 2019-04-16 오후 8 18 42" src="https://user-images.githubusercontent.com/26560119/56205480-d9f25e00-6084-11e9-85d4-9d4ba7dbc523.png">

- Buffer 에서 페이지가 들어갈 자리를 Frame 이라고 한다. 
- Buffer 가 꽉 차면 버퍼에서 내보내기도 하고 다른놈을 넣어주기도 한다
- LRU, MRU, Clock 등 Dynamic하게 Buffer Replacement Policy 를 변경한다.

> *그런데 이때, 요청한 페이지가 Buffer Pool 에 없다면?*  
1) 들고올 페이지가 위치할 프레임을 고른다. 그리고 프레임의 **pin_count**를 증가시킨다.
2) 프레임이 **dirty (데이터 변경사항 o)** 하다면, 디스크에 이놈을 일단 반영
3) 요청한 페이지를 프레임에 살포시 꽂아준다.
4) 그리고 꽂힌 메모리 주소를 요청한 코드에게 날려준다.
5) 코드에서 제어가 돌아오면 **unpin** 하고 **dirty bit** 을 1로 바꿔준다.

- 위에서 pin_count 가 증가되었다는 것은 pinned 되었다는 것을 말하는데, 이는 제어가 한 코드에게 넘어가 있다는 뜻이고, 따라서 pinned 된 프레임 (페이지)는 수정하지 못한다. 따라서 로직에서 복귀할 시 꼭 다쓴 페이지는 unpinned 해야한다.
- 당연히 순차 스캔 같은 경우, 즉 요청이 예상 될때는 pre-fetch 하는 것이 좋다.
- 아무튼 결론적으로 unpin 된 놈만 교체 후보에 오를 수 있다.

#### 4-1) Buffer Replacement Policy
아까 말했듯이, LRU, MRU, Clock 등 여러가지 정책이 있는데,  
그중 가장 흔한놈이 LRU 이다. 그러나 간혹 이것이 최선이 아닐 수 도 있다.  
예를 들어 버퍼(최대 10개)보다 읽어야 할 페이지수(11개)가 많을 때, LRU 를 써버리면 가장 오래된 놈 찾을라고 디스크로 숨어버린 놈들까지 다 뒤져야 한다.  
이럴 바에는 MRU 써서 가장 최근 쓰인놈 잡으면 된다.
> 이것을 바로 순차적 범람 (Sequencial Flooding) 이라고 부른다.

### 5. 왜 OS 말고 DBMS 를 쓰는가?  
> 아니, OS도 Disk space / Buffer 관리 할 수 있는데, 왜 OS 안쓰나?  

1) OS마다 지원이 다르잖아
2) DBMS의 Buffer mgmt 가 훨씬 좋은데, 그 이유는  
2-1) 버퍼 풀에 page pin 하고, 강제로 Dirty 한놈 디스크에 쓰게 할 수 있다.  
2-2) replacement policy 가 access pattern 에 따라 유동적이다.  
2-3) 요청 예측을 통해, pre-fetch 가 가능하다.

### 6. Record Format
#### 6-1) Fixed Length (고정길이)
<img width="502" alt="스크린샷 2019-04-16 오후 8 52 24" src="https://user-images.githubusercontent.com/26560119/56207417-9e0dc780-6089-11e9-9b2d-a21159f8bfca.png">

- 모든 레코드의 field type 이 동일하다 (System Catalog 에 정보 저장)
- i 번째 필드를 찾는데에 풀 스캔이 필요하지 않다. 

#### 6-2) Variable Length (가변길이)
<img width="569" alt="스크린샷 2019-04-16 오후 8 52 36" src="https://user-images.githubusercontent.com/26560119/56207419-9f3ef480-6089-11e9-87f0-a2c0ceae0d27.png">

- 위 부분은 그냥 $ 로 경계를 구분해서 찾아내는 것, 직접 접근은 불가하다
- 아래 부분은 앞쪽에 필드 오프셋을 저장해 두어 직접 접근이 가능하다.

### 7. Page Format
#### 7-1) Fixed Length Records 
<img width="646" alt="스크린샷 2019-04-16 오후 9 00 31" src="https://user-images.githubusercontent.com/26560119/56207847-bcc08e00-608a-11e9-81d1-1465c9a922fd.png">

- 얘는 rid **<page id, slot #>** 바꿀때 조심 해야한다. 왜냐하면 포인터가 없기 때문  


<img width="669" alt="스크린샷 2019-04-16 오후 9 00 40" src="https://user-images.githubusercontent.com/26560119/56207849-bcc08e00-608a-11e9-947f-ed189dacbdfc.png">

- 얘는 자리 막 바꿔도 된다. 어차피 포인터로 연결 되어 있기 때문에 rid 를 바꿀 필요가 없다.

### 8. Record File 
> Page는 IO 관련 부분이고, 이제 더 상위레벨인 파일에 대해 알아보자,  
그리고 페이지 집합이 어떻게 파일로 구성되는지 알아보자 

#### 8-1) 힙 파일 구현
#### List 로 구현한 힙 파일 구조
<img width="604" alt="스크린샷 2019-04-16 오후 9 16 28" src="https://user-images.githubusercontent.com/26560119/56208831-0316ec80-608d-11e9-9ddc-06147626916b.png">

- 윗 부분은 Full Pages 이고, 아래는 빈 페이지다.
- 각 노드들은 Header Page 의 id와 name 을 가지고 있다.
- 각 노드들은 데이터와 포인터 2개를 가지고 있다.

#### Page Directory 를 이용한 힙 파일 구조
<img width="530" alt="스크린샷 2019-04-16 오후 9 16 43" src="https://user-images.githubusercontent.com/26560119/56208833-04481980-608d-11e9-868c-6e5d49d697d5.png">

- 엔트리들은 페이지 각각의 free bytes 정보를 가지고 있다.
- 디렉터리는 페이지들의 집합이다.

### 9. System Catalog
시스템 카타로그는 Meta-data 이다. 
- 각 인덱스의 구조와 skey 가 나와있고
- 각 relation 에 대한 이름, 파일명, 파일구조 (heap 등) attr, type, index name 등등의 정보
- view name, definition
- 심지어는 statistics, authorization, buffer pool size 정보 등이 있다.

Catalog 는 relations 형태로 저장되어 있다.


