## 배열 (Array)

배열이란 동일한 자료형의 데이터를 연속된 공간에 저장하는 **자료구조**.

단독으로 배열이라는 특별한 데이터타입이 있는 것이 아니라 다른 데이터타입을 **지정한 크기**만큼 모아서 하나의 변수로 다루는 것임.

배열은 다음과 같은 방법으로 선언 및 생성된다.

```java
데이터타입[] 배열이름 = new 데이터타입[크기]
```

- **데이터타입[]** : 데이터타입만 적어주면 그것은 배열이 아니라 해당 데이터타입의 변수가 된다. 하지만 []를 붙여주면 해당 변수가 **그 데이터타입의 값이 여러개 모여있는 배열**이라는 데이터타입의 변수가 됨.

- **배열이름** : 낙타등 표기법을 사용하고 소문자로 시작하는 명사

- **데이터타입[크기]** : 해당 배영에 총 몇개의 값이 들어갈 수 있는지를 지정한다. 배열에서는 크기를 지정하지 않으면 오류 발생. 즉 **배열의 크기는 고정되어 있음**. 크기는 다른 변수를 사용하여 지정할 수 있다.

<br>
<br>

---

### 요소와 인덱스

배열을 구성하는 각각의 값을 **요소(element)**라고 하며, 각각의 요소가 배열에서 가지는 순서의 번호를 **인덱스(index)**라고 함.

인덱스를 통해서 배열의 엘리먼트들을 호출할 때에는 `배열이름[인덱스]`로 호출하게 된다.

```java
int[] arr = {1,2,3};
System.out.println(arr[0]) // = 1
```

인덱스는 언제나 0부터 시작하며, 0 이상의 양의 정수만을 가질 수 있음. 제일 마지막 인덱스는 배열의 길이의 -1이 된다.

만약 유효하지 않은 인덱스를 접근하면 `ArrayIndexOutOfBoundException`이 발생.

<br>

#### 배열의 길이

특정 배열의 길이를 알고 싶을 때는 `배열이름.length`를 통해 알 수 있다. 단 배열의 길이를 바꾸지 못하므로 `배열이름.length`는 상수이다.

요소의 개수 = 배열의 크기 = 배열의 길이.

즉 요소의 개수가 4개인 배열의 길이는 4이며, 인덱스의 범위는 0~3이다.

```java
int[] arr = new int[4]; //길이가 4인 배열 생성
System.out.println(arr.length); // 4 출력
```

> 배열의 길이는 0 이상의 양의 정수가 가능하다. 0도 포함된다는 사실을 잊지 말 것

<br>
<br>

---

### 배열의 초기화

배열의 특정 인덱스의 값을 할당하고 싶을 때는 `배열이름[인덱스] = 값;` 으로 할당 가능하다.

```java
int[] arr = new int[3];
arr[0] = 1; // arr이라는 배열의 첫번째 인덱스에 1을 할당
```

<br>

물론 모든 미리 데이터의 값을 알고 있는 경우에는 초기화 블록(`{}`)을 통해 모든 인덱스의 값을 한번에 할당이 가능하기도 함.

```java
int[] arr = {1,2,3};

//or
int[] arr = new int[]{1,2,3};
```

그러나 선언과 초기화를 별도로 할 경우에는 첫번째 방법은 사용 x

```java
int[] arr;

// arr = {1,2,3}; -> error

arr = new int[]{1,2,3}; // -> 정상적으로 수행
```

<br>

배열은 기본형 데이터타입의 배열이냐 아니면 참조형 데이터타입의 배열이냐에 따라서 _자동으로_ 초기화되는 값이 달라진다.

기본형 데이터타입의 배열일 경우, 해당 배열 안에 들어가 있는 값(=요소, 엘리먼트)들을 0으로 초기화한다. (단 char의 경우 `'\u0000'`, boolean은 `false`)

참조형 데이터타입의 배열일 경우, 해당 배열 안에 들어가 있는 엘리먼트들을 `null`로 초기화한다.

<br>
<br>

---

### 배열의 출력

배열은 **참조변수**이므로 `println()` 메소드에 바로 배열을 집어넣을 경우 `타입@주소`가 출력된다.

(타입은 말 그대로 배열의 타입 @ 이후는 해당 배열의 주소)

<br>

따라서 배열을 출력하려면,

1. for문을 통해 각각의 요소 호출
2. Arrays.toString 활용

<br>

```java
int[] arr = {1,2,3,4,5};

// 1번
for(int element : arr) {
    System.out.println(element);
}

// 2번
System.out.println(Arrays.toString(arr));
```

<br>
<br>

---

### 다차원 배열

배열이란? 똑같은 데이터타입이 여러개 모여있는 <u>데이터타입</u> = 결국 배열도 하나의 데이터타입임.

따라서 배열이라는 데이터타입을 모은 또 다른 배열을 만들어 줄 수 있으며 이러한 배열 of 배열 of 배열 ... 의 형태를 **다차원 배열**이라고 함.

<br>

#### 2차원 배열의 선언과 생성

```java
데이터타입[][] 배열이름 = new 데이터타입[배열이 몇개 모여있는지를 나타내는 크기][1차원 배열의 크기]

int[][] arr = newint[4][2]
```

만약 2차원 배열 `arr`을 위와 같이 선언 및 생성할 경우 이는 크기가 2인 int 1차원 배열의 "4개" 모여있는 2차원 배열을 의미한다.

다차원 배열의 경우 테이블과 같이 이해하면 되므로 각각의 인덱스를 행과 열로 취급하면 된다. 따라서 `arr`은 4개의 행에 각각 2열이 존재하는 것으로 이해하면 되며, `arr[2][1]`의 경우 3행의 2열로 인식하면 된다.

<br>
<br>

---

