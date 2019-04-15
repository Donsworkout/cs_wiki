# (7) Requirement Analysis

### 1. Class Diagram 을 만들기 위한 두 모델
1) Domain Model  
TOP-DOWN 방식으로, 어플리케이션 도메인 지식으로부터 직접 구성하는 방법
2) Analysis Class Model  
각 유즈케이스마다 각각의 Class Diagram을 그리고, 나중에 이것을 조립해서 Single Model 을 만듬

### 2. Use Case Realization
<img width="941" alt="스크린샷 2019-04-15 오후 11 33 49" src="https://user-images.githubusercontent.com/26560119/56141026-fa63df00-5fd6-11e9-876b-2d6e5a690b39.png">
  
  - Use case diagrams 에서 Class diagrams 까지의 과정
  - **Use case Diagrams -> Collaboration -> Communication Diagram -> Class diagram**

### 3. Use Case diagrams to Class diagrams
1) Use Case Diagrams

<img width="461" alt="스크린샷 2019-04-15 오후 11 37 28" src="https://user-images.githubusercontent.com/26560119/56141314-91309b80-5fd7-11e9-9694-139ac385f185.png">

2) Collaboration 

<img width="712" alt="스크린샷 2019-04-15 오후 11 37 41" src="https://user-images.githubusercontent.com/26560119/56141318-9392f580-5fd7-11e9-9db8-bc026da9d20f.png">

- **:AddAdvert** 와 같이 클래스명 앞에 colon 붙은 것은, 해당 클래스로 부터 파생된 임의의 인스턴스를 말하는 것임
- 인터랙션의 순서나 디테일한 메시지는 나와있지 않지만, 어떤 Object가 어떤 Object와 관계를 맺고 있는지 정도를 파악할 수 있게 해준다.

3. Communication Diagrams

<img width="715" alt="스크린샷 2019-04-15 오후 11 37 57" src="https://user-images.githubusercontent.com/26560119/56141334-9857a980-5fd7-11e9-8429-62e1f256c7f0.png">

- 메시지의 내용과 Sequence 까지 표시된다. 
- 메시지의 Sequence number 옆에 붙은 '*' 표시는 여러번 요청 할 수 있다는 뜻이다. (iterate 같은 경우)

4. Class Diagrams

<img width="567" alt="스크린샷 2019-04-15 오후 11 46 41" src="https://user-images.githubusercontent.com/26560119/56141935-bf62ab00-5fd8-11e9-800e-5752b57e3274.png">

- 클래스 다이어그램 맨 윗단을 보면 Object 의 이름앞에 colon 이 없다. 이는 클래스를 나타낸다.
- 그리고 User Interface::AddAdvertUI 는 패키지를 의미한다. (유저 인터페이스의 패키지)
- 다이어그램의 구성을 보면, 맨 윗단은 이름, 두번째는 attributes, 세번째는 operations 다.
- 그리고 클래스간 숫자가 나와있는 경우가 있는데, 이는 관계에서의 연관 갯수를 의미한다  
그림의 0..* 같은 경우는 1개의 left class 에 0개부터 여러개의 right class 들이 연결될 수 있다는 뜻


그리고 떄로는 ,, 아래와 같이 한 유즈 케이스에서 여러 기능으로 나누고 이를 파샬로 나타낼 수 도 있다.

<img width="716" alt="스크린샷 2019-04-15 오후 11 52 33" src="https://user-images.githubusercontent.com/26560119/56142380-8ecf4100-5fd9-11e9-9ca8-7b25e7233e87.png">

### 4-1) Class Diagrams - Stereotypes
하나의 유즈케이스에서 클래스들은 역할에 따라 3가지의 Streotypes 로 나누어질 수 있다.  

1. Boundary objects  

<img width="625" alt="스크린샷 2019-04-16 오전 1 35 21" src="https://user-images.githubusercontent.com/26560119/56149650-fdb39680-5fe7-11e9-85e5-506eaaffcd05.png">

- 액터와 시스템 사이의 인터렉션을 모델링 한다.

2. Entity objects  

<img width="577" alt="스크린샷 2019-04-16 오전 1 35 28" src="https://user-images.githubusercontent.com/26560119/56149651-fdb39680-5fe7-11e9-8e7b-f5d61c5a0d50.png">

- 어플리케이션 도메인에서, information 과 behaviour 를 표현한다. 

3. Control objects  

<img width="587" alt="스크린샷 2019-04-16 오전 1 35 34" src="https://user-images.githubusercontent.com/26560119/56149652-fe4c2d00-5fe7-11e9-8cd0-7ec07364d9db.png">

- 유즈케이스가 잘 돌아가도록 다른 objects 들을 maintain 하는 역할이다. 유즈케이스당 한개 이상이 있어야 한다.


### 4-2) Symbols

1. Class symbol

<img width="684" alt="스크린샷 2019-04-16 오전 1 39 24" src="https://user-images.githubusercontent.com/26560119/56149894-91856280-5fe8-11e9-8714-4b12c68704c4.png">

2. Instance symbol

<img width="659" alt="스크린샷 2019-04-16 오전 1 39 29" src="https://user-images.githubusercontent.com/26560119/56149895-91856280-5fe8-11e9-9259-7d7d3ee501ca.png">

- 인스턴스는 operations 가 없고, attr 과 그 값만 존재한다.

### 4-3) Link / Associations / Multiplicity

<img width="679" alt="스크린샷 2019-04-16 오전 1 43 30" src="https://user-images.githubusercontent.com/26560119/56150117-0f496e00-5fe9-11e9-990b-736eb711b6b2.png">

- Associations 는 클래스간 링크를 나타낸다. 
- 데이터베이스의 1:N 이런 관계같이 위 그림의 숫자를 보면 Multiplicity 에 대해 알 수 있음
1) 정확히 한 staff 는 각각의 클라이언트에게 연결되어 있다.
2) staff 멤버는 0 에서 다수의 클라이언트들과 연결될 수 있다.

### 5. Identifying classes
1) 하나의 Use Case 로 시작한다
2) Use Case 콜라보레이션에 포함되어 있는 객체들을 밝힌다.
3) Communication Diagram 그린다.
4) 이러한 콜라보레이션을 Class Diagram으로 바꾼다. 
5) 많은 Use Case Diagram은 하나의 합쳐진 analysis class diagram 으로 바뀔 수 있다.
6) 다른 Use case diagram 에도 적용한다.

### 6. CRC cards
<img width="563" alt="스크린샷 2019-04-16 오전 1 52 52" src="https://user-images.githubusercontent.com/26560119/56150685-5dab3c80-5fea-11e9-8475-68ed061ef130.png">

- 책임과 콜라보레이터를 기술해 놓은 카드이다 
- Class Responsibility Collaboration card