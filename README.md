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

[참고 자료](https://atoz-develop.tistory.com/entry/JAVA-%EC%A0%9C%EB%84%A4%EB%A6%ADGenerics-%ED%81%B4%EB%9E%98%EC%8A%A4%EC%99%80-%EB%A9%94%EC%86%8C%EB%93%9C)

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

## 컬렉션이란

-   collection은 자바에서 자료구조를 구현한 클래스
-   자료 구조로는 list, stack, queue, set, hash table 등이 있다.
-   Map과 같은 경우에는, Collection 인터페이스를 상속받고 있지는 않지만 Collection으로 분류된다.
-   초기에는 Vector, Stack, HashTable, Bitset, Enumeration으로 존재하였고, 버전 1.2부터는 인터페이스와 구현(List 인터페이스를 ArrayList와 LinkedList 클래스가 구현)을 분리하며, 풍부한 컬렉션 라이브러리를 제공했다.
    <br>

Java Collections Framework(JCF) <br>
┣ Collection(순서나 집합적인 저장 공간) <br>
┃ ┣ List(순서가 있는 저장 공간) <br>
┃ ┃ ┣ LinkedList(링크드리스트) <br>
┃ ┃ ┣ Stack(스택 자료 구조) <br>
┃ ┃ ┣ Vector(동기화 보장) <br>
┃ ┃ ┗ ArrayList(동기화 보장하지 않음) <br>
┃ ┣ Set(집합적인 저장공간) - 인터페이스 클래스 <br>
┃ ┃ ┣ HashSet(Set계열의 대표 클래스) <br>
┃ ┃ ┣ SortedSet(정렬을 위한 Set 계열의 클래스) <br>
┃ ┃ ┗ TreeSet <br>
┣ Map(키와 값으로 데이터 전달) <br>
┃ ┣ Hashtable(동기화 보장하는 Map 계열의 클래스) <br>
┃ ┣ HashMap(동기화 보장하지 않는 Map 계열의 클래스) <br>
┃ ┣ SortedMap(정렬을 위한 Map 계열의 클래스) <br>
┃ ┗ TreeMap <br>

## 컬렉션의 특징

-   컬렉션은 제너릭을 사용한다.
-   컬렉션은 클래스만 받아올 수 있기 때문에 기초 자료형은 저장하지 못하고, 랩퍼 클래스는 저장할 수 있다.
-   기본 자료형을 저장하면, 랩퍼 클래스의 객체로 변환된다.(**auto boxing**)

```java
// Collection 인터페이스의 주요 메소드

boolean isEmpty() // 공백 상태이면 true 반환
boolean contains(Object obj) //obj를 포함하고 있으면 true 반환
boolean containsAll(Collection<?> c) //  특정한 원소들이 모두 있으면 true 반환

boolean add(E element) // 원소를 추가
boolean addAll(Collections<? extends E> c) // 컬렉션에 존재하는 모든 원소들을 추가

boolean remove(Object obj) // 원소를 삭제
boolean removeAll(Collection<?> c) // 컬렉션에 존재하는 모든 원소들을 삭제
boolean retainAll(Collection<?> c) // 컬렉션에 존재하는 원소들을 제외하고 모두 삭제
void clear() // 모든 원소를 삭제

// ---- 원소 방문 ----
Iterator<E> iterator()
Stream<E> stream()
Stream<E> parallelStream()
// ---- 원소 방문 ----

int size() // 원소의 갯수 반환

Object[] toArray()
<T> T[] toArray(T[] toAtrray(T[] a))
```

## 컬렉션의 모든 요소 방문하기

```java
String a[] = new String[] { "A", "B", "C", "D", "E" };
List<String> list = Arrays.asList(a);

// - 1. 전통적인 for 구문을 사용 -
for (int i=0; i<list.size(); i++>)
    System.out.println(list.get(i));

// - 2. 전통적인 for-Each 구문을 사용 -
for (String s:list)
    System.out.println(s);

// - 3. 반복자, Iterator를 사용 -
String s;
Iterator e = list.iterator();
while(e.hasNext()){ // 아직 방문하지 않은 원소가 있으면 true를 반환
    s = (String)e.next(); // 다음 원소를 반환(반복자는 Object type으로 반환! 따라서 casting 필요)
    System.out.println(s);
}

// - 4. Stream 라이브러리를 사용 -
list.forEach((n) -> System.out.println(n));
```

> 중간점검! <br>
1. 컬렉션에는 어떤 것들이 있는가? <br>
 --> 컬렉션에는 크게 List, Set, Map, Queue로 인터페이스 클래스가 있고 세부 클래스들로 구현되어 있다. <br><br>
2. 컬렉션 클래스들은 어디에 이용하면 좋은가? <br>
 --> 글쎄요... <br><br>
3. Colleciton 인터페이스의 각 메소드들의 기능을 자바 API 웹페이지를 이용하여 조사하여 보자 <br>
 --> 일단 패스... <br><br>

## ArrayList & Vector

### ArrayList

-   ArrayList를 배열(Array)의 가변 크기의 배열
-   ArrayList의 생성
    `ArrayList<String> list = new ArrayList<String>();`

### Vector

-   Vector 클래스는 java.util 패키지에 잇는 컬렉션의 일종으로 가변 크기의 배열을 구현하고 있다.
-   Vector의 생성
    `Vector<String> list = new Vector<String>();`

### Vector vs ArrayList

-   Vector는 스레드 간의 동기화를 지원하는데 반하여 ArrayList는 동기화를 하지 않기 대문에 Vector 보다 성능은 우수하다.
    (Vector와 같은 경우에는 동기화를 지원하여 스레드를 차근차근 처리하기 때문에 느리다.)

### ArrayList 기본 연산

```java
// -- add 메소드를 통해서 순서대로 추가 --
list.add( "MILK" );
list.add( "BREAD" );
list.add( "BUTTER" ); // ["MILK", "BREAD", "BUTTER"]

// -- set 메소드를 통해서 해당 Index의 원소를 해당하는 원소로 대체 --
list.set( 1, "APPLE" ); // ["MILK", "APPLE", "BUTTER"]

// -- remove 메소드를 통해서 해당 인덱스의 원소를 제거 --
list.remove( 2 ); // ["MILK", "APPLE"]
```

> 참고: 불행하게도 자바에서는 배열, ArrayList, 문자열 객체의 크기를 알아내는 방법은 약간 다르다. (배열: array.length, ArrayList: arrayList.size(), 문자열: string.length())

> 중간점검! <br>
1. ArrayList가 기존의 배열보다 좋은 점은 무엇인가? <br>
 --> 기존의 배열은 크기가 처음에 정해지는 반면에, ArrayList는 가변배열이다. [참고](https://velog.io/@humblechoi/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-Array-vs-ArrayList) <br><br>
2. ArrayList의 부모 클래스는 무엇인가? <br>
 --> ArrayList의 부모 클래스는 List이다. <br><br>
3. 왜 인터페이스 참조 변수를 이용하여서 컬렉션 객체들을 참조할까? <br>
 --> 글쎄요... <br><br>
4. ArrayList 안의 객체들을 반복 처리하는 방법들을 모두 설명하라. <br>
 --> for문, for-Each문, while문(get 메소드), Iterator(hasNext, next 메소드), 람다식(forEach 메소드), Stream API <br><br> 

## LinkedList
- 빈번하게 삽입과 삭제가 일어나는 경우에 사용하는 배열
- 기본적인 배열은 배열의 중간에 데이터를 삽입하려면 원소들을 이동해야하지만, linkedList는 연결 리스트 중간에 삽입하려면 링크만 수정하면 된다.

### LinkeList 기본 연산

```java
LinkedList<String> list = new LinkedList<String>();

list.add("BREAD");
list.add("BUTTER");
list.add("APPLE");
// ["BREAD", "BUTTER", "APPLE"]
list.addFirst("MILK");
// ["MILK", "BREAD", "BUTTER", "APPLE"]
list.add(1, "MANGO");
// ["MILK", "MANGO", "BREAD", "BUTTER", "APPLE"]
list.remove(); // 맨 앞 값을 제거
// ["MANGO", "BREAD", "BUTTER", "APPLE"]
list.remove("BREAD"); // Object로도 지울 수 있음!
// ["MANGO", "BUTTER", "APPLE"]
list.remove(2);// index로도 지울 수 있음!
// ["MANGO", "BUTTER"]
list.set(1, "APPLE"); // 인덱스를 통해 대체
// ["MANGO", "APPLE"]

String peeked1 = list.peek(); // 맨 첫 값을 가져온다
System.out.println(peeked1);
String peeked2 = list.peekFirst();
System.out.println(peeked2);

String peeked3 = list.peekLast(); // 맨 마지막 값을 가져온다
System.out.println(peeked3);
String last = list.getLast();
System.out.println(last);

System.out.println("----------------------------");

String poll = list.poll(); // 맨 첫 값을 가져오고 반환
System.out.println(poll);

// ["APPLE"]

String poll2 = list.poll();
System.out.println(poll2);

// []

for (int i=0; i<list.size(); i++){
    System.out.println(list.get(i) + " ");
}
```

### ArrayList vs LinkedList
- ArrayList는 인덱스를 가지고 원소에 접근할 경우, 항상 일정한 시간만 소요된다. ArrayList는 리스트의 각각의 워소를 위하여 노드 객체를 할당할 필요가 없다. 또 동시에 많은 원소를 이동해야 하는 경우에는 System.arraycopy()를 사용할 수 있다.
 
- 만약 리스트의 처음에 빈번하게 원소를 추가하거나 내부의 원소 삭제를 반복하는 경우에느 LinkedList를 사용하는 것이 낫다. LinkedList에서는 일정한 시간만 걸리지만, ArrayList는 원소의 갯수에 비례하는 시간이 소요된다. <br><br>

[참고](https://devlog-wjdrbs96.tistory.com/64)

<br><br>

## Set
- 집합(Set)은 순서가 없고, 원소의 중복을 허용하지 않는다.

### HashSet
- HashSet은 해쉬 테이블에 원소를 저장하기 때문에 성능면에서 가장 우수하다. 하지만, 원소들의 순서가 일정하지 않은 단점이 있다.

### TreeSet
- 레드-블랙 트리(red-black tree)에 원소를 저장한다. 값에 따라서 순서가 결정되지만, HashSet보다는 느리다.

### LinkedHashSet
- 해쉬 테이블과 연결 리스트를 결합한 것으로 원소들의 순서는 삽입되었던 순서와 같다.

### 위 3가지의 차이
- HashSet은 순서를 보장하지 않는다.
- LinkedHashSet은 입력된 순서대로 데이터를 관리한다.
- TreeSet은 오름차순으로 자동 정렬을 해준다.
<br>
[참고](https://velog.io/@ayoung0073/Java-HashSet%EA%B3%BC-LinkedHashSet-%EB%B9%84%EA%B5%90)
<br><br>

### Set 연산

```java
HashSet<String> set = new HashSet<String>();

set.add("Milk");
set.add("Bread");
set.add("Butter");
set.add("Cheese");

if(set.contains("Bread")){ // 해당 원소가 set에 존재하는지 확인
    System.out.println("Bread가 존재한다");
}
```

### 대량 연산 메소드

```java

```