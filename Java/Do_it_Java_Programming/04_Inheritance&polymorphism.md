# 상속과 다형성
<br>


## 상속(Ingeritance)

- 클래스를 정의 할 때 이미 구현된 클래스를 `상속(inheritance)` 받아서 속성이나 기능이 확장되는 클래스를 구현함   
- 상위 클래스는 하위 클래스 보다 일반적인 의미를 가짐
- 하위 클래스는 상위 클래스 보다 구체적인 의미를 가짐
- is a 관계 성립시 상속 사용, 만약 has a 관계라면 객체를 선언하고 사용.
- `extends` 를 사용하며, `extends` 뒤에는 단 하나의 `class`만 사용할 수 있음 (단, `interface` 에서는 다중 상속 가능)

- 상속 문법
```java

    class B extends A {
        ...
    }

```
<br>

## 상속 예제

### 부모클래스
 - `protected` : 패키지가 달라도 상속된 모든 클래스에서 사용 가능하다.
 - `(defalut)` : 같은 패키지 않에서만 사용 가능한 접근제어자

```java
///.. 생략
public class Customer {
	
	protected int customerID;
	protected String customerName;
	protected String customerGrade;
	int bonusPoint;
	double bonusRatio;
	
	
	public Customer() {
		customerGrade = "SILVER";
		bonusRatio = 0.01;
	}

	public Customer(int customerID, String customerName) {
		super();
		this.customerID = customerID;
		this.customerName = customerName;
		customerGrade = "SILVER";
		bonusRatio = 0.01;
	}
//.. 생략

```


### 자식클래스 
 - 부모클래스를 상속 받았기 때문에 부모클래스의 기능을 자식클래스에서 사용할 수 있다.

```java
//..생략
public class VIPCustomer extends Customer {
	
	private int agentID;
	private double saleRatio;
	
	public VIPCustomer() {
		customerGrade = "VIP";
		bonusRatio = 0.05;
		saleRatio = 0.1;
	}

	public int getAgentID() {
		return agentID;
	}
//..생략
```

### 접근 제한자

||외부클래스|하위클래스|동일패키지|내부클래스|
|:---:|:---:|:---:|:---:|:---:|
|`public`|O|O|O|O|
|`protected`|X|O|O|O|
|`(default)`|X|X|O|O|
|`private`|X|X|X|O|



### 상속을 언제 사용할까?
 - 중복되는 코드가 많으면 클래스를 나누어 상속시키면 된다. (단 상속은 is a관계일때 사용, has a 관계일땐 객체 선언)
 - 만약 상속을 하지 않는다면 하나의 클래스 내에 많은 if문이 생길 수 있다.
 - 하지만 상속은 항상 사용하지 않아도 된다.

#### IS-A 관계 
 - 일반적인 관계/ 구체적인 관계라면 `extends` 키워드로 상속해주면 된다.

```java

    class B extends A {
        ...
    }

```



#### HAS-A 관계
 - `Student` 가 `Subject` 클래스를 포함한 관계. 즉, `Student`클래스에서 `Subject` 메서드등을 사용할 수 있다.

```java

class Student {
    Subject subject;
}


```


### 상속과 생성자
 - 상위(부모)클래스의 생성자가 먼저 실행되고 상속받은 하위(자식)클래스의 생성자가 출력된다.
 - 상위(부모) 클래스의 매개변수가 있는 생성자가 있고, 기본 생성자가 없다면, 하위(자식)클래스에서 기본생성자 첫출에 `super.(0, null)`로 하거나 상위(부모)클래스의 매개변수 값을 받아서 작성한다.

### 상속에서의 메모리 상태
 - 상위 클래스의 인스턴스가 먼저 생성이 되고, 하위 클래스의 인스턴스가 생성됨.

### 상위 클래스로의 묵시적 형 변환(업캐스팅)
 - 상위 클래스 형으로 변수를 선언하고 하위 클래스 인스턴스를 생성할 수 있음
 - 하위 클래스는 상위 클래스의 타입을 내포하고 있으므로 상위 클래스로 묵시적 형변환이 가능 함.
 - 만약 타입을 상위클래스로 선언했다면, 하위클래스의 멤버변수는 사용할 수 없다. 그 이유는 타입을 상위클래스로 선언했기 때문이다. (단 메서드의 경우는 하위 메서드가 먼저 불러진다.)

```java
//.. 생략
    
    //상위클래스 [변수명] = new 하위클래스();
    Customer vip1 = new VIPCustomer();

//..생략
```


<br>

