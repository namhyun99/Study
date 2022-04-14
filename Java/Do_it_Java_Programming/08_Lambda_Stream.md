# 내부 클래스 - 람다식,스트림

## 내부 클래스 요약

|종류|구현위치|사용할 수 있는 외부 클래스 변수|생성 방법|
|:---:|:---:|:---:|:---:|
|인스턴스 내부 클래스|외부 클래스 멤버 변수와 동일| 외부 인스턴스 변수 외부 전역 변수|외부클래스를 먼저 만든 후 내부 클래스 생성|
|정적 내부 클래스|외부 클래스 멤버 변수와 동일|외부 전역 변수|외부 클래스와 무관하게 생성|
|지역 내부 클래스|메서드내부에 구현|외부 인스턴스 변수 외부 전역 변수|메서드를 호출할 때 생성|
|익명 내부 클래스|메서드 내부에 구현 변수에 대입하여 직접구현|외부 인스턴스 변수 외부 전역 변수|메서드를 호출할 때 생성되거나, 인터페이스 타입 변수에 대입할 때 new예약어를 사용하여 생성|


### 1. 인스턴스 내부 클래스 예제
```java

class OutClass{
	
	private int num = 10;
	private static int sNum = 20;
	private InClass inClass;
	
	
	public OutClass() {
		inClass = new InClass();
	}
	
	private class InClass{
		int inNum = 200;
		static int sInNum = 100;
		
		void inTest() {
			System.out.println(num);
			System.out.println(sNum);
		}
	}
	
	public void usingInTest() {
		inClass.inTest();
	}
}

public class InnerClass {
	public static void main(String[] args) {
		OutClass outClass = new OutClass();
		outClass.usingInTest();  // output : 10 20 
    }
}

```

### 2. 정적 내부 클래스 예제
```java
class OutClass{
	
	private int num = 10;
	private static int sNum = 20;
	
	static class InStaticClass{
		int iNum = 100;
		static int sInNum = 200;
		
		void inTest() {
			//num += 10;
			sNum += 10;
			System.out.println(sNum);
			System.out.println(iNum);
			System.out.println(sInNum);
		}
		
		static void sTest() {
			System.out.println(sNum);
			// System.out.println(iNum); // 정적변수만 사용가능
			System.out.println(sInNum);		
		}
		
	}
	
}
public class InnerClass {
	public static void main(String[] args) { 		
		OutClass.InStaticClass.sTest();  // output : 20 200
	}
}

```

### 3.지역 내부 클래스 예제
```java
class Outer{
	
	int outNum = 100;
	static int sNum = 200;
	
	public Runnable getRunnable() {
		
		int localNum = 300; // 로컬변수는 변화할 수 없다. final로 선언되기 때문
		class MyRunnable implements Runnable{

			@Override
			public void run() {
				System.out.println(outNum);
				System.out.println(sNum);
				System.out.println(localNum);
			}
		}
		
		return new MyRunnable();
		
	}
}

public class LocalInnerTest {
	public static void main(String[] args) {
		
		Outer outer = new Outer();
		outer.getRunnable().run();
		
	}

}
```
### 4.익명 내부 클래스 예제
- 안드로이드에서 쓰레드 만들때 많이 사용한다.
- 단 하나의 추상클래스나 인터페이스만 구현할 수 있다.

```java
class Outer{
	
	int outNum = 100;
	static int sNum = 200;
	
	Runnable runnable = new Runnable() {
		
		@Override
		public void run() {
			System.out.println("runnable");
		}
	};
	
	
	
	public Runnable getRunnable() {
		
		int localNum = 300; // 로컬변수는 변화할 수 없다. final로 선언되기 때문
		return new Runnable() {
			
			@Override
			public void run() {
				System.out.println(outNum);
				System.out.println(sNum);
				System.out.println(localNum);				
			}
		};		
	}
}

public class LocalInnerTest {
	public static void main(String[] args) {
		
		Outer outer = new Outer();
		outer.getRunnable().run();
		
		outer.runnable.run();
		
	}

}
```


