## 자료형(Data Types)

데이터타입이란 해당 변수 혹은 상수가 어떠한 값을 가질 수 있는 지를 규정하는 것을 의미함.
데이터타입에는 **기본형 데이터타입**과 **참조형 데이터타입** 두가지가 존재.

- **기본형 데이터타입** : 값이 이진법으로 변환되어서 해당 공간에 직접 저장됨.
- byte, short, int, long, float, double, char, boolean 총 8가지 데이터타입이 존재함.

- **참조형 데이터타입** : 실제 값은 다른 공간에 저장이 되고 그 공간에 주소값이 해당 공간에 저장이 됨.
- 클래스형, 배열형, 인터페이스형까지 총 3가지 종류의 데이터타입이 존재함.

<br>

### 기본형 데이터 타입

1. 정수형 : 정수란 소수점이 존재하지 않는, 명확하게 떨어지는 숫자. **byte, short, int, long**

2. 실수형 : 실수란? 소수점이 존재하는 숫자. **float, double**

3. 문자형: 문자란? ASCII 테이블이라는 특수한 문자표값을 따르는 글자들을 말한다. **char**

4. 논리형 : 논리형은 true/false 라는 2가지 값만 가지는 특수한 데이터타입이다. **boolean**

<br>
<br>

---

### 1. 정수형

프로그래밍에서는 기본적으로 4가지 종류의 정수형 데이터타입을 지원

1. **byte** : 8bit (총 2^8개의 숫자를 저장 가능) -> -2^7 ~ 2^7 - 1
2. **short** : 16bit (총 2^16개의 숫자를 저장 가능) -> -2^15 ~ 2^15 - 1
3. **int** : 32bit
4. **long** : 64bit (총 2^64개의 숫자를 저장 가능) -> -2^63 ~ 2^63 - 1

> 비트란? 2진법 자릿수를 뜻한다.비트는 컴퓨터의 저장체계에서 최소단위에 속한다.
> 8bit = 1byte

프로그래밍에서 만약 우리가 표현 가능한 범위를 벗어나게 되면 버그 혹은 에러가 발생하게 된다.
해당 버그를 overflow 혹은 underflow 라고 표현.

자바에서는 기본적으로 코드에 적혀있는 10진법 숫자를 int로 인식하기 때문에 굳이 byte나 short를 사용할 필요가 없고
_+-20억 사이의 숫자이면 int를, 그 이상은 long을 사용하면 된다._

<br>
<br>

---

### 2. 실수형

실수란, 소수점이 존재하는 숫자. 프로그래밍에서는 실수형 데이터타입에 float와 double 2가지 데이터타입이 존재한다.

1. **float** = 32bit
2. **double** = 64bit

**자바에서는 코드 안의 실수를 모두 double로 인식.**

혹은 자바에게 우리가 입력한 실수가 double타입인 것을 명확하게 말을 해주는 방법도 있다. 이때는 실수값의 맨 뒤에 d를 붙이면 됨

```java
myDouble = 1.234d;
```

float 변수를 만들때는 자반에서는 코드 상의 실수를 double로 인식하므로 곧장 실수값을 넣을 수는 없고, 명시적 형변환을 하거나 해당 실수가 float 값인 것을 나타내줘야함.

1. 명시적 형변환을 사용하는 방법

```java
float myFloat;
myFloat = (float) 3.45; // 이 때 (float)를 지우면 에러
```

2. 실수값이 float 타입의 값임을 명시 -> 이때는 실수의 뒤에 f를 붙여주면 된다

```java
myFloat = 78.1234f;
```

<br>
<br>

---

### 3. char

char은 character의 줄임말. 요즘에는 캐릭터라고 하지만 차 변수라고 불리기도 한다.
char 데이터타입은 **1개의 문자를 2진법으로 변환하여 저장**할 수 있다. 2진법으로 변환할 때는 ASCII 테이블이라는 특수한 문자표를 사용하여 해당 글자의 값을 저장.

따라서 우리가 char 값을 저자할 때는 2가지 방법이 가능.
글자를 직접 지정해서 넣는 방법과 해당 글자의 값을 저장하는 방법 2가지이다.
글자를 직접 지정해서 넣을 때는 '' 안에 글자를 넣어서 저장하면 된다.

> 단, ""을 넣어서도 안되고 1개를 초과한 글자를 넣어서도 안된다.
> 왜나하면, ""는 char의 값이 아니라 String이라는 데이터타입의 값을 나타내기 때문이다.
> 또한 char는 해당 공간의 무조건 1개의 문자 값만 저장가능하기 때문에 '' 안에 2개 이상의 글자를 넣어서도 안된다.

결론적으로 char은 문자를 표시 가능한 데이터타입이지만 한번에 한개의 글자만 표시할 수 있다는 단점이 존재하여 잘 안씀

#### char 변수를 사용하는 2가지 방법

1. 문자를 직접 넣기

```java
myChar = 'A';
//출력 시 결과 : A
```

실제 공간에는 A라는 문자가 저장이 되는 것이 아니라 A가 가지는 decimal을 2진법으로 전환하여 저장. (ASCII table 참조)

2. 해당 문자의 10진법 값을 직접 넣어보기

```java
myChar = 97;
//출력 시 결과 : a
```

3. 복합적으로 사용해보기

```java
myChar = 'B' + 32;
//출력 시 결과 : b
```

---

### 4. 논리형

논리형 데이터타입인 boolean 데이터타입은 **true/false** 딱 2가지 값만 저장 가능하다.
단 boolean 타입도 해당 타입의 변수를 그대로 쓰기보다는 연산자 혹은 메소드의 결과값을 그대로 쓰는 경우가 대부분이다.

```java
boolean myBoolean = true;
```

<br>

---

### 참조형 변수

참조형의 경우 기본형을 제외한 모든 데이터타입을 의미한다. 앞서 언급한 클래스형, 인터페이스형, 문자열, 배열, 열거 등이 있다. `String` 또한 클래스형 참조 변수이다.

<br>

#### 기본형 vs 참조형

1. 참조형의 경우 기본형과 다르게 해당 값이 정적 메모리인 **스택(Stack)**에 저장되는 것이 아닌 동적 메모리인 **힙(Heap)**에 저장되며, 스택에는 주소값만 저장됨.

2. 기본형과 달리 크기가 정해져있지 않다.

3. **제네릭(generic)**에는 참조형만 가능

4. `null` 역시 참조형에만 가능

5. 단, 참조형의 경우 프로그램 실행 시 메모리에 동적으로 할당되기 떄문에 기본형보다 접근 속도가 느림(즉 성능이 별로)

<br>
