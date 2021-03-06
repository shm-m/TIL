## 형변환(Type Casting)

형변환이란 특정 데이터타입의 값을 다른 데이터타입으로 바꿔주는 것을 의미.
형변환에는 **암시적 형변환**과 **명시적 형변환** 2가지가 존재한다.

### 1. 암시적 형변환(implicit type casting)

특별한 명령어를 적어주지 않더라도 자바가 내부적으로 특정 값의 데이터타입을 바꿔주는 것.
암시적 형변환은 더 작은 데이터타입의 값을 더 큰 데이터타입의 공간에 담거나
정수를 실수로 변환할 때 발생된다.

> 암시적 형변환 : 작은 데이터 -> 큰 데이터 or 정수 -> 실수

```java
ex)
byte myByte = 5;
int myInt = myByte;
```

int는 32비트, byte8비트이므로 myByte의 값은 얼마가 들어가든간에 int로 다 표현 가능. 따라서 위의 코드처럼 적어주면 암시적 형변환이 일어나서 myByte의 값이 int의 형태로 바뀌게 된다.

<br>

### 2. 명시적 형변환(explicit type casting)

명시적 형변환이란 우리가 특별히 명령어를 적어야 한다.
명시적 형변환을 발생시킬 때는

```
공간 = (바꿀 데이터타입값);
```

으로 발생시킬 수 있다.
암시적 형변환이 발생하지 않는 모든 경우에는 반드시 명시적 형변환을 해줘야 한다. (ex) 4.1을 정수로..)

**더 큰 데이터타입의 값을 더 작은 공간에 담을 때는(데이터 손실) 매번 명시적 형변환을 해야한다.** (마치 큰컵에 있는 물을 작은 컵에 옮겨 담는 것처럼)

```java
ex)
// myInt의 현재값을 myByte에 옮겨 담을 때 이럴 때 myInt 앞에 byte를 명시적 형변환을 해줘야 함
myByte = (byte) myInt;
```

---

#### overflow와 underflow

- **overflow** : 해당 데이터타입의 최대값보다 큰 값이 들어와서 값이 음의 영역으로 향하는 것. 자바는 기본적으로 오버플로우가 발생하면 에러를 발생시키지만 강제로 명시적 형변환을 사용하여 오버플로우를 발생시킬 수 있다.

```java
myByte = 130; // byte의 범위 : -128 ~127 -> 따라서 에러 발생
myByte = (byte) 130;
// myByte 출력값 : -126
```

오버플로우로 인해 127+1은 128이 아닌 -128이 됨. 130 = 127 + 1 + 2 = (127+1) + 2 = -128 + 2 = -126

<br>
        
- **underflow** : 해당 데이터타입의 최소값보다 작은 값이 들어와서 값이 양의 영역으로 향하는 것

```java
myByte = -140; //에러 발생
        위의 코드는 에러가 난다.
myByte = (byte) -140;
// myByte 출력값 : 127
```

(-128 -1)을 하면 언더플로우가 발생하여 -129가 아닌 127이다. 따라서 -140 = -128 -1 - 11 = 127 - 11 = 116

<br>