### 오버라이딩(Overriding)

 - 상위 클래스에 정의 된 메서드 중 하위 클래스와 기능이 맞지 않거나 추가 기능이 필요한 경우 같은 이름과 매개변수로 하위 클래스에서 재정의 함.
 - `@Override` 라는 Annotation 을 사용
 - 아래 예제는 java Object의 toString() 메소드를 오버라이딩한 예제이다.

```java

//..생략
	@Override
	public String toString() {
		return "customerName= " + customerName + " , customerGrade=" + customerGrade + " , bonusPoint= "
				+ bonusPoint;
	}
//..생략

```


### 가상메서드 (virtual method)
 - 프로그램에서 어떤 객체의 변수나 메서드의 참조는 그 타입에 따라 이루어 짐. 가상 메서드의 경우는 타입과 상관없이 실제 생성된 인스턴스의 메서드가 호출 되는 원리


<br>


## 다형성(polymorphism)
 - 하나의 코드가 여러가지 자료형으로 구현되어 실행되는 것
 - 정보은닉, 상속과 더불어 객체지향 프로그래밍의 가장 큰 특징 중 하나
 - 객체지향 프로그래밍의 유연성, 재활용성, 유지보수성에 기본이 되는 특징임

<br>

## 다형성 예제

```java
package inheritance;

class Animal {
	public void move() {
		System.out.println("동물이 움직입니다.");
	}
}

class Human extends Animal {
	public void move() {
		System.out.println("사람이 두발로 걷습니다.");
	}
}

class Tiger extends Animal {
	public void move() {
		System.out.println("호랑이가 네발로 뜁니다.");
	}
}

class Eagle extends Animal {
	public void move() {
		System.out.println("독수리가 하늘을 날읍니다.");
	}
}

public class AnimalTest {

	public static void main(String[] args) {

		AnimalTest test = new AnimalTest();
		
		test.moveAninal(new Human());
		test.moveAninal(new Tiger());
		test.moveAninal(new Eagle());

	}

	public void moveAninal(Animal animal) {
		animal.move(); // 다형성을 나타내고 있음
	}

}
```

## 다운캐스팅 - instansof
 - 다시 원래 자료형인 하위 클래스로 형변환 하려면 명시적으로 다운 캐스팅 해야한다.
 - 이때 원래 인스턴스의 타입을 체크하는 예약어가 `instanceof` 이다.
 - 다운캐스팅은 좋은 방법은 아니다. 가능하면 오버라이딩으로 해결하는게 좋다. (단, Oject를 사용한다면 사용할 수도 있다.)


```java

Animal Animal = new Human();
if(Animal instanceof Human) { // hAnimal 인스턴스 자료형이 Human형이라면
    Human human = (Human) Animal; // 인스턴스 hAnimal을 Human형으로 다운 캐스팅
}

```



## 추상 클래스란? (abstract class)
 - 추상 메서드를 포함한 클래스이다.
 - 추상 메서드는 구현코드 없이 메서드의 선언만 있다
 - `abstract`예약어를 사용한다.
 - 추상 클래스는 new(인스턴스화) 할 수 없다.
 - 추상 클래스는 상속을 위한 클래스이다. 
 - `abstract` 클래스에서 구현된 메서드들은 상속받은 하위 클래스에서 구현해야 한다.
  

```java
//...생략
public abstract class Computer {
	
	public abstract void display();
	public abstract void typing();
	
	
	public void turnOn() {
		System.out.println("전원을 켭니다.");
	}
	
	public void turnOff() {
		System.out.println("전원을 끕니다.");
	}

//...생략

```


### 추상 클래스는 어디서 쓸까?
 - 이미 만들어진 프레임워크에서 많이 사용하고 있다.
 - 상위클래스에서 구현하지 못하는 부분이 있을 때 사용한다.


### 템플릿 메서드
 - 추상 메서드나 구현된 메서드를 활용하여 전체 기능의 흐름(시나리오)을 정의하는 메서드

### final 예약어
 - `final` 변수는 값이 변경될 수 없는 상수, 오직 한번만 값 할당.
 - `final` 메소드는 하위 클래스에서 재정의 할 수 없음.
 - `final` 클래스는 더이상 상속되지 않음


```java
public abstract class Car {

	public abstract void drive();
	public abstract void stop();

	public void startCar() {
		System.out.println("시동을 켭니다.");
	}

	public void turnOff() {
		System.out.println("시동을 끕니다.");
	}
		
	//아래 메소드가 템플릿 메서드이다.
	public final void run() {
		startCar();
		drive();
		stop();
		turnOff();
	}
}

```


