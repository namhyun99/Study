# 컬렉션 프레임워크

    프로그램 구현에 필요한 자료구조(Data Structure)를 구현해 놓은 라이브러리   
    java.util 패키지에 구현되어 있음   
    개발에 소요되는 시간을 절약하면서 최적화 된 알고리즘을 사용할 수 있음
    여러 인터페이스와 구현 클래스 사용 방법을 이해해야 함


## 제네릭(Generic) 프로그래밍
- 변수의 선언이나 메서드의 매개변수를 하나의 참조 자료형이 아닌 여러 자료형을 변환 될 수 있도록 프로그래밍 하는 방식이다
- 실제 사용되는 참조 자료형으로의 변환은 컴파일러가 검증하므로 안정적인 프로그래밍 방식
- `컬렉션 프레임워크`에서 많이 사용되고 있음
- 자바 5.0부터 나온 기능.

## 자료형 매개 변수 `T`
- `type`의 의미로 `T`를 많이 사용한다.
- `<T>`에서 `<>`는 다이아몬드 연산자라고 한다.
- `static` 키워드는 `T`에 사용할 수 없다. 그 이유는 인스턴스 생성과 상관없이 메모리를 잡기 때문이다.
- ArrayList<String> list = new ArrayList<>();   
  다이아몬드 연산자 내부에서 자료형은 생략 가능 하다.
- 제네릭에서 자료형을 추론해 컴파일 한다. (자바 10부터가능)   
    ArrayList<String> list = new ArrayList<String>(); => var list = new ArrayList<String>();

```java
class TreeDPrinter<T extends Material> {
	//여기서 T는 type의 약자 매개 변수의 타입을 Object로 변환, 다른 알파벳을 사용해도 된다.

	private T material;  
	
	public T getMaterial() {
		return material;
	}

	public void setMaterial(T material) {
		this.material = material;
	}

	@Override
	public String toString() {
		return material.toString();
	}
	
	public void printing() {
		material.doPrinting();
	}

}

public class TreeDPrinterTest {

	public static void main(String[] args) {
		
		//아래의 타입을 클래스명으로 선언해주면 다음 코드에서 선언하지 않아도 된다.
		TreeDPrinter<Powder> printer = new TreeDPrinter<Powder>();
		printer.setMaterial(new Powder());		
		Powder powder = printer.getMaterial();
		System.out.println(printer);
		
		
		TreeDPrinter<Plastic> printer1 = new TreeDPrinter<Plastic>();
		printer1.setMaterial(new Plastic());		
		Powder Plastic = printer.getMaterial();
		System.out.println(printer1);
		
		
//		TreeDPrinter<Water> printer2 = new TreeDPrinter<Water>();
//		printer2.setMaterial(new Water());	
//		System.out.println(printer2);
		
		printer1.printing();
		

	}

}
```

## List 인터페이스
- Collection 하위 인터페이스
- 객체를 순서에 따라 저장하고 관리하는데 필요한 메서드가 선언된 인터페이스
- 배열의 기능을 구현하기 위한 인터페이스
- `ArrayList`, `Vector`, `LinkedList` 등이 많이 사용 됨

## `Array` (배열)
- 같은 형의 데이터 타입을 메모리에 저장하는 선형적 자료구조.
- 논리적 구조와 물리적 구조가 동일합니다.
- `fixed-length`, 배열의 크기를 정하고 들어간다. 만약 자리수가 초과된다면 다른 배열을 또 만든다.
- 인덱스 연산이 빠르것이 장점이다.
- ins/del 시 연산 과장이 많아지는 단점이 있다.
- `ArrayList`는 객체 배열을 구현해 놓은 것.
- `Vector`는 지금은 자주 사용하지 않음.


## `ArrayList`와 `Vector`
- 객체 배열을 구현한 클래스
- `Vector`는 자바2 부터 제공된 클래스
- 멀티 쓰레드 상태에서 리소스에 대한 동기화 가 필요한 경우 `Vector`를 사용
- 일반적으로 `ArrayList`를 더 많이 사용 함
- `ArrayList`에 동기화 기능이 추가 되어야 하는 경우
```java
    Collections.synchronizedList(new ArrayList<String>());
```
- `동기화(synchronization)`: 두 개의 쓰레드가 동시에 하나의 리소스에 접근 할 때 순서를 맞추어서 데이터에 오류가 발생하지 않도록 함.


## `LinkedList` (링크드리스트)
- 배열의 단점을 보완한 녀석
- 배열과 다르게 크기를 지정하지 않고 사용한다는 장점이 있다.
- 물리적 구조와 논리적 구조가 다르다. (ex. 물리적으로는 떨어져 있어도, 논리적으로는 바로 옆에 있다.)
- ins/del 시 연산과정 없이 링크 연결만 변경해주면 된다.

## `Stack` (스택)
- 일반적인 의미는 `쌓다.` `더미.`로 정의 할 수 있다.
- LIFO (Last In First Out) 후입선출 방식
- `push()` 넣다. / `pop()` 꺼내다.
- `Peek` : 스택의 맨위에 있는 원소를 반환, 하지만 스택에서 제거하지는 않음. get()과 비슷한 의미

```java
class MyStack{
	
	private ArrayList<String> arrayStack = new ArrayList<String>();
	
	public void push(String data) {
		arrayStack.add(data);
	}
	
	public String pop() {
		int len = arrayStack.size();		
		if(len == 0) {
			System.out.println("스택이 비었습니다");
			return null;
		}
		return arrayStack.remove(len - 1);		
	}
	
	
}

public class StackTest {

	public static void main(String[] args) {
		
		MyStack stack = new MyStack();
		
		stack.push("a");
		stack.push("b");
		stack.push("c");
		
		System.out.println(stack.pop()); // c
		System.out.println(stack.pop()); // b
		System.out.println(stack.pop()); // a
		System.out.println(stack.pop()); // 스택이 비었습니다. null
	}
}

```


