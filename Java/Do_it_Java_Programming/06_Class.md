# 기본 클래스
<br>

## Java.lang 패키지
- 프로그래밍시 import하지 않아도 자동으로 import 됨
- 많이 사용하는 기본 클래스들이 속한 패키지
- String, Integer, System 등


## Object 클래스
- 모든 클래스의 최상위 클래스 (java.lang.Object클래스)
- 모든 클래스는 `Object` 클래스에서 상속 받음 (직접 입력하지는 않지만 컴파일러가 추가함)
- 모든 클래스는 `Object` 클래스의 메서드를 사용할 수 있다.
- 모든 클래스는 `Object` 클래스의 메서드 중 일부는 재정의(Overriding) 할 수 있음 (final 로 선언된 메서드는 제외)


## 1. toString() 메서드
- Object 클래스의 메서드
- 객체의 정보를 String으로 바꾸어서 사용할 때 많이 쓰임

```java
class Book {
	String title;
	String author;

	public Book(String title, String author) {
		super();
		this.title = title;
		this.author = author;
	}

	@Override
	public String toString() {
		return "Book [title=" + title + ", author=" + author + "]";
	}

}

public class ToStringEx {

	public static void main(String[] args) {
		Book book = new Book("Do It Java", "은종님");
		System.out.println(book);
	}

}
```


## 2. equals() 메서드
- 두 인스턴스의 주소값을 비교하여 `true`/`false`를 반환
- 재정의 하여 두 인스턴스가 논리적으로 동일함의 여부를 반환
- 같은 학번의 학생인 경우 여러 인스턴스의 주소 값은 다르지만, 같은 학생으로 처리해야 학점이나 정보 산출에 문제가 생기지 않으므로 이런 경우 `equals()` 메서드를 재정의 함

```Java
class Student {
	int studentID;
	String studentName;

	public Student(int studentID, String studentName) {
		super();
		this.studentID = studentID;
		this.studentName = studentName;
	}

	@Override
	public boolean equals(Object obj) {
		if (obj instanceof Student) {
			Student std = (Student) obj;
			if (studentID == std.studentID)
				return true;
			else
				return false;
		}
		return false;
	}

}

public class EqualsTest {

	public static void main(String[] args) {

		String str1 = new String("test");
		String str2 = new String("test");

		System.out.println(str1 == str2); // false (물리적인 비교,주소값)
		System.out.println(str1.equals(str2)); // true (논리적인 비교. 데이터값)

		Student std1 = new Student(1001, "Tomas");
		Student std2 = new Student(1001, "Tomas");

		System.out.println(std1 == std2); // false
		System.out.println(std1.equals(std2));
		// equals 를 오버라이딩 하기 전에 : false 출력
		// equals 를 위와 같이 오버라이딩 하면 : true 출력

	}

}

```

## 3.hashCode() 메서드
- `hash` : 정보를 저장, 검색하기 위해 사용 하는 자료구조
- 자료의 특정 값(키 값)에 대해 저장 위치를 반환해주는 해시 함수를 사용함
- 해시 함수는 어떤 정보인가에 따라 다르게 구현 됨
- `hashCode()` 메서드는 인스턴스의 저장 주소를 반환함
- 힙 메모리에 인스턴스가 저장되는 방식이 `hash`
- `hashCode()`의 반환 값 : 자바 가상 머신이 저장한 인스턴스의 주소값을 10진수로 나타냄

```java
//..생략
	@Override
	public int hashCode() {
		return studentID;
	}
//..생략
	public static void main(String[] args) {

		Student std1 = new Student(1001, "Tomas");
		Student std2 = new Student(1001, "Tomas");

		System.out.println(std1.hashCode()); // output : 1001
		System.out.println(std2.hashCode()); // output : 1001
		System.out.println(System.identityHashCode(std1));
		System.out.println(System.identityHashCode(std2));

	}
//..생략
```


## 4.clone() 메서드
- 객체의 원본 복제하는데 사용하는 메서드
- 원본을 유지해 놓고 복사본을 사용할 때
- `clone()` 메서드를 사용하면 객체의 정보(멤버변수 값)가 같은 인스턴스가 또 생성되는 것이므로 객체 지향 프로그램의 정보은닉, 객체 보호의 관점에서 위배될 수 있음
- 객체의 `clone()` 메서드 사용을 허용한다는 의미로 `cloneable` 인터페이스를 명시해 줌 

