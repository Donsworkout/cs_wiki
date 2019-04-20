# (6) Requirements Capture

### 1. Types of Requirements
* Functional 
  * 시스템이 할 것을 기술한 것, use case diagram 과 바로 맵핑이 가능
  * 시스템 output에 대한 상세 내용
* Non-Functional
  * Quality에 관련한 내용
  * performance criteria (성능)
  * anticipated volumes of data (기대되는 데이터 볼륨)
  * security considerations (보안 고려)
  * requirement list 에는 기술되나, diagram에서는 나타나지 않는다.
* Usability 
  * 사용자가 목표를 효율적이고 편하게 이루기 위한 정도
  * 유저의 특성, 유저가 해야할 것, 시스템에 대한 사용자의 기준

### 2. Requirements list 기술
![스크린샷 2019-04-20 오후 6 50 20](https://user-images.githubusercontent.com/26560119/56455731-2ac3c880-639d-11e9-815c-ce61e4c11b2e.png)

1. 이름을 기록하기 위해 이름, 주소, 연락처 정보들을 각 고객들에게 입력받아야 한다.
2. 각 클라이언트에게 각 캠페인의 상세 정보를 기록할 수 있어야 한다. 상세 정보에는 캠페인의 타이틀, 시작/종료 일, 산정 비용, 예산, 실제 비용과 날짜, 그리고 완성을 위한 현재 상태등을 포함하고 있다. (상세히 기술)

### 3. Use Case Diagram 그리기
> **유저 관점** 에서 시스템의 funtionality를 기술하기 위한 목적

#### 3-1) Notation (기본)
![스크린샷 2019-04-20 오후 6 55 17](https://user-images.githubusercontent.com/26560119/56455788-e684f800-639d-11e9-95b7-8f7fb311ec21.png)

* Actor은 사람이나 다른 시스템이 될 수도 있다. 
* Use case는 타원형이며 동사 형태로 적어야 한다.
* Association은 actor 와 use case 간의 선이다
  * 처음 통신시 arrow point 를 그려 주는것이 미덕

#### 3-1) Notation (Dependancies)
![스크린샷 2019-04-20 오후 6 58 58](https://user-images.githubusercontent.com/26560119/56455818-93f80b80-639e-11e9-9f49-d34d378818ea.png)
> Use case 간 특별한 dependencies를 stereotypes 로 표현한다.
1. extend  
extend는 어떠한 use case 뒤에 선택적인 옵션을 표현한다.
2. include  
include는 use case들이 공통적으로 가지고 있는 어떤 부분을 모듈로 뺐다고 보연 된다.

### 4. Use Case Description 
![스크린샷 2019-04-20 오후 7 04 10](https://user-images.githubusercontent.com/26560119/56455865-2e584f00-639f-11e9-9fcb-c3ba3e9c814d.png)

> step by step breakdown 으로 간결하게 actor 과 system의 상호작용을 기술한다.  

