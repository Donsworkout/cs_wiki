# (4) What is Object Orientation?

### 0. Object
![스크린샷 2019-04-20 오후 2 06 08](https://user-images.githubusercontent.com/26560119/56453010-7c585d00-6375-11e9-8adc-bac1e4e65f55.png)
> An abstraction of sth in problem domain, reflecting the capabilities of the system to  
* keep information about it
* interact with it
* or boths
> ***정보를 저장하고, 그것과 상호작용 하기위한 시스템의 능력을 추상화 한 것***

### 객체 지향 프로그래밍의 컨셉
### 1. Abstraction (추상화)
> A form of representation that includes only **what is important or interesting** from a particular viewpoint.  
***특정 시점에서 중요하거나 흥미로운 것만 모아놓은 표현의 형태***  
쉽게 말하면, ***필요한 특징만 뽑는 것***

#### 1.1) Class and Instance
![스크린샷 2019-04-20 오후 4 53 42](https://user-images.githubusercontent.com/26560119/56454540-62c30f80-638d-11e9-86f0-4159bd9ebf12.png)
> Person으로부터 p라는 인스턴스를 만들었다.
* 모든 객체는 클래스의 인스턴스이다.
* 특징, 의미론, 제약 등을 담는다.
* 객체들 간의 유사성을 추상화 시켜 놓은 것이다.
* 한 클래스의 인스턴스들은 구조나 Behaviour이 같다.

### 2. Generalization and Specialization
> 계층 구조, superclass와 subclass가 존재한다.  
공통 사항은 subclass에도 존재하지만, Specialization은 추가 정보를 더 제공한다.

**OOP 에서 Inheritance가 이 부분이다** 

![스크린샷 2019-04-20 오후 4 58 43](https://user-images.githubusercontent.com/26560119/56454552-9b62e900-638d-11e9-83eb-5b49022bc5cf.png)

> 보이다시피 Superclass로 부터 생성자는 물려받지 않아, 생성자를 새로 정의해 주어야 한다.

1. 서브클래스는 그것의 슈퍼클래스의 모든것을 상속받는다.
2. 서브클래스는 슈퍼클래스로부터 상속 받은것 제외, 최소 하나라도 디테일이 추가되어야 한다.

### 3. Messange-passing
* 각각의 객체들은 각각의 시스템 액션을 충족시키기 위해 collaborate 한다.
* 이 객체들은 서로에게 메시지를 보내어 소통한다.
* 소통할때, 상대방의 디테일은 알 필요가 없다.

> 읽자마자 생각나는 단어가 있다. 바로 **'Encapsulation'**

#### 3-1) Encapsulation
![스크린샷 2019-04-20 오후 4 31 52](https://user-images.githubusercontent.com/26560119/56454327-d400c380-6389-11e9-8ff6-1ecf16708db8.png)

* Data Binding
  * to bind data and functions together
* Data Hiding
  * to keep data/functions from outside interference and misuse

**cf)** Access Modifier
* Public : 누구나 접근 가능
* Private : 클래스 내부 함수에서만 접근 가능
* Protected : Private과 비슷하나, 상속받은 클래스도 이용할 수 있음

> 아래의 예시를 보면, 인스턴스의 변수를 직접 바인딩 할 수 없도록 protected 하고, 멤버 함수로서 할당할 수 있게 한 것을 볼 수 있다.

![스크린샷 2019-04-20 오후 4 56 16](https://user-images.githubusercontent.com/26560119/56454700-fc8bbc00-638f-11e9-86f6-d8a44a3b534f.png)

### 4. Polymorphism
> Sender은 메시지를 그냥 전달만 하고, Receiver 가 class type 에 따라 결과가 달라지는 것  

#### 4-1) Overriding
> 오버라이딩은 클래스 외적인 개념이라고 보면 됨, 상속받은 클래스가 슈퍼클래스의 멤버함수를 덮어 씌우는 것이 Overriding

![스크린샷 2019-04-20 오후 5 01 47](https://user-images.githubusercontent.com/26560119/56454580-014f7080-638e-11e9-844a-407af0e7bab9.png)

>위를 보면 기존 Person 에서 만든 정보출력 함수를 쓰지 않고, 재 정의해서 쓰고 있다.  
이것이 바로 오버라이딩이다.

#### 4-2) Overloading
> 오버로딩은 클래스 내적인 개념이라고 보면 됨, 파라미터나 클래스 타입에 따라서 실행되는 함수가 달라짐

![스크린샷 2019-04-20 오후 4 56 39](https://user-images.githubusercontent.com/26560119/56454567-ce0ce180-638d-11e9-8e6c-6eff47da0715.png)

> 위를 보면 생성자를 오버로딩 하는것을 알 수 있다.

### 5. 가상함수 (Virtual Function)
> 가상함수는 베이스 클래스에서 정의되는데, 이는 컴파일 시간이 아니라 런타임에 해당 클래스를 판단하여 함수를 바인딩 시킨다.

#### 아래와 같은 상황을 가정해 보자

~~~cpp
Base *bp = new Derived;
bp -> func();
~~~
* Base 클래스 포인터 bp로 Sub 클래스를 가리킬 때 가상함수를 쓰지 않으면, 당연히 베이스 포인터이기 때문에 Base 클래스의 멤버함수를 호출한다.

* 그러나 아래와 같이 가상함수를 쓰면
~~~cpp
class Derived: public Base{
  public:
    virtual void func();
};
~~~
* 런타임에 함수가 바인딩 되어, 포인터가 상속된 클래스의 객체인지 확인하고 Derived의 func가 실행되게 된다. 

#### 또다른 예시 코드
![스크린샷 2019-04-20 오후 5 11 38](https://user-images.githubusercontent.com/26560119/56454663-6d7ea400-638f-11e9-936b-42db893f9540.png)
