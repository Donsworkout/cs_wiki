# (5) Modeling Concept

### 0. Model
> 모델은 실제나 가상의 어떠한 것을 추상화 한 것이다. 시스템 개발 모델은 대부분 diagram으로 나타내어 진다.

### 1. Diagram
> 다이어그램은 규칙이나 표준을 따르며, 실세계의 것을 표현할때 쓰이는 추상적 모양이다.
* 다이어그램은 모델의 한 부분을 시각적으로 표현한 것이다.
* 다이어그램은 지나치게 자세한 부분은 표현하지 않는다.

### 2. UML (Unified Modeling Language)
> 객체 지향적 접근으로 소프트웨어 시스템의 구조를 시각화하는 그래피컬한 언어이다.

#### 2-1) UML 구성 요소
![스크린샷 2019-04-20 오후 5 38 49](https://user-images.githubusercontent.com/26560119/56454921-2f837f00-6393-11e9-8590-d42a722c396d.png)


### 3. UML - Activity Diagram
> 목적
* Use case 애 의해 표현되는 시스템의 기능을 표현하기 위해
* 로직이나 operation 을 표현하기 위해

**Notation 1**

![스크린샷 2019-04-20 오후 5 46 58](https://user-images.githubusercontent.com/26560119/56455029-863d8880-6394-11e9-9ea2-f4dd2450220c.png)

**Notation 2**
>Decision Node  
[Guard Condition] 에 따라 분기했다가 다시 merge 된다.

![스크린샷 2019-04-20 오후 5 48 22](https://user-images.githubusercontent.com/26560119/56455030-863d8880-6394-11e9-9e30-ead14be940d9.png)

**Activity Diagram 모양**

![스크린샷 2019-04-20 오후 5 51 18](https://user-images.githubusercontent.com/26560119/56455068-eb917980-6394-11e9-93d6-974875546a90.png)

**Notation 3**
> Fork / Join Nodes  
병렬로 일어나는 액션에 대해 표현할 수 있다.

![스크린샷 2019-04-20 오후 5 56 08](https://user-images.githubusercontent.com/26560119/56455117-b20d3e00-6395-11e9-8e18-4d1477c8ac0a.png)

**Notation 4**  
* Sequences (순차 flow)
* Selections (if / else)
* Iterations (iterate)

**Notation 5**

* 객체를 표현할때 Operation 과 Class Name을 써서 Action name 처럼 쓸 수 있다.

![스크린샷 2019-04-20 오후 6 12 59](https://user-images.githubusercontent.com/26560119/56455343-02859b00-6398-11e9-9fd4-b4a2fc47cda8.png)


**Notation 6**

* 한 action 에 대해 input / output 이 될 수 있다.
* 직사각형 형태이며, [Bracket] 으로 객체의 상태를 나타낼 수도 있다.

![스크린샷 2019-04-20 오후 6 13 15](https://user-images.githubusercontent.com/26560119/56455344-02859b00-6398-11e9-8ed5-32fb899fc0b3.png)

**Notation 7**  
* Activity Partition 으로 책임을 구분할 수 있다.

![스크린샷 2019-04-20 오후 6 17 17](https://user-images.githubusercontent.com/26560119/56455392-a7a07380-6398-11e9-95a2-f40eab7e1a0d.png)

### Activity 그리는 과정 정리
1. Actions 를 나열해 보고 flow에 따라 쭉 나열해 본다.  
<img style="margin-top:10px; height:300px;" src='https://user-images.githubusercontent.com/26560119/56455410-f6e6a400-6398-11e9-9f2a-57e9c6aade90.png'/>

2. 분기처리 할 부분을 decision / merge 노드 / 가드 조건 으로 구성한다 
<img style="margin-top:10px; height:400px;" src='https://user-images.githubusercontent.com/26560119/56455439-5775e100-6399-11e9-8aeb-467bccfbeb0e.png'/> 

3. Pararel 될 수 있는 부분을 fork / join nodes 로 표현한다.
4. 반복되는 부분이 있나 체크하고 반영한다.  
<img style="margin-top:10px; height:400px;" src='https://user-images.githubusercontent.com/26560119/56455479-bf2c2c00-6399-11e9-96fb-4daf38108595.png'/>

5. Activity Partition 으로 주체를 구분한다  
<img style="margin-top:10px; height:400px;" src='https://user-images.githubusercontent.com/26560119/56455506-0d412f80-639a-11e9-8423-cb606dfc4643.png'/>

6. 마지막으로 객체를 표현한다. (탄생되는 객체 등)
<img style="margin-top:10px; height:400px;" src='https://user-images.githubusercontent.com/26560119/56455508-0dd9c600-639a-11e9-9c80-ba399dd8aa67.png'/>