```java
class Point {
	int x;
	int y;

	public Point(int x, int y) {
		this.x = x;
		this.y = y;
	}

	@Override
	public String toString() {
		return "x=" + x + "," + "y=" + y;
	}
}

class Circle implements Cloneable {
	Point point;
	private int radius;

	public Circle(int x, int y, int radius) {
		super();
		point = new Point(x, y);
		this.radius = radius;
	}

	@Override
	public String toString() {
		return "원점은 " + this.point + "이고, 반지름은 " + radius + " 입니다.";
	}

	@Override
	protected Object clone() throws CloneNotSupportedException {
		return super.clone();
	}

}

public class CloneTest {

	public static void main(String[] args) throws CloneNotSupportedException {

		Circle cc = new Circle(10, 20, 5);
		Circle ccClone = (Circle) cc.clone();

		System.out.println(System.identityHashCode(cc));
		System.out.println(System.identityHashCode(ccClone));

		System.out.println(cc);
		System.out.println(ccClone);

	}

}
```


## 5.String 클래스
- Stirng을 선언하는 두 가지 방법
```java
    String str1 = new String("abc"); // 생성자의 매개변수로 문자열 생성
    String str2 = "test"; // 문자열 상수를 가리키는 리터럴 방식
```
- 인스턴스로 생성되면 힙 메모리에 리터럴 방식으로 선언하면 상수 풀에 있는 주소를 참조한다.
- 상수 풀의 문자열은 참조하면 모든 문자열이 같은 주소를 가르킨다.
- 한번 new로 선언된 문자열은 변경할 수 없다.
- 두개의 문자열을 연결하면 새로운 인스턴스가 생성됨.

```java
public class StringTest {

	public static void main(String[] args) {
		
		String str1 = new String("hello ");
		String str2 = new String("world");
		
		System.out.println(System.identityHashCode(str1)); 
		
		str1 = str1.concat(str2); // 두개의 문자열 연결
		
		System.out.println(str1); // output: hello world
		System.out.println(System.identityHashCode(str1)); //위의 주소값과 다름
	}

}
```

- 위 방식처럼 문자열 연결을 계속하면 메모리에 gabage가 많이 생길 수 있기 때문에 사용하지 않음. 만약 계속 연결해서 사용해야 한다면 `StringBuilder`, `StringBuffer`를 사용해야 한다.


## 6. StringBuilder, StringBuffer 사용하기
- 내부적으로 가변적인 char[] 배열을 가지고 있는 클래스
- 문자열을 여러번 연결하거나 변경할 때 사용하면 유용함
- 매번 새로 생성하지 않고 기존 배열을 변경하므로 gabage가 생기지 않는다.
- `StringBuffer` 는 멀티 쓰레드 프로그래밍에서 동기화(sybchronization)를 보장
- `StringBuilder`는 단일 쓰레드 프로그램에서 사용하기를 권장
- 문자열을 출력할땐 `toString()` 메소드 사용

```java
public class StringBuilderTest {

	public static void main(String[] args) {
		
		String str1 = new String("java");
		
		System.out.println(System.identityHashCode(str1));
		
		StringBuilder buffer = new StringBuilder(str1);
		System.out.println(System.identityHashCode(buffer));
	
		
		buffer.append(" and");
		buffer.append(" android");
		System.out.println(System.identityHashCode(buffer)); // 위 buffer 주소와 같음
		
		String str2 = buffer.toString();
		System.out.println(str2); // output : java and android

	}

}
```

## 7.Class 클래스
- 자바의 모든 클래스와 인터페이스는 컴파일 후 class 파일로 생성됨
- class 파일에는 객체의 정보 (멤버변수, 메서드, 생성자등) 가 포함되어 있음
- Class 클래스는 컴파일된 class 파일에서 객체의 정보를 가져올 수 있음

### 7.1 Class 클래스 오기
1. Object 클래스의 getClass() 메서드 사용하기
```java
    String s = new String();
    Class c = s.getClass(); // getClass()메서드의 반환형은 Class
```

2. 클래스 파일 이름을 Class 변수에 직접 대입하기

```java
    Class c = String.Class;
```

3. Class.forName("클래스 이름") 메서드 사용하기
```java
    Class c = Class.forName("java.lang.String");
```

### 7.2 Class 클래스로 정보 가져오기
- reflection 프로그래밍 : Class 클래스를 이용하여 클래스의 정보(생성자, 멤버변수, 메서드)를 가져오고 이를 활용하며 인스턴스를 생성하고, 메서드를 호출하는 등의 프로그래밍 방식
- 일반적으로 자료형을 알 수 있는 경우에는 사용하지 않음

### 7.3 Class.forName() 메서드로 동적 로딩 하기
- 실행 시 로딩되므로 경우에 따라 다른 클래스가 사용될 수 있어 유용하다.
- 동적 로딩이란? 컴파일 시에 데이터 타입을 알고 binding 되는 방식.
- 컴파일 타임에 체크 할 수 없으므로 해당 문자열에 대한 클래스가 없는 경우 예외 `ClassNotFoundException`이 발생할 수 있다.

