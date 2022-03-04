## 제어문 - 반복문

### 반복문(Iteration Statements)

반복문이란 프로그램 내에서 똑같은 명령을 일정 횟수만큼 반복하도록 하는 제어문. 반복문에는 크게 while문과 for문이 존재.

#### 반복문 - while문

**while문**은 특정 조건을 만족할 때까지 계속해서 주어진 명령문을 반복하는 명령문.

while 반복문은 조건식을 만들고 해당 조건식이 참일 동안 그 안의 코드가 계속 반복된다. 코드 반복이 끝날 때마다 조건식을 다시 체크하여 반복 여부를 결정

<br>

```java
while(조건문) {
    조건문이 true일 때까지 반복해서 실행되는 코드;
}
```

만약 반복문에서 조건식이 처음부터 false가 나오면 반복문 자체가 실행되지 않는다. 왜냐하면 조건식을 처음부터 체크하여 true가 나올 때에만 코드가 반복되기 때문.

##### 무한 루프 (Infinite Loop)

while문은 조건식이 참일 경우 계속 해서 반복되므로 무한 루프를 만들 수 있다.

```java
while(true) {
    System.out.println("무한 루프");
}

```

무한 루프는 특별히 의도한 경우가 아니라면 기피해야 하기 때문에 while문 사용 시에는 조건식의 결과가 특정 조건에서는 거짓(false)이 되도록 조건식의 결과를 변경하도록 if문과 같은 명령문을 포함해야함.

##### break

while문을 사용 시에 강제로 whlie문에서 빠져나가야 하는 경우가 생김. 그럴 떄는 **break**를 사용하면 된다.

- break 사용 예시

```java
while(true) {
    System.out.println("1. 입력 2. 출력 3. 종료");
    System.out.print("> ");
    int userChoice = scanner.nextInt();

    if(userChoice == 1) {
                입력하는 코드를 여기에 구헌
            } else if(userChoice == 2) {
                출력하는 코드를 여기에 구현
            } else if(userChoice == 3) {
                메시지 출력 후 종료
                System.out.println("종료");
                break;
            }

```

사용자가 3을 입력 시에는 break가 호출되어 무한 루프에 빠졌던 while문이 종료된다.

##### continue

while 문에서는 특정 조건에 도달할 시 while문의 처음으로 돌아가 명령문을 다시 수행할 수 있음. 이러한 기능을 수행하는 것이 바로 **continue**

- continue 사용 예시

```java
int a = 0;
while (a < 10) {
    a++;
    if (a % 2 == 0) {
        continue;
    }
    System.out.println(a);
}
```

위의 코드의 경우 a가 짝수이면 `continue`를 통해 while 내 코드를 끝까지 실행하지 않고, 다시 while 초기의 조건으로 돌아가 결과적으로 홀수만 출력하도록 함.

<br>

---

### 반복문 - for문

for 반복문 while 반복문과는 좀 다르게 비교적 명확하게 **반복 횟수가 예상 가능**함.

for 반복문은 다음과 같은 형태를 가지고 있다.

```java
for(제어 변수의 선언과 초기화식; 조건식; 변화식) {
   반복한 코드
}
```

- **제어 변수** : 해당 for 반복문을 반복시킬지 말지를 결정할 때 사용할 조건식에서 쓸 변수. 실제로 변수이기 때문에 유효범위가 for문 전체인 변수이다. 주로 정수형 변수를 쓰고, 전통적으로 int i를 쓴다.

- 조건식 : 해당 for 반복문의 반복할 코드를 실행시킬지 말지를 결정할 조건식이다. true이면 반복, false이면 반복이 종료된다.

- 변화식 : for 문에서 _반복할 코드를 모두 실행시키고 나서_ 제어 변수의 값을 변화시킬 할당연산자가 들어간 식.

```java
for (int i = 1; i <= 4; i++) {
    System.out.println(i);
}
```

#### 중첩 for문 (Nested For)

다른 중첩문들과 마찬가지로, 중첩 for문은 for문안에 또다른 for문이 들어가 있는 형태이다.

단 중첩for문의 경우 바깥쪽 for문이 한번 실행되는 동안 안쪽 for문은 전체횟수가 실행된다.
또한 중첩 레벨에 따라서 i -> j -> k ... 순으로 제어변수가 선언된다. 즉, 한개의 i for 문 안에는 여러개의 for 문이 들어갈 수 있지만 다른 i for문이 들어갈 수는 없음.

```java
int iStart = 1;
int iEnd = 100;
for (int i = iStart; i <= iEnd; i++) {
    int count = 0;
    for (int j = 1; j <= i; j++) {
        int k = i % j;
        if (k == 0) {
            count++;
        } else {}
    }
    if (count == 2) {
        System.out.println(i + "는 소수입니다.");
    }
}
```

위와 같이 중첩 for문을 통해 소수를 구하는 코드를 작성할 수도 있음.

#### for each

for each의 경우 for문을 보다 간단하게 작성할 수 있는 방법이다. for문과 동일한 기능을 수행하지만, **반복회수 지정이 불가능하고** 변화식이 1씩 증가만 가능함. 따라서 배열같은 변수에 활용하기 적절하다.

for each문의 구조는 다음과 같다.

```java
for (제어변수 명 : 대상 변수) {
    반복 코드
}
```

- 사용 예시

```java
int[] arr = {1,2,3,4,5};
for (int num : arr) {
    System.out.println)(num);
}
```
