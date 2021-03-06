## 추상 클래스(abstract class)와 추상 메서드(abstract method)

### 추상 클래스

**추상 클래스**란, 추상 메서드를 포함한 클래스이다. 클래스가 일종의 설계도라면, 추상 클래스는 **미완성 설계도**라고 할 수 있음.

미완성 설계도로는 완제품을 만들 수 없듯이, <u>추상 클래스로는 객체를 생성하는 것이 불가능하다.</u>

> 추상 클래스는 상속을 통해 자식클래스로만 완성이 가능하다.

<br>

```java
abstract class className {}
```

추상클래스는 `abstract` 키워드를 붙여줌으로써 해당 클래스가 추상 클래스임을 명시하고 자식 클래스 내에서 추상 클래스 내 추상메서드를 오버라이딩을 통해 완성해야함을 알려준다.

<br>

#### 추상 클래스의 용도 및 장점

추상 클래스는 그 자체로는 클래스의 역할을 다하지 못하나, <u>새로운 클래스의 바탕이 되는 조상 클래스</u>로써 그 의미를 가짐.

앞서 공부한 객체지향의 특징인 [추상화](https://github.com/shm-m/TIL/blob/main/Java-basic/10-OOP.md)를 자바에서 구현한 것이 바로 추상 클래스.

> 추상화는 클래스간의 공통점을 찾아내서 공통의 조상을 만드는 작업이다.

추상화는 구체화와 반대되는 개념으로, 상속계층도를 따라 내려가면 구체화가 심해지고, 올라가면 추상화가 심해진다.(상위의 조상 클래스일수록 추상화 정도가 심해진다.)

미완성 설계도라 할지라도 특정 제품간의 공통된 부품이나 기능이 있다면 이를 미완성 설계도를 통해 명시해놓고 이를 바탕으로 각각의 설계도를 완성하는 것이 효율적인 것과 같은 맥락.

<br>

**추상 클래스는 추상 메서드를 포함하고 있다는 사실을 제외하고는 일반 클래스와 동일하므로 생성자, 필드, 일반 메서드를 작성할 수 있다.(이는 인터페이스와 구분되는 특징이기도 함)**

따라서 추상 클래스의 자식 클래스는 추상 클래스 내의 필드를 사용 가능하다.

<br>
<br>

---

### 추상 메서드

**추상 메서드**란, 일반적인 메서드와 달리 선언부만 작성하고 구현부는 비워두는 메서드이다. 구현부를 작성하지 않는 이유는 각각의 자식 클래스에서 용도에 맞게 구헌하기 쉽도록 하기 위함.

```java
abstract 리턴타입 methodName();
```

추상 메서드 역시 `abstract` 키워드를 통해 추상메서드임을 명시.

추상 메서드를 사용하는 이유는 **자식 클래스에서는 반드시 오버라이딩을 통해 해당 메서드의 작성을 강제**하기 위함이다. 만일 자식 클래스에서 추상 메서드를 오버라이딩하지 않는다면 <u>자식 클래스 역시 추상 클래스로 지정해줘야 함.</u>

<br>

#### 추상 메서드의 용도와 장점

구현부가 없기에 불필요한 기능처럼 보이지만, <u>필수적인 기능을 강제한다는 장점을 지닌다.</u>

또한, 선언부를 미리 작성한 것만으로도 추상메서드를 활용한 코드 작성이 가능하기에 생산성이 향상되고 배포가 쉬워짐.

<br>
<br>

---

### 추상 클래스의 구현

유명한 게임 젤다의 전설을 예시로 들어보자. 게임 내에는 여러 종류의 보코블린, 모리블린, 리잘포스와 같은 여러 종류의 몬스터가 존재한다.

이들의 형태와 공격 패턴은 다르지만 모두 유저를 공격하는 몬스터라는 **공통점**을 가진다. 몬스터는 실제 게임 내에서 존재하는 대상이 아니지만, 개별 몬스터들의 추상적인 원형이 된다. 따라서 이들을 클래스로 구현하기 이전에 `Monster`라는 공통의 조상 클래스를 만들면 각각의 몬스터 클래스를 작성하는데 해당 클래스를 재활용할 수 있을 것이다.

```java
abstract class Monster {
    int hp;
    int power;
    Monster(int hp, int power) {
        this.hp= hp;
        this.power = power;
    }
    void sleep () {}
    abstract void attack (User u) {}
}

class Bokoblin extends Monster {
    Bokoblin (int hp, int power) {
        super(hp, power);
    }

    void attack (User u) {}

    void riding () {}
}

class Lizalfos extends Monster {
    Lizalfos (int hp, int power) {
        super(hp, power);
    }

    void attack (User u) {}

    void spit () {}

}
```

모든 몬스터는 유저를 공격해야하므로 조상 클래스인 Monster에 attack 메서드를 추상 클래스로 구현해 자식 클래스에서 구현을 **강제**하도록 한다. 따라서 구현은 어차피 자식 클래스에서 하므로 조상 클래스에서는 구현부를 작성하지 않아도 상관 x.

한편, sleep 메서드의 경우 모든 몬스터의 공통 행위이긴하나 몬스터마다 다르게 구현되지 않으므로 추상클래스로 구현하지 않는다.

> 결과적으로 추상메서드는 모든 자식 클래스에서 반드시 구현하되, 클래스의 특징에 따라 다르게 구현할 필요가 있을 때 사용된다.

<br>

#### 다형성과 배열

[다형성](https://github.com/shm-m/TIL/blob/main/Java-basic/18-polymorphism.md)에서 확인했듯이 조상클래스는 서로 다른 클래스라 하더라도 자식 클래스라면 하나의 배열에 묶을 수 있다. 이것은 추상 클래스도 마찬가지.

```java
Monster monsters = new Monster[2];
monsters[0] = new Bokoblin(100, 150);
monsters[1] = new Lizalfos(200. 200);

for (int i = 0; i < monsters.length; i++) {
    monsters[i].sleep();
}
```

따라서 위의 코드처럼 공통 조상인 Monster 클래스 타입의 참조변수 배열을 통해서 서로 다른 종류의 인스턴스를 하나의 묶음으로 다룰 수 있다.

<br>
<br>
