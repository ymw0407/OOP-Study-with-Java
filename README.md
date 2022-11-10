# 13장

## 제너릭이란

제너릭 프로그래밍이란 다양한 종류의 데이터를 처리할 수 있는 클래스와 메소드를 작성하는 기법이다.<br>
`데이터를 포괄적으로 사용할 수 있도록 하는 프로그래밍, 어떤 데이터 타입도 가질 수 있도록 일반화시키는 프로그래밍`

```java
// Box.java
public class Box<T> {
    private T t;

    public void set(T t) {
        this.t = t;
    }

    public T get() {
        return t;
    }
}

// Main.java
public class Main {
    public static void main(String[] args) {
        Box<Integer> box = new Box<Integer>();
        box.set(10)

        Integer i = box.get();
        System.out.println(i.intValue());
    }
}
```

Main 클래스와 같이 Box\<T>에서 T 위치에, 데이터 타입을 넣어 생성하면, 해당 데이터 타입이 T라고 쓰이게 된다.
<br><br>
그렇다면, Generic한 클래스를 사용하지 않아도, Object라는 super class인 객체를 사용하면 되지 않을까?

```java
// Box.java
public class Box {
    private Object t;

    public void set(Object t) {
        this.t = t;
    }

    public Object get() {
        return t;
    }
}

//Main.java
public class Main {
    public static void main(String[] args) {
        Box box = new Box();
		box.set(10);

		// Integer i = box.get(); // 컴파일 에러
		Integer i = (Integer)box.get();
		System.out.println(i.intValue());
    }
}
```

위와 같이 Generic 대신 Object를 사용하게 된다면, 형변환이 필요하다. 그 이유는 슈퍼 클래스인 Object를 서브클래스인 Integer가 참조하는 형태이기 때문이다.<br><br>

하지만, Generic을 사용하면 캐스팅하는 코드가 불필요하다는 것을 알 수 있다.<br>
추가적으로, Generic을 사용하면 컴파일 시점에 잡을 수 없었던 타입 에러를 검출할 수 있다.<br><br>

```
제네릭은 클래스, 메소드에서 사용할 데이터 타입을 나중에 확정하는 기법이다. 나중에라는 말은 클래스나 메소드를 선언할 때가 아닌 사용할 때, 즉 인스턴스를 생성할 때나 메소드를 호출할 때 정한다는 의미이다.
```

![참고 자료](https://atoz-develop.tistory.com/entry/JAVA-%EC%A0%9C%EB%84%A4%EB%A6%ADGenerics-%ED%81%B4%EB%9E%98%EC%8A%A4%EC%99%80-%EB%A9%94%EC%86%8C%EB%93%9C)

```
중간점검!
1. 데이터를 Object 참조형 변수에 저장하는 것이 왜 위험할 수 있는가?
 --> Object의 경우에는, 잘못된 객체를 넣어도 컴파일 단계에서 걸러내지 못한다.

2. Box 객체에 Rectangle 객체를 저장하도록 제너릭을 이용하여 생성하여 보라.
 --> MiddleTest.java 참고

3. 타입 매개 변수 T를 가지는 Point 클래스를 정의하여 보라. Point 클래스는 2차원 공간에서 점을 나타낸다.
 --> 일단 패스...

4. 제너릭 메소드 sub()에서 매개 변수 d를 타입 매개 변수를 이용하여서 정의하여 보라.
 --> public <T> void sub(t d){var = d;}

```
