# (9) Object Interaction

### 1. Object Messaging
  오브젝트간 메시징은 operation 과 같다고 보면 됨  
  아래의 코드도 메시지를 날리는 것임
~~~cpp
  currentadvertCost = anAdvert.getCost()
~~~

### 2. Interaction & Collaboration
#### Collaboration
콜라보레이션은 functionality 나 behaviour를 제공하기 위해 협력하는 객체나 클래스 그룹을 말함
#### Interaction
인터랙션은 콜라보레이션의 맥락에서 특정한 behaviour을 얻기 위해 객체간 메시지 패싱하는 것을 정의함  
인터렉션은 다양한 notation을 통해 모델링 될 수 있다. 그것들은 아래와 같음 
1. Sequence diagrams
2. Communications diagrams
3. Interaction overview diagrams
4. Timing diagrams

### 2.1) Sequence Diagrams
#### Communication 다이어그램과 같은 것을 나타냄  
- 시간 순에 따라 인터랙션 나타냄
- 개발 생명 주기에 따라 다양한 목적을 만족시키기 위해 각각 다른 레벨의 디테일로 표현됨
- 한 **오퍼레이션** (메소드 등)의 인터랙션을 상세히 나타냄

#### 아래는 Sequence Diagram

<img width="958" alt="스크린샷 2019-04-15 오후 9 35 58" src="https://user-images.githubusercontent.com/26560119/56133057-83264f00-5fc6-11e9-983b-b03b5c771a6d.png">

- 수직선은 시간을 나타냄
- 인터랙션 하는 오브젝트들은 lifelines 위에 표현됨 (수직 점선)
- Message는 수평 실선으로 나타냄 (reply 메시지도 추후에 나옴)
- Operation의 Execution 이나 Activation 은 lifeline 위 직사각형으로 표시함
- Iteration 은 combined fragment 에 의해 표현된다  
 **loop** 이라는 **interation operator** 가 써져 있는 직사각형을 참고하면 됨
- loop combined fragment 는 **interaction constraint** 의 **[ guard condition ]** 을 충족시켜야만 실행된다.
- 그리고 그림 우측 하단을 보면 Advert 생명선으로 향하는 점선을 볼 수 있는데, 이것은 객체 생성하는 것을 나타낸다. 보이다시피 message 이름이 클래스 이름과 동일한 것을 보아 이것이 **생성자**를 나타내는 것임을 알 수있다.

### 2.2) Boundary & Control classes 
- 대부분 유주케이스에는 하나의 boundary object가 있다. 이것은 액터와 시스템의 메시징을 담당하는데, 쉽게 말하면 UI다! 아래 그림에서는 **:AddAdvertUI** 를 참고하면 됨
- control object는 전체적인 object communication을 담당하는데, 아래 그림에서 **:AddAdvert** 를 참고하면 됨, 모든 플로우가 이걸로 부터 시작 됨  
*예를 들어, Actor가 AddAdvertUI 에 접근하고 싶어도 먼저 클라이언트들을 들고오지 않으면 아무것도 할 수 없다. 그런데 control object 인 **AddAdvert**가 먼저 getClient 로 Client 클래스와 메시지를 주고받고 클라이언트 목록을 가져온다면, 비로소 인터페이스가 시작되는 것이다.*  


<img width="893" alt="스크린샷 2019-04-15 오후 9 54 51" src="https://user-images.githubusercontent.com/26560119/56134208-1f515580-5fc9-11e9-9c9a-fea4d219a880.png">

### 2.3) Object Destruction 
- object 삭제 요청이 들어오면, lifeline을 끊고 Object를 파괴함, 그리고 파괴되는 시점 X로 나타낸다.  

<img width="520" alt="스크린샷 2019-04-15 오후 9 57 05" src="https://user-images.githubusercontent.com/26560119/56134376-6d665900-5fc9-11e9-9b9c-cf988987e5c7.png">

### 2.4) Reflexive Message
- 자기한테 보내는 메시지, 원래 활성화된 Activation이 있었다면, 화살표로 시작하여 Activation 직사각형에 덧대어 표현한다.

<img width="740" alt="스크린샷 2019-04-15 오후 9 59 15" src="https://user-images.githubusercontent.com/26560119/56134530-bf0ee380-5fc9-11e9-9c80-3e9005783f1f.png">

### 2.5) Focus of Control / Reply Messagee

<img width="815" alt="스크린샷 2019-04-15 오후 10 03 09" src="https://user-images.githubusercontent.com/26560119/56134800-478d8400-5fca-11e9-9501-88f69e4e17b6.png">

- 기존 Activation에 검은색 칠하면 그것은 타임라인에 따라 누가 제어를 갖고 있는지 표현한다.  
Activation 도중 다른 Class 와의 인터랙션을 통해 제어를 다른 클래스에 넘겨주고 기다리는 등의 상황을 shadow로 나타낼 수 있는 것이다.

- 그리고 제어를 다시 요청한 클래스로 돌리는 과정에서 reply 메시지가 발생할 수 있다. (optional)  
이 메시지는 보이다 시피 점선으로 나타낸다.