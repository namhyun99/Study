
# 클래스와 객체1

## 목차


## 객체지향 프로그래밍(Object Oriented Programming)
    
    - 객체를 기반으로 하는 프로그래밍
    - 객체를 정의
    - 객체의 기능구현
    - 객체의 협력

 1. 클래스 : 객체에 대한 속성과 기능을 코드로 구현한 것. 설계도
    - 객체의 속성 > 멤버 변수
    - 객체의 기능 > 메서드

 2. 메서드 : 함수의 일종, 객체의 기능을 제공하기 위해 클래스 내부에 구현되는 함수

 3. 함수란?
    - 하나의 기능을 수행하는 일련의 코드 중복되는 기능은 함수로 구현하여 함수를 호출하여 사용함.

## 용어정리
- `객체` : 객체 지향 프로그램의 대상, 생성된 인스턴스
- `클래스` : 객체를 프로그래밍하기 위해 코드로 만든 상태
- `인스턴스` : 클래스가 메모리에 생성된 상태
- `멤버변수` : 클래스의 속성, 특성
- `메서드` : 멤버 변수를 이용하여 클래스의 기능을 구현
- `참조 변수` : 메모리에 생성된 인스턴스를 가리키는 변수
- `참조 값` : 생성된 인스턴스의 메모리 주소 값
- `Stack` : 지역변수들이 자리를 잡는곳 ( 함수, 클래스 등 )
- `Heap` : 동적으로 생성되는 메모리 / 인스턴스화시 저장되는곳

## 생성자(constructor)
- 내가 처음 이 객체를 사용하면서 해야하는 일들.
- 인스턴스를 초기화 할 때의 명령어 집합


## 참조 자료형 (reference data type)
- 변수의 자료형 : int, long, float, double등
- 참조 자료형 : String, Data, className 등


## 정보은닉(information hiding) / 접근제어자
 - private : 클래스의 외부에서 클래스 내부의 멤버 변수나 메서드에 접근하지 못하게 하는 접근 제어자
 - public : 아무대서나 가져다가 쓸수 있도록 공용의 개념
 - (defualt) : 기본 접근제어자, 같은 패키지 내에서만 사용.
 - 변수가 필요한 경우 getter / setter를 사용한다.



# 클래스와 객체2


## this 키워드

    - 자신의 메모리를 가리킴 
    - 생성자에서 다른 생성자 호출
    - 자신의 주소를 반환 함 (자주 사용하지는 않음)


**자신의 메모리를 카리키는 this**   
    - 생성된 인스턴스 스스로를 가리키는 예약어
    - 아래 코드에서 this를 생략하게 되면 name이나 age는 파라미터로 사용되는 변수로 인식된다.
    - 아래 코드의 경우 this 사용 시 클래스 내에 있는 멤버변수를 가르킴

```java

public class Person {
    private String name;
    private int age;


    public Person(String name, int age){
        this.name = name;
        this.age = age;
    }
}

```

**생성자에서 다른 생성자를 호출 하는 this**   
    - this를 이용하여 다른 생성자를 호출할 때는 그 이전에 어떠한 statement도 사용할 수 없다.
    - 아래와 같이 생성자가 여러개 이고 파라미터만 다른 경우 ***constructor overloading*** 이라고 한다.

```java

    //.. 생략
    public Person(){
        this("이름없음",1) // 여기서 this는 아래 생성자를 불러옴
    }

    public Person(String name, int age){
        this.name = name;
        this.age = age;
    }

```


## static 변수
    - 여러 개의 인스턴스가 같은 메모리의 값을 공유하기 위해 사용
    - 인스턴스가 생성될 때 마다 다른 메모리를 가지는것이 아니라 프로그램이 메모리에 적재(load) 될대 데이터 영역의 메모리에 생성 됨
    - 클래스 변수라고도 불림. 
    - 클래스 변수는 인스턴스로 불러오지 않고, 클래스 이름으로 불러온다.


```java
public class Student {
	
	static int serialNum = 1000;  // 이와 같이 사용
	
	int studentID;
	String studentName;
	
	public Student() {
		serialNum++;
		studentID = serialNum;
	}
}
```


### static 변수 vs 인스턴스 변수
    - serialNumber를 static으로 선언하면 모든 student instance에 대해 하나의 변수로 유지되고 이러한 변수를 class 변수라 한다.


### static 변수 예
    - 여러 인스턴스가 하나의 메모리 값을 공유할 때 필요
    - 학생이 생성될 때 마다 학번이 증가해야 하는 경우 기준이 되는 값은 static  변수로 생성하여 유지함

### static 메서드

    - 클래스 메서드라고도 함
    - 메서드에서 static 키워드를 사용하여 구현
    - 주로 static 변수를 위한 기능 제공
    - static 메소드에서 인스턴스 변수를 사용할 수 없음

### 변수의 유효 범위

|변수 유형|선언 위치|사용 범위|메모리|생성과 소멸|
|:---:|:---:|:---:|:---:|:---:|
|지역변수(로컬 변수)|함수 내부에 선언|함수 내부에서만 사용|스텍|함수가 호출될 때 생성되고 함수가 끝나면 소멸함|
|멤버 변수(인스턴스 변수|클래스 멤버 변수로 선언|클래스 내부에서 사용하고 private이 아니면 참조 변수로 다른 클래스에서 사용 가능|힙|인스턴스가 생성될 때 힘에 생성되고, 가비지 컬렉터가 메모리를 수거할 때 소멸됨|
|static 변수(클래스변수)|static예약어를 사용하여 클래스 내부에 선언|클래스 내부에서 사용하고 private이 아니면 클래스 이름으로 다른 클래스에서 사용 가능|데이터 영역|프로그램이 처음 시작할 때 상수와 함께 데이터 영역에 생성되고 프로그램이 끝나고 메모리를 해제할 때 소멸됨|


### static 응용 : singleton 패턴
    전 시스템에 단 하나의 인스턴스만이 존재하도록 구현하는 방식

```java

class Company {
	
	// 외부에서 접근할 수 없는 클래스 변수
	private static Company company = new Company();
	
	// 생성자, 외부에서 사용할수 없음
	private Company() {
	}

	public static Company getCompany() {
		if(company == null)
			company = new Company();
		return company;
	}

	public static void setCompany(Company company) {
		Company.company = company;
	}
	
}

public class Run {

	public static void main(String[] args) {
		
		Company c1 = Company.getCompany();
		Company c2 = Company.getCompany();
		
		System.out.println(c1); // 주소값 동일
		System.out.println(c2); // 주소값 동일
	}

}

```