## 람다식 (lambda expression)
- 자바에서 함수형 프로그래밍을 구현하는 방식 (자바 8부터 지원)
- 클래스를 생성하지 않고 함수의 호출만으로 기능을 수행
- 입력 받은 자료를 기반으로 수행되고 외부에 영향을 미치지 않으므로 병렬 처리가 가능하다. 
- 안정적인 확장서 있는 프로그래밍 방식


### 람다식 구현하기
1. 익명 함수 만들기
2. 매개 변수와 매개 변수를 활용한 실행문으로 구현
3. 두 수를 입력 받아 더하는 add()함수
   ```java
	int add(int x, int y){
		return x + y;
	}

	// 위 식을 람다식으로 표현하면 아래와 같다.

   (int x, int y) -> {return x+y;}
   ```
4. 함수 이름 반환형을 없애고 -> 를사용
5. {}까지 실행문을 의미

### 람다식 문법
- 매개벼순(하나인 경우) 자료형과 괄호 생략 가능
- 매개 변수가 두 개 인 경우 괄호를 생략할 수 없음
- 중괄호 안의 구현부가 한 문장인 경우 중괄호 생략 가능
- 중괄호 안의 구현부가 한 문장이라도 return문은 중괄호 생략할 수 없음
- 중괄호 안의 구현부가 반환문 하나라면 return과 중괄호 모두 생략 가능


```java
@FunctionalInterface // 함수형 인터페이스 선언 애노테이션
public interface MyNumber {
	int getMaxNumber(int num1, int num2); 
	//람다식을 사용하기 위한 인터페이스는 메서드가 무조건 한개여야 한다.
}

public class TestMyNumber {
	public static void main(String[] args) {
		
		MyNumber maxNum = (x, y) -> (x >= y)? x: y;
		
		int max = maxNum.getMaxNumber(10, 20);	
		System.out.println(max); // 20
	}
}
```

### 익명 객체를 생성하는 람다식
- 자바는 객체 지향 언어로 객체를 생성해야 메서드가 호출 된다.
- 람다식으로 메서드를 구현하고 호출하면 내부에서 익명 클래스가 생성된다.
- 람다식에서 외부 메서드의 지역변수는 상수로 처리된다. (지역 내부 클래스와 동일한 원리)


### 함수를 변수처럼 사용하는 람다식
```java
interface PrintString{
	void showString(String str);
}

public class LambdaTest {

	public static void main(String[] args) {
		
		// 변수에 대입해서 사용하는 방법
		PrintString LP = str -> System.out.println(str);
		LP.showString("test");
		
		// 매개 변수로 넘어가는 방법
		showMyString(LP);
		
		// 반환값으로 넘기는 방법
		PrintString reStr = returnPrint();
		reStr.showString("hello");
	}

	private static void showMyString(PrintString lP) {
		lP.showString("test2");
	}

	private static PrintString returnPrint() {
		return s -> System.out.println(s + " wrold");
	}

}
```


## 스트림(stream)
- **자료의 대상과 관계 없이 동일한 연산을 수행**
  - 배열, 컬렉션을 대상으로 동일한 연산을 수행 함
  - 일관성 있는 연산으로 자료의 처리를 쉽고 간단하게 함
- **한번 생성하고 사용한 스트림은 재사용 할 수 없음**
  - 자료에 대한 스트림을 생성하여 연산을 수행하면 스트림은 소모됨
  - 다른 연산을 위해서는 새로운 스트림을 생성 함
- **스트림 연산은 기존 자료를 변경하지 않음**
  - 자료에 대한 스트림을 생성하면 별도의 메모리 공간을 사요하므로 기조 ㄴ자료를 변경하지 않음
- **스트림 연산은 중간 연산과 최종연산으로 구분됨**
  - 스트림에 대해 중간 연산은 여러 개 적용될 수 있지만 최종 연산은 마지막에 한 번만 적용됨
  - 최종연산이 호출되어야 중간 연산의 결과가 모두 적용됨. 이를 '지연 연산'이라 함