### 파일

> 파일이란, 정보를 담고 있는 임의 길이의 순차 바이트 배열  
디바이스 드라이버 보다 파일 시스템이 상위 레벨임 (SW)

### 파일 메타데이터 (FCB, 리눅스에서는 파일 디스크립터)

> 타입, 크기, 오너, 생성 시각 등 파일에 관한 정보

- 파일에 관한 정보를 담은 오브젝트를 FCB (File Control Block) 이라고 부르며, 파일을 생성할 때, 커널이 **Disk** 에 생성해 저장한다.

### 커널에 의해 관리되는 파일시스템

<img width="546" alt="스크린샷 2019-12-09 오전 12 12 28" src="https://user-images.githubusercontent.com/26560119/70391434-9a63f200-1a18-11ea-9a4f-3c0aad7d9e1d.png">

- PCB 에는 open file pointer array 가 있어서 프로세스가 사용하는 open file pointer 들을 볼 수 있다.

- open file pointer 들은open file table 엔트리에 연결됨 

- 그리고 그 엔트리는 file descriptor table (FCB table, 인메모리에 위치, Disk FCB 카피) 의 엔트리에 연결된다.

- 그리고 그 FCB 엔트리에는 파일 데이터가 연결되어 있음

### 파일 Read 작업 시퀀스 

<img width="700" alt="KakaoTalk_Photo_2019-12-10-14-22-39" src="https://user-images.githubusercontent.com/26560119/70498052-8b845900-1b58-11ea-9f3e-9524ee95c6d3.png">

**위 그림 다시 그려볼것**

### 디렉터리 구조

- Single Level

- Two Level

- Tree Structured directories

> 디렉터리도 파일의 한 종류로 취급하는 것이 일반적 

### Disk Block 할당 방식 

<img width="504" alt="스크린샷 2019-12-10 오후 2 24 32" src="https://user-images.githubusercontent.com/26560119/70498136-cd150400-1b58-11ea-8274-15cf26e24e99.png">

<img width="788" alt="스크린샷 2019-12-09 오전 12 16 01" src="https://user-images.githubusercontent.com/26560119/70391479-1cecb180-1a19-11ea-9c84-7043cec4953c.png">

> 인덱스 방식은 인덱스 전용 블럭이 존재하는 것 

> INODE 방식의 INODE 는 파일과 연결된다. inode 는 다텍터리, 파일과 실제 데이터를 연결해주는 인터페이스와도 같다.

### Free block 할당 방법

- 비트 맵
- 기타
    - Linked list 
    - Grouping
    - Counting
    - Space map (ZFS)


### (디스크) 버퍼 캐시

<img width="481" alt="스크린샷 2019-12-10 오후 3 25 38" src="https://user-images.githubusercontent.com/26560119/70501409-57616600-1b61-11ea-9579-b195a2ea90fb.png">

> 디스크를 읽는 것은 메모리 읽는 것보다 너무 느리다.  
동일한 영역을 짧게 짧게 읽는 경우가 많으므로 메모리에 넣어놓는 것이다.
읽기는 캐시에서 write는 버퍼에 잠시 담아두었다가 배치처럼 나중에 처리하는 방식 

### WAFL (write anywhere file layout)

>  NFS, CIFS, http, ftp 에 적용 