## `Queue` (큐)
- FIFO (First In First Out) 선입선출 방식
- 대기열, 흔히 말하는 선착순의 개념과 비슷하다.


```java
class MyQueue {
	private ArrayList<String> arrayQueue = new ArrayList<String>();
	
	public void enQueue(String data) {
		arrayQueue.add(data);
	}
	
	public String deQueue() {
		int len = arrayQueue.size();
		if(len == 0) {
			System.out.println("큐가 비었습니다.");
			return null;
		}
		
		return arrayQueue.remove(0);
	}
}

public class QueueTest {

	public static void main(String[] args) {
		
		MyQueue queue = new MyQueue();
		
		queue.enQueue("a");
		queue.enQueue("b");
		queue.enQueue("c");
		
		System.out.println(queue.deQueue()); // a
		System.out.println(queue.deQueue()); // b
		System.out.println(queue.deQueue()); // c
		System.out.println(queue.deQueue()); // 큐가 비었습니다 null
	}
}

```


## Set 인터페이스
- Collection  하위의 인터페이스
- 중복을 혀용하지 않는다.
- 아이디, 주민번호, 사번등 유일한 값이나 객체를 관리할 때 사용한다.
- List는 순서기반의 인터페이스지만, Set은 순서가 없다.
- 저장된 순서와 출력순서는 다를 수 있다
- get(i)메서드가 제공되지 않는다. 그래서 Iterator를 사용한다.
```java
    Iterator ir = memberArrayList.iterator();
```


## `Hashing` (해싱)
- 산술 연산을 이욯한 검색 방법
- 필요한 값을 찾을때 쓴다. 단, 충돌이 날 수 있다.



## TreeSet 클래스
- 객체의 정렬에 사용되는 클래스
- 중복을 허용하지 않으면서 오름차순이나 내림차순으로 객체를 정렬 한다.
- 내부적으로 이진 검색 트리(Binary Search Tree)로 구현되어 있다.
- 이진 검색 트리에 자료가 저장 될 때 비교하여 저장될 위치를 정함
- 객체 비교를 위해 `Comparable` 이나 `Comparator` 인터페이스를 구현해야 한다.

```java

public class Member implements Comparable<Member> {

	private int memberId;
	private String memberName;

	public Member(int memberId, String memberName) {
		super();
		this.memberId = memberId;
		this.memberName = memberName;
	}

// .. get/set 메서드, toString, hashCode, equals 오버라이딩 생략...
	@Override
	public int compareTo(Member member) {
        return (this.memberId - member.memberId); // 오름차순 정렬
		return (this.memberId - member.memberId) * (-1); // 내림차순 정렬 
        // 내 아이디 값이 입력받은 아이디 값보다 크다면.. 오름차순 
        return(this.memberName.compareTo(member.memberName)); // String값 오름차순 정렬
        // 양수(+)면 오름차순 정렬 / 음수(-)면 내림차순
	}
```

```java
import java.util.Comparator;
import java.util.TreeSet;

class MyCompare implements Comparator<String> {

	@Override
	public int compare(String str1, String str2) {
		return str1.compareTo(str2) * (-1);
	}
}

public class ComparatorTest {

	public static void main(String[] args) {
		
		TreeSet<String> tree = new TreeSet<String>(new MyCompare());
		
		tree.add("aaa");
		tree.add("bbb");
		tree.add("ccc");
		
		System.out.println(tree); // [ccc,bbb,aaa]

	}

}

```

## `Tree` (트리)
### Binary Search Tree (BTS, 이진 탐색 트리)
- 자식 노드가 최대 2개인 것을 말함.
- left는 나보다 작은수 , right는 나보다 큰수 (중복허용 X)

<img src = "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbCe3QD%2Fbtq2ytHuN1Z%2FAi82KHYBlgY01j9hbwjOO1%2Fimg.png"> <br>
<font size="2px"> < 이미지 출처 : https://yoongrammer.tistory.com/71 >  </font>

- in-order 오름차순으로 정렬됨. 만약 반대로 한다면 내림차순으로 정렬된다.
- 유일한 키 값, 루트 노드의 키 값 기준


## Map 인터페이스
- key- value pair의 객체를 관리하는데 필요한 메서드가 정의 된다. (파이썬의 딕셔너리와 비슷)
- key는 중복 될 수 없다
- 검색을 위한 자료 구조이다.
- key를 이용하여 값을 저장하거나 검색, 삭제 할때 사용하면 편리함
- 내부적으로 `hash` 방식으로 구현됨
```java
    index = hash(key) // index는 저장위치
```
- key 가 되는 객체는 객체의 유일성함의 여부를 알기 위해 `equals()`와 `hashCode()` 메서드를 재정의 한다.

## HashMap 클래스 
```java
public class MemberHashMap {
	
	private HashMap<Integer, Member> hashMap;
	
	public MemberHashMap() {
		hashMap = new HashMap<Integer, Member>();
	}
	
	public void addMember(Member member) {
		hashMap.put(member.getMemberId(), member);
	}
	
	public boolean removeMember(int memberId) {
		if(hashMap.containsKey(memberId)) {
			hashMap.remove(memberId);
			return true;
		}
		
		System.out.println(memberId + "가 존재하지 않습니다.");
		return false;
	}
	
	public void showAllMember() {
		Iterator<Integer> ir = hashMap.keySet().iterator();
		
		while(ir.hasNext()) {
			int key = ir.next();
			
			Member member = hashMap.get(key);
			System.out.println(member);
		}
	}
}

```








