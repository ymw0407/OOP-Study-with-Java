# 13장

## 제너릭이란
제너릭 프로그래밍이란 다양한 종류의 데이터를 처리할 수 있는 클래스와 메소드를 작성하는 기법이다.<br>
`데이터를 포괄적으로 사용할 수 있도록 하는 프로그래밍, 어떤 데이터 타입도 가질 수 있도록 일반화시크는 프로그래밍`

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
위와 같이 Generic 대신 Object를 사용하게 된다면, 형변환이 필요하다. 그 이유는 슈퍼 클래스인 Object를 서블클래스인 Integer가 참조하는 형태이기 때문이다.<br><br>

하지만, Generic을 사용하면 캐스팅하는 코드가 불피요하다는 것을 알 수 있다.<br>
추가적으로, Generic을 사용하면 컴파일 시점에 잡을 수 없었던 타입 에러를 검출할 수 있다.

제너릭을 이용한 방법
