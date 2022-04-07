# 인터페이스(interface)
<br>

 - `인터페이스`에서 `멤버변수`는 `public static final`을 쓰지 않아도 프리컴파일 과정에서 자동선언
 - `메서드`는 `public abstract` 로 자동으로 선언 
 - 즉, `인터페이스`에서는 `상수`와 `추상메소드`가 선언된다. 

```java
package interfaceex;

public interface Calc {
	
	double PI = 3.14;   // 멤버변수 : public static final
	int ERROR = -99999999;
		
	int add(int num1, int num2);  // 메소드 ( public abstract )
	int sub(int num1, int num2);
	int times(int num1, int num2);
	int divdie(int num1, int num2);

}
```


## 인터페이스 구현과 형 변환
- 인터페이스를 구현한 클래스는 인터페이스 형으로 선언한 변수로 전환할 수 있다.
- 상속에서의 형 변환과 동일한 의미
- 단 클래스 상속과 달리 구현코드가 없기 때문에 여러 인터페이스를 구현할 수 있다.
- 형 변환시 사용할 수 있는 메서드는 인터페이스에 선언된 메서드만 사용할 수 있다.
  

## 인터페이스와 다형성
- 인터페이스는 "Client Code"와 서비스를 제공하는 "객체" 사이의 약소이다.
- 어떤 객체가 어떤 `interface` 타입이라 함은 그 `interface`가 제공하는 메서드를 구현했다는 의미이다.
- Client는 어떻게 구현되었는지 상관없이 `interface`의 정의만을 보고 사용할 수 이따. (ex: JDBC)
- 다양한 구현이 필요한 `interface`를 설계하는 일은 매우 중요한 일이다.


## 왜 인터페이스를 사용하는가?

- UserInfoWeb 은 IUserInfoDao 에 정의된 메소드 명세만 보고 Dao를 사용할 수 있고, Dao 클래스들은 IUserInfoDao에 정의된 메소드를 구현할 책임이 있다.


## 인터페이스의 요소
- 상수 : 모든 변수는 상수로 변환 됨
- 추상 메서드 : 모든 메서드는 추상 메서드로 구현
- 디폴트 메서드 : 기본 구현을 가지는 메서드, 구현 클래스에서 재정의 할 수 있음 (Java8 부터 추가)
```java
//..Calc class
	default void description() {
		System.out.println("정수 계산기를 구현합니다.");
	}
//..

//.. run class
	public static void main(String[] args) {

		Calc calc = new CompleteCalc();
		calc.description();
    }
//
```

- 정적 메서드 : 인스턴스 생성과 상관 없이 인터페이스 타입으로 사용할 수 있는 메서드 (Java8 부터 추가)

```java
//..Calc class
	static int total (int[] arr) {
		int total = 0;
		
		for(int i : arr) {
			total += i;
		}
		return total;
	}
//..

//..run class

	public static void main(String[] args) {

		int[] arr = {1,2,3,4,5};
		int sum = Calc.total(arr);  // 인스턴스 선언없이도 인터페이스 이름으로 사용
		System.out.println(sum);
		
	}
//
```

- private 메서드 : 인터페이스를 구현한 클래스에서 사용하거나 재정의 할 수 없음. 인터페이스 내부에서만 기능을 제공하기 위해 구현하는 메서드 (Java8 부터 추가)



## 인터페이스 상속
- 인터페이스 간에도 상속이 가능
- 구현코드의 상속이 아니므로 형 상속(type inheritance) 라고 함

```java
//인터페이스 x클래스
interface X {
	void x();
}
//인터페이스 y클래스
interface Y {
	void y();
}
//인터페이스 myinterface클래스에 x,y인터페이스 상속
interface MyInterface extends X, Y { // 인터페이스는 여러개 상속 가능
	void myMethod();
}

// MyClass에 Myinterface를 implements함
public class MyClass implements MyInterface {

	@Override
	public void x() {
		System.out.println("x()");
	}

	@Override
	public void y() {
		System.out.println("y()");
	}

	@Override
	public void myMethod() {
		System.out.println("myMethod()");
	}

    //실행을 위한 메인메소드
	public static void main(String[] args) {

		MyClass myclass = new MyClass();

		X xClass = myclass;
		xClass.x(); // output : x()

	}

}

```