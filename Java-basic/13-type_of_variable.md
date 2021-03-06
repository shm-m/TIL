## 변수의 종류 (type of variable)

> 해당 내용은 변수의 데이터타입과 다른 내용, 클래스 및 메서드 내에 존재하는 변수들을 구분하기 위한 내용.

### 선언 위치에 따른 변수의 종류

변수는 **선언 위치**에 따라 그 종류가 달라짐.

보통 클래스 내에 선언된 변수는 **멤버변수**, 메소드 내에 선언된 변수는 **지역변수**라고 함. 그리고 멤버변수는 `static`이 붙은 **클래스변수**와 붙지 않은 **인스턴스변수**로 나뉨. 따라서 자바 내 변수의 종류는 크게 클래스변수. 인스턴스변수, 지역변수로 나뉜다.

<br>

1. 멤버변수(필드) : 클래스변수, 인스턴스변수
2. 지역변수

<br>

보통 지역변수와 구분되는 개념으로 전역변수(Global Variable)이라는 개념을 사용하나 자바에는 전역변수의 개념이 없으므로 멤버변수를 사용하는 것이 옳다. [전역변수에 대한 설명](https://shm-m.github.io/blog/global_variable)

```java
class Sample {
    // 선언 위치 : 클래스 내부
    static int a; // 클래스변수
    int b; // 인스턴스변수

    void method() {
        // 선언 위치 : 메소드 내부
        int a = 10; // 지역 변수
    }

}
```

<br>
<br>

---

### 클래스변수 (Class Variable)

`static`이 붙었기 때문에 `static variable`이라고도 함. 모든 인스턴스가 공통된 저장 공간을 가지기 때문에 클래스의 모든 인스턴스가 공통적인 값을 유지해야할 때 사용(`shared variable`).

따라서 **별도의 인스턴스를 생성하지 않아도 바로 사용 가능함.** 추가로 클래스변수 앞에 `public`을 붙이면 어디서든 사용 가능한 전역변수의 성격을 가지게 됨.

<br>
<br>

---

### 인스턴스변수 (Instance Variable)

클래스변수와 마찬가지로 클래스 내부에서 생성되며, 클래스의 인스턴스를 생성할 때 만들어짐. 인스턴스 변수는 <u>독립적인 저장공간을 가지기 때문에</u> 인스턴스마다 다른 값을 가질 수 있음. 즉 변수를 변경해도 다른 인스턴스에는 영향을 끼치지 않는다.

인스턴스 변수를 사용하기 위해서는 이전에 해당 클래스의 인스턴스(객체)를 선언해야함.

<br>
<br>

---

### 지역변수 (local Variable)

<u>메서드 내에서 선언되어</u> 메서드가 종료되면 해당 변수도 소멸됨. 기존에 사용한 블럭 내부에 선언된 함수는 모두 지역변수.

메서드 내부에서만 쓰이기 때문에 밖의 변수들의 이름과는 상관이 없어 겹쳐도 무방함.

<br>

정리하면 각각의 변수는 다음과 같은 특징을 가짐

| 선언 위치 | 변수의 종류  | 생성 시기                      | 소멸 시기            | 저장 메모리          | 사용 방법           |
| --------- | ------------ | ------------------------------ | -------------------- | -------------------- | ------------------- |
| 클래스    | 클래스변수   | 클래스가 메모리에 올라갈 때    | 프로그램이 종료될 때 | 메소드 (Method Area) | `클래스이름.변수`   |
| 클래스    | 인스턴스변수 | 인스턴스가 생성되었을 때       | 인스턴스가 소멸될 떄 | 힙 (Heap)            | `인스턴스이름.변수` |
| 메서드    | 지역변수     | 블록 내에서 선언문이 수행될 때 | 블록을 벗어날 때     | 스택 (Call stack)    | `변수이름`          |

<br>
<br>

#### 클래스변수와 인스턴스 변수의 차이 사용 예제

```java
class Box {
   static int width = 10; // 클래스변수 선언
   static int height = 20;
   String content; // 인스턴스변수 선언
}

public class BoxMaking {
   public static void main(String args[]) {
      Box.width = 20; // 클래스변수는 객체를 생성하지 않아도 사용 가능

      Box a = new Box();
      Box b = new Box();

      a.content = "flower"; // content는 인스턴스변수이므로 인스턴스를 통해 값 변경 가능

      a.height = 30; // 인스턴스를 통해 값을 변경했지만 `heght`는 클래스변수이므로 모든 인스턴스에 동일하게 적용됨

      b.content = "toy";

      System.out.println("[box a's info] " + " width : " + a.width + " height : " + a.height  + " content : " + a.content);

      System.out.println("[box b's info] " + " width : " + b.width + " height : " + b.height  + " content : " + b.content);

   }
}
```

##### 출력값

```
[box a's info] width : 20 height : 30 content : flower
[box b's info] width : 20h eight : 30 content : toy
```

해당 코드를 보면 `width`나 `height`와 같은 클래스변수의 경우 값을 인스턴스를 통해 변경해도 모든 인스턴스에 동일하게 적용이 된다. 그러나 인스턴스변수인 `content`의 경우 개별 인스턴스마다 고유의 값을 가짐을 알 수 있다.

<br>
<br>
