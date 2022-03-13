## 객체지향 프로그래밍 (Object Oriented Programming)

자바는 **객체지향언어**이고, 자바를 통해 사용자는 객체지향 프로그래밍을 할 수 있다.

### 객체지향언어

객체지향언어는 **동일한 목적이나 기능을 하는 변수와 함수들을 각각 하나로 묶어서 object(객체)로 만들고 그 객체들끼리 상호 통신하면서 프로그램 전체가 돌아가도록 코드를 구성**한다.

즉, 순차적인 흐름에 따라 코드를 작성하는 절차지향언어와 달리 객체지향언어는 실제 사물(객체)와 유사하게 코드를 유기적으로 작성한다.

> 객체라는 하나의 큰 틀을 만들고 그 속성들을 지닌 존재들을 만들어내는 것

객체지향언어는 모든 데이터를 하나의 물체 취급. 프로그램이 길어지고 복잡해지면 변수(숫자가 들어가는 자리)도 많아지고, 코드 또한 길어져서 해석하는데 시간이 오래 걸림

객체지향 프로그래밍을 활용하면 어떤 오류가 발생했을 때 프로그램 전체를 살펴보지 않고 각 객체들을 살펴보면서 수정가능 또한 이렇게 함으로서 프로그램의 크기도 줄일 수 있다.

> 객체 지향 프로그래밍이란 결국 우리가 커다란 프로그램 안에서 작은 클래스들의 변수를 다양하게 활용하여 커다란 프로그램 내부에선 별다른 코드를 적어주지 않는 것을 말한다.

<br>

### 객체지향언어의 이점

객체지향언어는 `상속`, `캡슐화`, `추상화`라는 개념을통해 더욱 구체화되었으며, 이러한 객체지향언어를 통한 프로그래밍은 다음과 같은 이점을 가진다.

1. 코드의 재사용성 증가

   - 새로운 코드를 작성 시 기존의 코드 재활용 가능

2. 코드의 관리가 용이(유지보수에 유리)

   - 코드 간의 관계를 최대한 활용하여 수정의 범위를 줄임

3. 프로그래밍의 신뢰성 증가

   - 코드의 중복을 방지하여 오동작 방지

결과적으로 객체지향 프로그래밍은 **코드의 재사용성을 높이고 유지보수를 쉽게함**으로써 효율적인 개발을 가능하게 함.

<br>

### 객체지향의 5원칙(클래스 설계 원칙) : S O L I D

1.  S : Single Responsibility Principle

단일 책임 원칙으로 하나의 클래스는 하나의 책임만이 필요함을 의미.(ex. MVC 패턴)
단일 책임 원칙을 사용하면 책임 영역이 확실해져 한 책임에서의 변경이 다른 책임으로의 변경으로까지 번지는 것을 방지할 수 있음.

<br>

2.  O : Open-Closed Principle

개방 폐쇄 원칙, 소프트웨어 요소 확장에는 열려 있으나 변경에는 닫혀 있어야 함. 이는 추가적인 기능이 필요해도 기존에 존재하는 기능을 수정할 것이 아니라 이를 재사용하여 확장된 기능을 추가해야함을 의미. **높은 응집도와 낮은 결합도**

<br>

3.  L : Liskov Substitution Principle

리스코프 치환 원칙은 프로그램의 객체는 프로그램의 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야 한다는 것을 의미한다. 이 말은 하위 타입은 언제나 기반 타입으로 교체가 가능하며, 이는 하위 타입이 기반 타입의 약속된 규악(public interface, 예외) 등을 지켜야 함을 나타낸다.(ex. 컬렉션 프레임워크) 다형성을 통한 확장이 해당 원칙을 기반으로 한다.

<br>

4.  I : InterFace Segregation Principle

인터페이스 분리 원칙으로 특정 클라이언트를 위한 인터페이스 여러 개가 범용 인터페이스 하나보다 낫다는 것을 의미. 즉, 하나의 일반적인 클래스보다 구체적인 여러 클래스가 나으며, 여러 클라이언트가 특정 클래스의 일부분만 사용한다면, 이를 따로 빼내어 별도의 클래스로 생성하는 것이 좋다.

<br>

5.  D : Dependency Inversion Principle

의존관계 역전 원칙, 프로그래머는 추상화에 의존해야지, 구체화에 의존하면 안됨 (ex. 의존성 주입) 즉, 구현체가 아니라 인터페이스에 의존해야함.