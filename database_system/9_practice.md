### 응용 데이터베이스 문제 풀이 (9장)
#### 컴퓨터공학과 4학년 B311018 김도현

#### 9.5
1.  트랙의 용량 : 512 * 50 = 25600 bytes  
    표면의 용량 : 25600 * 2000 = 50,000 kb  
    디스크 용량 : 50,000 kb * 2 * 5 = 500,000 kb

2. 2000개의 트랙이 있으므로, 2000개의 실린더를 가진다

3. 2048 byte 는 섹터 크기의 배수라 가능, 51,200 byte 는 track 의 사이즈를 벗어나서 불가

4. 초당 90회 회전하므로, 최대 회전 지연 (한바퀴 회전) 1/90 초 (= 0.011 sec)

5. 25kb * 90 = 2250 kb/s

#### 9.6
1. 1024 // 100 = 10 개 records 들어갈 수 있음

2. 순차 배열이기 때문에, 먼저 같은 트랙, 실린더에 데이터를 저장할 것이다.  
한 블록에 10 records 가능하므로, 10000 blocks 필요하다.  
  한 실린더 (트랙 * 10) 에 2500개 레코드 들어가므로 100,000개는 2500을 한참 뛰어넘는다.  
  따라서, **10개** 면 모두 필요하다.

3. 10 * 500,000 (블록=kb) = 5,000,000 records

4. 한 트랙 다 차면, 같은 실린더로 내려간다.  
그리고 1트랙 = 25 blocks 이므로, 다음 표면의 track1 의 block1 에는 26번째 block 이 저장된다.  
만약, 디스크 헤드가 동시에 데이터를 쓸 수 있다면  
한 트랙을 먼저 채우지 않고 실린더를 채울 것이기 때문에, 
block 2 가 다음 표면에 들어갈 것이다.

5. 10000 block 이면 한 트랙에 25 block 이므로 400 트랙이 필요하다.  
한 트랙 다 차면 실린더로 내려갈 것이므로 한 면에 40 트랙씩 읽으면 된다.  
(1) seek time (암 움직이는 시간) 0.01sec * 40 (실린더) = 0.4 sec  
(2) 회전 지연 : 0.011 sec * 400 = 4.4 sec  
즉 0.4 + 4.4 = 4.8 sec이 걸린다.

#### 9.15
1. 하드웨어에 의존하지 않고 DBMS 자체 기능을 사용하기 위하여 자체 pre-fetching 을 사용한다. 

2. 페이지 요청이 동시에 수행되면, 이미 요청한 리소스가 사용하고 있을때 hit rate을 떨어트릴 수 있다.

3. pinned 된 프레임에는 접근하지 못하도록 한다.

4. 동시에 프레임 할당이 가능해 진다. pre-fetching은 여전히 중요하다. 그러나 더 믾은 케이스를 고려해야 하고 요청한 파일이 각 segment를 넘을 떄 똑같이 비효율적이게 된다.
