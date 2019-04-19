# (1, 4) Software Process

### 1. Software Process
> A set of ***activities*** whose goal is the development or evolution of software  
목표가 소프트웨어의 개발이나 혁신인 activities 의 집합

> Problem to SW product 까지 과정들이 모두 ***activities***

#### 1-1. Generic Activities
1. **Specification**  
What the system should do and its development constraints  
***' 시스템이 무엇을 해야 하는지와, 개발 제약사항을 정의하는 단계'***

2. **Development (Design and Implementation)**  
Production of Software System  
***' 소프트웨어 시스템의 생산 '***

3. **Validation**  
Checking that the software is what the customer wants  
***' 고객이 원하는 소프트웨어가 맞는지 체크하는 것 '***

3. **Evolution**  
Changing the software in response to changing demands  
***' 변화하는 요구에 맞추어 소프트웨어를 변경하는 것 '***

### 2. Generic Software Process Models
* An abstract representation of a process  
프로세스의 추상적 표현 
* A description of a process from some particular perspective  
특별한 관점에서 본 프로세스에 대한 설명 

#### 2-1. The waterfall model
<img width="666" alt="스크린샷 2019-04-19 오후 7 17 00" src="https://user-images.githubusercontent.com/26560119/56420153-d0146900-62d7-11e9-8b1c-976b9277452f.png">

> Generic activities (Specification, Development, Validation, Evolution) 단계들을 철저히 분리한 모델이다. 
* 장점 
  * Documentation 이 각 phase 마다 생산된다.
* 단점 
  * Inflexible한 프로젝트 partitioning은 요구사항의 변화에 대응하기 힘들다.
* 정리
  * 요구사항이 그렇게 완벽한 시스템은 아주 적다
  * 단계들이 항상 서로 분리되어 있지는 않다.
  * 요구사항이 완벽히 파악되었거나, 디자인 프로세스에서 변화를 제한한다면 적절하다.

#### 2-2. Evolutionary Development
<img width="728" alt="스크린샷 2019-04-19 오후 7 23 51" src="https://user-images.githubusercontent.com/26560119/56420384-b32c6580-62d8-11e9-8687-094215abedc8.png">

> 초기 구현에서 발전시켜 나가며, 사용자의 피드백을 받으며 발전한다. 많은 버전으로 개발을 한다.

* 2 Fundamental types of evolutionary development
  * Exploratory Development  
  잘 이해된 요구사항으로 시작하여, 새로운 기능들을 고객에게 제안 받는다.
  * Throw-away Prototyping  
  잘 이해되지 않는 요구사항으로 시작하여 개발하며 요구사항을 명확히 하는 방법 

* 장점 
  * 유저들이 시스템을 점점 더 이해함에 따라 Specification 이 점점 더 발전할 여지가 있다. 
  * Specification 의 불확실함을 해소하기가 쉽다.
* 단점 
  * 프로세스 가시성이 부족하다.
  * 시스템 구조가 별로일 가능성이 높다.
  * Rapid prototyping 등 특별한 기술이 필요하다.
* 적용
  * 작은 시스템
  * 구현 전에 예측하는 못하는 큰 시스템의 서브시스템 (UI 등)

#### 2-3. Component Based SW engineering
<img width="1091" alt="스크린샷 2019-04-19 오후 7 36 11" src="https://user-images.githubusercontent.com/26560119/56420797-68abe880-62da-11e9-9967-a88e115a88fd.png">

> 재사용 지향적이며, 사용할 수 있는 컴포넌트를 기반으로 SW를 설계한다.  
컴포넌트에 맞추어 요구사항을 수정하는것이 특징

* 장점 
  * 개발해야 할 SW를 줄여 비용 절감
  * 빠른 딜리버리가 가능하다.
* 단점 
  * 요구사항 타협이 불가피하여, 원하는 것이 아닐 가능성이 있다.
  * 재사용 가능한 컴포넌트가 못쓰게 되면, 새로운 버전 업데이트가 불가능하다.
* 정리
  * 요구사항이 그렇게 완벽한 시스템은 아주 적다
  * 단계들이 항상 서로 분리되어 있지는 않다.
  * 요구사항이 완벽히 파악되었거나, 디자인 프로세스에서 변화를 제한한다면 적절하다.