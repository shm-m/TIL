## 클래스(Class)

### 클래스와 객체

클래스란? 한개의 프로그램이자, 공통의 목적 혹은 기능을 가진 변수와 메서드를 묶어놓은 것.

객체지향 프로그래밍에서는 그러한 클래스들을 여러개 만들어서 커다란 프로그램에서 그 작은 클래스들을 변수로 다루는 것이 목적임.

보통 클래스는 **객체를 만들기 위한 틀**, 객체를 정의해놓은 것이라고 함.

- 객체 : 실제로 존재하는 것, 사물 혹은 개념 / 클래스에 정의된 내용대로 메모리에 생성되는 것

<br>

**클래스 : 객체 = 설계도 : 제품**

따라서 클래스를 한번 잘 만들어 놓으면 매번 객체를 생성할 때마다 어떻게 객체를 만들어야하는지 고민할 필요가 없고 비용과 시간이 절약됨.

<br>
<br>

---

### 객체와 인스턴스

클래스를 통해 만들어진 객체를 **인스턴스(instance)**, 이러한 과정을 인스턴스화라고 함
인스턴스는 해당 객체가 어느 클래스를 통해 만들어졌는지 강조하기 위해 사용됨.

#### 인스턴스 생성

ex) 학생1은 객체이다. 학생1은 Student 클래스의 인스턴스이다.

클래스로부터 인스턴스를 생성하는 방법은 다음과 같다.

```java
클래스명 참조변수명;

참조변수명 = new 클래스명();
```

클래스의 객체를 참조하기 위한 **참조변수**를 선언, 객체 생성 후 **객체의 주소**를 참조변수에 저장.

<br>

- **참조변수** : 해당 인스턴스의 이름이자 해당 객체의 주소값이 저장될 공간.

<br>

인스턴스는 참조변수를 통해서만 생성 가능하며, 참조변수의 타입은 인스턴스의 타입과 일치해야함.

이 것이 클래스를 **사용자 정의 타입(user-defiend-typye)**이라고 불리는 이유이다. 사용자가 서로 관련된 변수들을 묶어서 하나의 타입으로 새로 추가하기 때문.

<br>
<br>

---

### 객체의 구성 요소 (필드와 메서드)

객체는 속성과 기능으로 이루어짐. 이를 객체의 **멤버**라고 함.

- 속성 : 멤버 변수, 객체 변수, 인스턴스 변수, 특성, 필드, 상태라고도 하며 보통 **멤버 변수(variable) 혹은 필드(field)**가 가장 많이 사용

- 기능 : 메서드. 함수, 행위라고 불리며 보통 **메서드(method)**를 가장 많이 사용.

필드와 메서드의 접근 및 호출 방법은 다음과 같다

```java
객체명.필드/메소드

```

<br>

- 변수 : 하나의 데이터를 저장하는 공간
- 배열 : 같은 종류의 여러 데이터를 저장하는 공간
- 구조체 : 여러 타입의 데이터들을 한 집합에 저장할 수 있는 공간 (2세대 언어까지 사용, 자바x)
- 클래스 : 여러 타입의 데이터와 함수의 집합 (구조체 + 함수)

관계가 깊은, 즉 용도가 유사한 데이터(변수)와 함수(메소드)를 하나의 공간(클래스에)에 모으는 것.

따라서 클래스는 프로그래밍 관점에서 서로 관련된 변수들을 정의하고 이들에 대한 작업을 수행하는 함수들을 함께 정의한 것이다.

<br>
<br>

---

### 클래스 사용

#### ex) 학생 클래스

학생의 정보를 저장하는 필드와 학생 클래스 변수가 공통적으로 가질 메소드들을 만들어준다.

- 필드 : 번호, 이름, 각 과목의 성적
- 메소드 : 학생의 총점을 계산해주는 메소드 , 평균을 계산해주는 메소드, 정보를 출력해주는 메소드 등등...

만약 두 명의 학생을 입력받아야 한다면? 학생 클래스 변수를 두개 만들면 된다. 그리고 이러한 클래스 변수를 객체라고 부름.

<br>

#### Student 클래스 생성

```java
public class Student {

// 필드
// 해당 필드들의 값은 클래스 내에서 초기화해주는 것이 아니라 객체를 만들어서 사용할 때 초기화.
public String name;
public int id;
public int korean;
public int english;
public int math;

    // 메소드
    // 해당 객체들이 공통적으로 가지고 있는 기능.
    // 특정 객체만 실행시키는 것이 아니다.

    public int calculateSum() {
        return korean + english + math;
    }

    public double calculateAverage() {
        return calculateSum() / 3.0;
    }
```

<br>

#### Student 클래스 타입의 객체 생성

```java
Student s; // Student 클래스 타입의 참조변수 s 선언, 메모리에 s를 위한 공간이 마련됨

s = new Student(); // 연산자 new에의해 클래스의 인스턴스가 메모리의 빈 공간에 생성(이떄 주소값도 할당됨)
```

이 때 각각의 필드값은 타압에 맞게 기본값으로 초기화됨.

이후 대입연산자에 의해서 생성된 객체의 주소값이 참조변수 s에 저장되고 사용자는 참조변수 s만을 통해 Student 인스턴스에 접근할 수 있음.

`s.math`에 값을 대입하면 참조변수 s에 저장된 주소에 있는 인스턴스의 필드에 해당 값을 저장함.

만약 s2를 생성하고 `s = s2`라고 작성 시 s역시 s2가 참조하고 있는 인스턴스를 같이 참조하게 된다. (이 때 기존에 참조했던 해당 인스턴스를 참조하는 변수가 없으면 가비지 컬렉터가 메모리에서 해당 인스턴스 삭제)
