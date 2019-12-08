### 파일

> 파일이란, 정보를 담고 있는 임의 길이의 순차 바이트 배열

### 파일 메타데이터

> 타입, 크기, 오너, 생성 시각 등 파일에 관한 정보

- 파일에 관한 정보를 담은 오브젝트를 FCB (File Control Block) 이라고 부르며, 파일을 생성할 때, 커널이 디스크에 생성해 저장한다.

### 커널에 의해 관리되는 파일시스템

<img width="546" alt="스크린샷 2019-12-09 오전 12 12 28" src="https://user-images.githubusercontent.com/26560119/70391434-9a63f200-1a18-11ea-9a4f-3c0aad7d9e1d.png">

### 디렉터리 구조

- Single Level

- Two Level

- Tree Structured directories

> 디렉터리도 파일의 한 종류로 취급하는 것이 일반적 

### Disk Block 할당 방식 

<img width="788" alt="스크린샷 2019-12-09 오전 12 16 01" src="https://user-images.githubusercontent.com/26560119/70391479-1cecb180-1a19-11ea-9c84-7043cec4953c.png">

> 인덱스 방식은 인덱스 전용 블럭이 존재하는 것 

