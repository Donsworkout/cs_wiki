### Intel CPU - Device connection

![스크린샷 2019-12-08 오후 4 53 35](https://user-images.githubusercontent.com/26560119/70386272-4b4b9c00-19db-11ea-8aa1-ae8e0b207fb1.png)

- 디바이스 컨트롤러 : IO 제어에 필요한 최소 명령어 셋을 포함하는 특수목적 컴퓨터

- 커널은 컨트롤러를 통해 IO device 를 간접 컨트롤한다 
  - IO intructions
  - Memory Mapped IO - 메모리 주소에 매핑된 디바이스 컨트롤러 레지스터 

- 디바이스 드라이버 : 컨트롤러의 레지스터로부터 읽고, 레지스터에 쓰기 위한 커널 코드 

- 폴링 IO / 인터럽티 기반 IO 

#### DMA steps

![스크린샷 2019-12-08 오후 5 29 33](https://user-images.githubusercontent.com/26560119/70386797-7be20480-19e0-11ea-814a-e646ad6d15e9.png)

1. 디바이스 드라이브는 x 메모리 주소 버퍼에 디스크 데이터를 옮기도록 명령 

2. 디바이스 드라이버는 디스크 컨트롤러에게 C bytes 를 디스크에서 X 주소로 쓰라고 명령

3. 디스크 컨트롤러는 DMA 전송 초기화 

4. 디스크 턴트롤러는 DMA 컨트롤러에서 바이트 전송 

5. DMA 컨트롤러는 버퍼 X 에 바이트 전송한다. (C가 0이 될때까지)

6. C=0 이되면 DMA는 전송완료 시그널을 위해 CPU 인터럽트  

### Two stage communication

<img width="822" alt="스크린샷 2019-12-08 오후 11 25 37" src="https://user-images.githubusercontent.com/26560119/70390837-19a1f780-1a12-11ea-9247-f74b85af9d3a.png">

### I/O 처리하는 방법 

1. User Process 가 시스템 콜을 통해 커널 모드로 진입하고 디스크  IO request 를 커널의 disk IO request queue 에 걸고 해당 프로세스를 Block (IO 해야 프로세스 다음 활동이 가능)

2. 커널은 스케줄러를 호출하여 시스템 상황에 따라 디바이스 드라이버들을 호출하고 디바이스 컨트롤러를 통해 Disk IO를 수행하고, 너무 요청이 많은 경우 DMA 를 수행하여 효율적으로 처리한다.

3. IO 가 완료되면 디스크 장치가 인터럽트를 치고 커널의 인터럽트 핸들러가 작동되도록 함

4. 디스크 관련 동작의 경우, 장치의 컨트롤러에 있는 임시 저장 레지스터로부터 커널의 정해진 메모리에 읽은 데이터를 복사하고 종료

5. 스케줄러가 블락된 놈 ready queue 로 다시 옮겨서 처리

### Device Numbers

> Device driver 은 device con
troller 을 제어하고, 하나의 디바이스 컨트롤러는 두개 이상의 장치를 제어가능

- 따라서 Device Driver 은 디바이스 컨트롤러와 실제 device 를 지칠하기 위한 번호가 필요

Major number : 디바이스 드리이버 SW 모듈 ID
Minor number : Physical Device ID

