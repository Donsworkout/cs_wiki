### Intel CPU - Device connection

![스크린샷 2019-12-08 오후 4 53 35](https://user-images.githubusercontent.com/26560119/70386272-4b4b9c00-19db-11ea-8aa1-ae8e0b207fb1.png)

- 디바이스 컨트롤러 : IO 제어에 필요한 최소 명령어 셋을 포함하는 특수목적 컴퓨터

- 커널 (디바이스 드라이버) 은 컨트롤러를 통해 IO device 를 간접 컨트롤한다.
  - IO intructions 이나,
  - Memory Mapped IO 

> Memory Mapped IO 란, 메모리와 IO 가 연속된 어드레스 영역에 할당됨, CPU 입장에서는 메모리나 IO나 동일한 외부기기로 취급되므로, CPU입장에서는 read write 프로그램은 load store 명령애 의해서만 둘다 처리된다.

https://m.blog.naver.com/PostView.nhn?blogId=ryutuna&logNo=100128601980&proxyReferer=https%3A%2F%2Fwww.google.com%2F

- 디바이스 드라이버 : 컨트롤러의 레지스터들로부터 읽고, 레지스터에 쓰기 위한 커널 코드 

- 폴링 IO / 인터럽트 기반 IO 

- DMA 
    - 호스트 디바이스 드라이버가 DMA 명령어 발행
    - 메모리 to 메모리 DMA 명령어도 존재 

#### DMA steps

![스크린샷 2019-12-08 오후 5 29 33](https://user-images.githubusercontent.com/26560119/70386797-7be20480-19e0-11ea-814a-e646ad6d15e9.png)

1. 디바이스 드라이브는 x 메모리 주소 버퍼에 디스크 데이터를 옮기도록 명령 

2. 디바이스 드라이버는 디스크 컨트롤러에게 C bytes 를 디스크에서 X 주소로 쓰라고 명령

3. 디스크 컨트롤러는 DMA 전송 시작

4. 디스크 컨트롤러는 DMA 컨트롤러에게 바이트 전송 

5. DMA 컨트롤러는 버퍼 X 에 바이트 전송한다. (C가 0이 될때까지)

6. C=0 이되면 DMA는 전송완료 시그널을 위해 CPU 인터럽트  

### Two stage communication

<img width="822" alt="스크린샷 2019-12-08 오후 11 25 37" src="https://user-images.githubusercontent.com/26560119/70390837-19a1f780-1a12-11ea-9247-f74b85af9d3a.png">

**대부분 하드웨어 접근 시, 가상화를 쓴다는 뜻**

### Disk I/O 처리하는 방법 

1. User Process 가 시스템 콜을 통해 커널 모드로 진입하고 디스크 IO request 를 커널의 disk IO request queue 에 걸어놓고 해당 프로세스를 Block (IO 해야 프로세스 다음 활동이 가능)

2. 커널 스케줄러는 요청을 읽고 IO 스케줄러를 부르고 IO 스케줄러는 시스템 상황에 따라 디바이스 드라이버들을 호출한다. 

3. 디바이스 드라버들은 디바이스 컨트롤러를 통해 Disk IO (Programmed IO) 를 수행하고, 너무 요청이 많은 경우 DMA 를 수행하여 효율적으로 처리한다.

4. IO 가 완료되면 디스크 장치가 인터럽트를 치고 커널의 인터럽트 핸들러가 작동되도록 함

5. 디스크 관련 동작의 경우, 장치의 컨트롤러에 있는 임시 저장 레지스터로부터 커널의 정해진 메모리에 읽은 데이터를 복사하고 종료

6. 스케줄러가 블락된 놈 ready queue 로 다시 옮겨서 처리

### Device Drivers 

<img width="610" alt="스크린샷 2019-12-10 오후 12 57 20" src="https://user-images.githubusercontent.com/26560119/70493901-acdf4800-1b4c-11ea-9b56-4bcd5fb3f68e.png">

- Block device driver
    - 고정 사이즈의 주소가 있는 데이터 (블록단위) 를 사용하는 디바이스의 드라이버 (하드디스크, 등)

- Character device driver

    - 자료의 순차성을 가지고 있는 하드웨어를 다룰 때 사용하며, 데이터를 문자 단위(또는 연속적인 바이트 스트림)로 전달하고 읽어들인다. 
    
    - 대표적인 하드웨어로는 터미널, 콘솔, 키보드, 사운드카드, 스캐너, 프린터, 직렬/병렬 포트, 마우스, 조이스틱 등이 있다. 

### Disk Device (HW)

1. HDD 
    - seek time, latency time, transfer time 등이 소요된다.

2. SSD
    - seek time 이 없어서 HDD보다 빠르다. 
    - seek time 은 실린더에 헤드 놓는 시간
    - Read 가 WRITE 보다 빠름
    - Fuse 때문에 쓰기작업 제한횟수가 있다.

### Block Accessing 을 위한 디스크 스케줄링

1. FIFO 
- 이론상으로는 별로인데, 실제로는 괜춘하다.

2. SSTF (shortest seek time first)
    - 짧은 탐색시간 걸리는놈부터 처리
    - 성능좋음
    - 그러나 밀리다가 starvation 가능성

3. SSTF + Aging 기법

4. 엘리베이터 알고리즘_스캔 (SSTF와 거의 동일)

5. 엘리베이터 알고리즘 + 배치

### Device Numbers

> Device driver 은 device controller 을 제어하고, 하나의 디바이스 컨트롤러는 두개 이상의 장치를 제어가능

- 따라서 Device Driver 은 디바이스 컨트롤러와 실제 device 를 지칠하기 위한 번호가 필요

Major number : 디바이스 드리이버 SW 모듈 ID
Minor number : Physical Device ID

### 파일 및 IO 디바이스 구조 

<img width="654" alt="스크린샷 2019-12-10 오후 1 02 28" src="https://user-images.githubusercontent.com/26560119/70494107-64745a00-1b4d-11ea-808d-4f25bf4b7a1d.png">

User Process 입장에서는 파일 인터페이스를 통해 파일 시스템에 접근한다. 그리고 디바이스 드라이버에 접근하기 위해서도 디바이스 인터페이스를 거치는데 그 종류는 아래와 같다 (이 디바이스 인터페이스가 OS[SW] 의 IO 시스템이다)

- block device interface
    - Disk Driver
    - CD Driver
- character device interface 
    - Character Disk Driver
    - Terminal Driver 
    - Ethernet Driver 

알맞는 디바이스가 선택 되었으면, 알맞는 디바이스 컨트롤러에 접근해서 간접적으로 디바이스를 컨트롤 할 수 있다.

### Generalized Disk Device Drivers 

- 파티셔닝 디스크
    - 하드 나누는 그 파티션 
    - 각 파티션 정보는 물리 디스크의 파티션 테이블에 저장
    - 각 파티션은 커널에게는 개별적인 logical disk 이고, device 인 만큼 minor number (device ID) 을 부여 받음

- 또는 디스크들을 하나의 큰 논리 디스크로 합치는 것 

- 파일로부터 만들어진 논리 디스크 

- 램 디스크 

### Disk Cache

> 디스크 드라이버에 접근해서 디스크 블록 요청하기 전에, 먼저 디스크 캐시에 들린다. 
쓰기같은 경우 일정 퍼센티지 

### Device Driver 의 2레벨 구조

<img width="682" alt="스크린샷 2019-12-10 오후 1 22 15" src="https://user-images.githubusercontent.com/26560119/70494821-1f055c00-1b50-11ea-95c8-f4557fb2e7d7.png">

드라이버는 디바이스 컨트롤러와 통신하는 소프트웨어로서 
두 PHASE 를 거친다.

- Upper Level
    - 커널이 디스크 읽기쓰기 요청 시 받아서    
    disk request queue 에 넣는거 까지

- Lower Level
    - 커널 디바이스 드라이버가 해당 큐의 요청 받아서 디스크 컨트롤러(HW) 에게 전달하는 것 

