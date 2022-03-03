## 제어문 - 조건문

### 제어문(Control Statement)

제어문이란 **특정 조건이 만족되는 상황**이라면 특정 코드들을 실행 혹은 반복이 되게 만들어주는 특수한 코드를 의미.

이러한 제어문에 속하는 명령문들은 중괄호({})로 둘러싸여 있으며, 이러한 중괄호 영역을 블록(block)이라고 합니다.
제어문에는 크게 조건문과 반복문이 존재.

1. **조건문** : 특정 조건이 만족하면 코드 블락을 실행. `if문`과 `switch문`이 있음.

2. **반복문** : 특정 조건을 만족하는 동안 코드 블락을 반복시킨다. `while`문과 `for`문으로 나뉨.

#### 코드 블락

한 쌍의 중괄호{}을 코드 블락이라고 함. 또한 한개의 코드 블락 안에는 여러개의 다른 코드 블락이 들어 올 수도 있음.

코드 블락이 중요한 이유는 _코드 블락과 변수의 유효범위가 상관되기 때문._

변수는 선언되고 나서부터 그 변수가 속한 코드 블락이 끝날 때까지를 해당 변수의 유효범위로 가짐. 즉, 변수의 유효범위 안에서는 똑같은 이름의 변수를 선언할 수 없고 변수의 유효범위가 끝나면 해당 변수는 더 이상 호출할 수 없게 된다.

<br>

---

### 조건문 - if

**if 조건문** 이란 특정 조건을 만족하면 그 if문에 속한 코드 블락을 실행시키는 제어문이다.

if (조건문)에 사용한 조건문은 참과 거짓을 판단하는 문장, 즉 true 혹은 false가 결과값으로 나오는 연산자 혹은 메소드를 말하며, 해당 조건문이 true일 경우 if문에 속한 코드 블락이 실행됨.

if문은 다음과 같은 형태를 가진다.

```java
if(조건식) {
조건식이 참일 떄 실행할 코드
}
```

<br>

#### if - else 조건문

if문만 단독으로 사용할 경우에는 해당 조건식이 false가 나오면 아무것도 하지 않음.

따라서 조건식 true일때와 false일때 각각 코드를 따로 실행해야한다면 if문을 2개 만들어서 조건식을 따로 해주는 것이 아니라 **if-else** 구조를 만들어서 조건식이 true일때와 false일때의 코드를 각각 넣어줘야 함.

```java
if(조건식) {
조건식이 true일때 실행할 콛
} else {
조건식이 false일때 실행할 코드
}
```

#### if - else if 조건문

조건문을 만들다 보면 true/false 두가지로만 나뉘는게 아니라 첫번째 조건식이 false일 때 그 다음 조건식, 두번째 조건식이 false일 때 그 다음 조건식 이렇게 차례대로 조건문을 나눠야 하는 경우도 존재함.

이럴때는 **if - else if** 구조가 나오게 된다. else if는 필요한 만큼 넣어줄 수 있음.

if - else if 구조는 다음과 같은 조건을 가져야함.

1. 위에서부터 차례대로 조건식을 실행시키기 때문에 만약 아래에서 처리해야할 코드가 있는데 위의 조건식에서 걸리면
   위의 조건식 기준으로 실행이 된다.
2. 때에 따라서는 **if - else if -else**의 구조가 될 때도 있다. else는 반드시 제일 마지막에 나와야 하는데
   위의 조건식이 false가 나왔을 때는 무조건 else로 향하기 때문에 else 다음에는 절대로 else if가 나올 수 없기 때문.

```java
age = 10;
if (age < 5) {
    System.out.println("영아 입니다.");
} else if (age < 19) {
    System.out.println("청소년 입니다.");
} else if (age < 14) {
    System.out.println("어린이 입니다.");
} else {
    System.out.println("성인입니다.");
}
```

<br>

#### 중첩 if 조건문 (Nested If)

모든 제어문들은 중첩해서 사용 가능하다. if문의 경우, 특정 조건을 먼저 체크하고 그 다음에 코드를 실행 시킨 후에
다시 조건을 체크해야하는 경우가 있을 때에는 중첩 if문을 사용하게 된다.

```java
System.out.println("국어점수를 입력하시오.");
int korean = scanner.nextInt();

if (korean >= 0 && korean <= 100) {
    System.out.println("영어점수를 입력하시오.");
    int english = scanner.nextInt();
    if (english >= 0 && english <= 100) {
        System.out.println("수학점수를 입력하시오.");
        int math = scanner.nextInt();
        if (math >= 0 && math <= 100) {
            System.out.printf("당신의 점수\n 국어: %d점\t 영어: %d점\t 수학: %d점\n", korean, english, math);
        } else {
            System.out.println("잘못된 입력이 감지되어 프로그램을 종료합니다.");
        }
    } else {
        System.out.println("잘못된 입력이 감지되어 프로그램을 종료합니다.");
    }
} else {
    ystem.out.println("잘못된 입력이 감지되어 프로그램을 종료합니다.");
}
```

<br>

---

### 조건문 - switch

**switch 조건문**은 우리가 ()안에 true/false가 나오는 조건식을 적어주는 것이 아니라 값이 변할 수 있는 변수 혹은 변수에 대한 산술연산자 등을 적어주고 그 값(결과값)에 대한 코드처리를 해줌.

switch문은 if문보다 정형화 되어있어 가독성이 좋고 컴파일러에 최적화되어 속도가 더 빠름.

```java
switch() {
    case 값:
    처라할 코드
    break;
}
```

만약 케이스에 해당 되지 않는 모든 경우에 대해 처리를 해줄 때에는 `default:` 라는 것을 만들어서 처리해주면 된다.

또한 break가 생략이 되면 만족하는 케이스부터 break가 나올 때까지 모든 코드들이 실행된다. break의 경우, 단순히 switch에서만 사용되는 것이 아니라 반복문에서도 사용. 이 때는 반복을 강제로 멈추라는 의미가 된다.

```java
int number = 1;
switch (number) {
case 0:
    System.out.println("0입니다.");
    break;
case 1:
    System.out.println("1입니다.");
    break;
case 2:
    System.out.println("2입니다.");
    break;
case 3:
    System.out.println("3입니다.");
    break;
default:
System.out.println("그외입니다.");
break;
}
```

<br>

- 의도적으로 break를 생략한 효율성을 증대시킨 예시

```java
Scanner scanner = new Scanner(System.in);
System.out.println("월을 입력해주세요.");
System.out.println("> ");
int month = scanner.nextInt();
switch (month) {
case 2:
    System.out.print("28일까지입니다.");
    break;
case 4:
case 6:
case 9:
case 11:
    System.out.println("30일까지입니다.");
    break;
default:
    System.out.println("31일까지입니다.");
    break;
}
scanner.close();
```
