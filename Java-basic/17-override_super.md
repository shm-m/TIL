## 오버라이드(Override), super / super()

### 오버라이딩이란

부모 클래스로부터 상속 받은 **메소드**들을 자식 클래스는 상황에 따라서 자신에게 맞는 방법으로 재정의해줄 수 있는데,
이렇게 부모 클래스의 메소드를 재정의하는 것을 **오버라이드(Override)**라고 부른다.

```java
class Mammal {
    void sleep() {
        System.out.println("zzz");
    }
}

class Tiger extends Mammal{
    void sleep() {
        System.out.println("Zzzz");
    }
}
```

오버라이드를 할 때는 부모 클래스가 정의한 메소드 선언식을 그대로 작성해야 한다. 따라서 자식 클래스에서 오버라이딩하는 메서드는 부모 클래스의 메서드와 **같은 이름, 파라미터, 반환 타입**을 가져야 한다.

단, 메소드의 반환 타입은 <u>부모 클래스의 반환 타입으로 타입 변환할 수 있는 타입</u>이라면 변경 가능.

<br>
<br>

오버라이드에 있어 접근 제어자와 예외는 다음과 같이 제한적으로 변경이 가능하다.

1. 접근 제어자는 더 넓은 범위에서만 변경을 허용한다.
   만약 부모 클래스 메서드의 접근 제어자가 `protected`라면, 자식 클래스에서는 동일하게 `protected` 혹은 더 넓은 범위의 `public`을 사용해야 한다.

   이는 상속의 특성 상, 부모 클래스를 데이터타입으로 갖는 인스턴스가 자식 클래스의 참조 변수로 선언될 수 있음을 고려했기 때문이다. 만약 자식 클래스의 접근 제어자의 범위가 더 좁을 경우, 해당 케이스는 사용이 불가능해진다.

2. 부모 클래스의 메서드보다 많은 예외의 수 선언 불가능
   이 역시 1번과 같은 케이스를 고려하여 불가능

3. 인스턴스 메서드를 `static` 메서드로 변경 불가능 (역도 성립 x)

<br>
<br>

---

### 오버로딩과 오버라이딩의 차이

**오버로딩**은 **같은 클래스** 내에서 같은 이름의 메서드를 **다른 시그니처**로 만드는 것을 의미하며,
**오버라이딩**은 **상속 관계**에서 같은 이름의, 같은 시그니처로 구현부만 달리하여 재정의 하는 것을 의미한다.

따라서 이름은 유사하지만, 엄연히 수행하는 기능과 내용이 다름.

```java
class Mammal {
    void sleep() {
        System.out.println("zzz");
    }
}

class Tiger extends Mammal {
    void sleep() { // 오버라이딩
        System.out.println("Zzzz");
    }
    void sleep(int hour) { // 오버로딩
        System.out.println("Zzzz for" + hour + "hours.")
    }
}
```

<br>
<br>

---

### super, super()

### super

**super**란 자식 클래스에서 부모 클래스로부터 상속받은 멤버를 참조할 떄 사용하는 **참조변수**이다. 이전에 멤버변수와 지역변수를 구별하기 위해 `this`를 사용했듯이 **상속받은 멤버와 자신의 멤버를 구분**하기 위해 사용한다.

`static` 메서드에서는 this와 마찬가지로 super 역시 사용 불가능하며 인스턴스 변수에서만 사용 가능

```java
class Mammal {
    int a = 10;
}

class Tiger extends Mammal {
    int a = 20;

    void method() {
        System.out.println(x); // 20
        System.out.println(this.x); // 20
        System.out.println(super.x); // 10
    }
}
```

위의 예제에서 만약 자식 클래스 Tiger에 별도의 멤버 변수 a가 없다면 모든 값은 10으로 출력된다. 그러나 자식 클래스에 _부모 클래스의 멤버변수와 같은 이름의 멤버 변수가 존재하므로_ 이를 super를 사용하면 이를 명확하게 구분이 가능해진다.

이는 변수뿐만 아니라 메서드에서도 동일하게 적용된다.

<br>
<br>

---

### super()

**super()**는 자식 클래스 내에서 부모 클래스의 **생성자**를 호출할 떄 사용된다.

`this()`가 같은 클래스 내의 다른 생성자를 호출할 떄 사용된다면, `super()`은 상속관계에서의 생성자를 구분하기 위해 사용한다.

**또한 `this()`와 달리 `super()`은 자식 클래스의 생성자를 만들 때 빈드시 호출되어야 한다.**

이는 자식 클래스에서는 언제나 부모 클래스의 멤버에 접근 가능하기 때문에(private 제외) 조상 클래스의 멤버들이 먼저 초기화가 되어야 하기 떄문이다.

만약 자식 클래스의 생성자에 `super()`을 입력하지 않을 경우 자바 컴파일러가 자동으로 추가해줌.

> 자식 클래스의 생성자 첫줄에는 반드시 super()를 호출한다.

중첩된 상속관계의 경우 생성자의 호출을 통해 거슬러 올라가 마지막에는 모든 클래스의 최고 조상읜 Object의 생성자 `Object()`를 호출하면서 끝이 난다.

```java
class Mammal {
    int feet;

    Mammal(int feet) {
        this.feet = feet;
    }
}

class Tiger extends Mammal {
    int toe;

    Tiger(int feet, int toe) {
        super();
        this.feet = feet;
        this.toe = toe;
    }
}

```

결국 조상 클래스의 멤버 변수는 조상의 생성자에 의해 초기화됨을 인지해야함.

<br>
<br>