### 가변 배열

가변 배열이란 2차원 이상의 중첩 배열에서 찾아볼 수 있는 특수한 배열인데 배열이 몇 개 모여 있는지는반드시 적어주어야 하지만, 그 안에 있는 배열들의 크기는 모두 다른 크기로 만들어 줄 수 있다. 즉 다차원 배열에서 행의 갯수는 반드시 지정해줘야 하지만 열의 갯수는 지정해줄 필요가 없다는 의미임.

하지만 가변 길이 배열일 경우 우리가 1차원 배열을 사용할 때는 반드시 초기화해줘야 한다.

> 즉, 배열의 크기는 상관이 없고, 중요한 것은 각각의 배열의 데이터타입이 같다는 것

<br>

#### 가변 배열의 선언 및 생성

```java
int[][] arr = new int[5][];
arr[5][0] = new int[2];
arr[5][1] = new int[3];
arr[5][2] = new int[2];
arr[5][3] = new int[3];
arr[5][4] = new int[4];
```

<br>
<br>

---

### 배열의 활용

배열을 다루는 여러 메소드가 기존에 정의되어 있음. 특히 `컬렉션 프레임워크`의 `Arrays` 클래스는 배열 활용 메소드로 구성이 되어있다.

<br>

#### 1. 배열의 복사

배열을 복사하는 방법은 앞서 배열을 출력했을 때처럼 for문을 활용해도 되지만, 자바에서 메소드로 제공해준다. 배열을 복사해주는 메소드는 크게 다음과 같다.

1. **arrayCopy()**

`System.arrayCopy(복사 배열, 복사를 시작할 인덱스, 붙여넣기 배열, 붙여넣기를 시작할 인덱스. 복사할 데이터의 갯수)`

```java
int[] arr1 = { 1, 2, 3, 4};
int[] arr2 = new int[5];

System.arrayCopy(arr1, 0, arr2, 0, arr1.length); // = {1, 2, 3, 4, 0}
```

`arrayCpoy()` 메소드 활용 시 두 배열을 합치기도 편리함. 또한 2차원 배열을 1차원 배열로 변환 시에도 유용하게 사용된다.

<br>

- 2차원 배열을 1차원 배열에 옮기는 경우

```java
int[][] arr1 = {{1, 2, 3}, {4, 7, 9}, {5, 8, 10}};
int[] arr2 = new int[arr1.length * arr1[0].length];

for (int i = 0; i < arr1.length; i++) {
    System.arraycopy(arr1[i], 0, arr2, i * arr1[0].length, arr1[i].length);
}

```

<br>
<br>

2. **copyOf()**

`Arrays.copyOf(복사 배열, 새로운 배열의 길이)`
`Arrays.copyOfRange(복사 배열, 복사 시작 인덱스, 복사 끝 인덱스 + 1)`

`copyOf()` 메소드의 경우 새로운 배열을 리턴함.

`copyOfRange()` 사용 시 복사 범위를 지정해줄 수 있으며, **지정된 범위의 끝은 포함되지 않는다.**

```java
int[] arr1 = { 1, 2, 3, 4};

int[] arr2 = Arrays.copyOf(arr1, 3); // = {1, 2, 3}

int[] arr3 = Arrays.copyOfRange(arr1, 2, 5); // = {3, 4, 0, 0, 0}
```

성능은 `arrayCopy()` 메소드가 더 좋으나 유연한 복사를 위해 `copyOf()`를 보편적으로 더 많이 사용.

<br>
<br>

#### 2. 배열 채우기

1. `Arrays.fill(대상 배열, 채워질 데이터)`
2. `Arrays.setAll(대상 배열, 함수형 인터페이스)`

`fill()` 메소드의 경우 모든 요소를 지정된 값으로 채울 수 있음.

```java
int arr = new int[5];
Arrays.fill(arr, 2); // arr = {2,2,2,2,2};

Arrays.setAll(arr, i -> (int) ((Math.random() * 3) + 1));
```

<br>

#### 3. 배열의 정렬과 검색

1. `Arrays.sort(대상 배열)`
2. `Arrays.binarySearch(대상 배열, 반환할 요소의 인덱스)`

`sort()` 메소드는 배열의 요소를 오름차 순으로 정렬함.

`binarySearch()`의 경우 정렬된 배열을 넣어야 올바른 결과를 얻을 수 있음

```java
int[] arr = { 2, 3 ,1 };
Arrays.sort(arr);  // arr ={ 1, 2, 3 };

int a = Arrays.binarySearch(arr, 2); // a = 3;
```

<br>

#### 4. 배열의 비교와 출력

1. `Arrays.equals(비교 배열 1, 비교 배열 2)`
2. `Arrays.toString(배열)`

다차원 배열일 경우 `toString`이 아닌 `deepToString()` 사용

```java
int[] arr1 = { 1, 2, 3 };
int[] arr2 = { 0, 0, 0};

System.out.println(Arrays.equals(arr1, arr2)); // = false

int[][] arr3 = { {1, 2}, {3, 4} };
System.out.println(Arrays.deepToString(arr3)); // = [[1, 2], [3, 4]]
```

<br>

#### 5. 배열 to 리스트

- `Arrays.toList(배열 or 요소나열)`

단 `toList()` 메소드가 반환하는 리스트는 크기를 변경할 수 없음. 크기를 변경하고 싶은경우 `ArrayList`로 변환 후 사용.

또한 int 배열에서 Integer 리스트로 변환이 불가능하기 때문에 주로 문자열타입을 변환할 때 쓰임 혹은 int 배열 대신 Integer 배열로 생성하면 사용 가능.

```java
String[] arr = { "a", "b", "c" };
ArrayList<String> list = new ArrayList(Arrays.toList(arr));
```

<br>
<br>
