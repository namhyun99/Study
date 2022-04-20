## Chap01. API의 개념

> Application Programming Interface의 약자   
   - 자바 라이브러리로 자바로 코딩 시 자바에서 제공되는 여러가지 클래스, 인터페이스 등
   - 예시로는 아래 import처럼 API를 가져와 쓸수 있다.

```java

import java.util.*;

public class TestStringTokenizer{
    public static void main(String[] args){
        String str= "AA,BB,CC";
    }
}

```

<br>


## Chap02. 문자열 관련 클래스

### String 관련 클래스

### 1. String 클래스
   - 문자열 값 수정 불가능, immutable(불변)
   - 수정 시 수정된 문자열이 새로 할당 되어 새 주소를 넘기

### 2. `StringBuffer` 클래스
   - 문자열 값 수정 가능, mutable(가변)
   - 수정 삭제 등이 기존 문자열에 수정되어 적용
   - 기본 16문자 크기로 지정된 버퍼를 이용하며 크기 증가 가능
   - 쓰레드 safe기능 제공(성능 저하 요인)

### 3. `StringBuilder` 클래스
   - StringBuffer와 동일하나 쓰레드 safe기능을 제공하지 않음

### 4. `StringTokenizer` 클래스
   - String 클래스에서 제공하는 split()메소드와 같은 기능을 하는 클래스로 생성 시 전달 받은 문자열을 구분자로 나누어 각 토큰에 저장한다.
   - 단, 한번 사용된 토큰은 사라지고, 계속 사용하려면 변수에 담아두어야 한다.

```java
//.. 생략
	public void stMethod() {
		
		String str = "Brian, 20, Seoul, Man";
		StringTokenizer st = new StringTokenizer(str,",");
		
		while(st.hasMoreTokens()) {
			System.out.println(st.nextToken());
		}		
	}
//.. 생략
```
<br>

## Chap03. 오버라이딩 메소드. 

### 1. `equlas()`
   - Objext 클래스에 선언된 메소드
   - 객체간의 주소가 일치하면 true가 반환되도록 구현되어 있음
   - 동등 비교시 오버라이딩해서 구현하는 메소드

### 2. `hashCode()`
   - Object 클래스에 선언된 메소드
   - 객체의 주소를 반환하도록 구현되어 있음
   - 자바와 다른 언어로 구현된 native 메소드로 JNI(Java Native Interface)의 일종      
     (JNI의 일종인 메소드는 오버라이딩을 하는건 JVM이 날아갈 수 있어 위험하다.) 
   - 컬렉션 사용시 동등 비교시 같이 오버라이딩해서 구현해야 하는 메소드

### 3. `toString()`
    - Object 클래스에 선언된 메소드
    - 객체의 주소값을 String으로 반환하도록 구현됨
    - 주로 객체 정보를 반환을 위해 오버라이딩

### 4. `clone()`
    - Object 클래스에 선언된 메소드
    - 자바와 다른 언어로 구현된 native 메소드로 JNI(Java Native Interface)의 일종      
     (JNI의 일종인 메소드는 오버라이딩을 하는건 JVM이 날아갈 수 있어 위험하다.) 
    - 얕은(shallow) 복사와 깊은(deep) 복사의 개념이 존재

```java
public class Phone implements Cloneable {

	private String model; // 모델명
	private int price; // 가격 (천원단위)

// 생성자 및 getter,setter 생략...

	@Override
	public boolean equals(Object obj) {
		Phone other = (Phone) obj;
		if (model.equals(other.model) && price == other.price)
			return true;
		return false;
	}

	@Override
	public int hashCode() {
		final int prime = 31;
		int result = 1;
		result = prime * result + ((model == null) ? 0 : model.hashCode());
		result = prime * result + price;
		return result;
	}

	@Override
	public String toString() {
		return "모델 : " + model + ", 가격 : " + price;
	}

//	@Override
//	protected Object clone() throws CloneNotSupportedException {
//		return super.clone();
//	}

	// 1. 생성자 이용하기 (new 연산자 이용)
//	@Override
//	public Phone clone() {
//		return new Phone(this.model, this.price); // 얕은 복사
//	}

	// 2. Object 클래스의 clone 이용하기 (예외처리, Cloneable)
	@Override
	public Phone clone() {
		Phone result = null;
		try {
			result = (Phone) super.clone();
		} catch (CloneNotSupportedException e) {
			e.printStackTrace();
		}
		return result;
	}

}
```

<br>

## Chap04. Wrapper클래스와 자료형 변환, 숫자관련 클래스

### Wrapper 클래스
> Primitive Data Type을 객체화 해주는 클래스이다.

### Wrapper 클래스가 좋은 이유
- 자료형 변환이 가능하다.
- 기본 자료형들에게 기능을 주어 기능을 사용할 수 있다.

### Wrapper 기능
- String을 기본 자료형으로 바꾸기
- 기본 자료형을 Stirng으로 바꾸기

```java
	public void strToPrim() {

		String str = "123";

		byte b = Byte.parseByte(str);
		System.out.println(b);

		int i = Integer.valueOf(str);
		System.out.println(i);

	}

```


### 숫자관련 클래스 - 싱글톤
> 단 하나의 인스턴스를 사용하는 디자인 패턴   
> (인스턴스가 필요한 때 똑같은 인스턴스를 만들어 내는 것이 아니라, 동일(기존) 인스턴스를 사용하게 함)

- Math 클래스가 대표적, `static` 영역

```java
public class MathDateController {

	public void mathMethod() {
		
		double num = -4.949;
		// abs 절대값
		System.out.println(Math.abs(num));  // 4.949
		
		// ceil 올림
		System.out.println(Math.ceil(num)); // -4.0
		
		// floor 버림
		System.out.println(Math.floor(num));  // -5.0
		
		//random 0~0.9999
		int ranNum = (int) (Math.random()) * 10 + 1;  // 1~10
		System.out.println(ranNum);
		
		// util의 Random 클래스 사용
		Random rn = new Random();
		int ranNum1 = rn.nextInt(4) + 1;
		System.out.println(ranNum1);
		
		// rint 가장 가까운 정수값
		System.out.println(Math.rint(num)); // -5.0
		
		// round 반올림한 정수
		System.out.println(Math.round(num)); // -5
		
		// sqrt 제곱근(루트) 계산
		System.out.println(Math.sqrt(121)); // 11.0
		
		// pow 제곱
		System.out.println(Math.pow(2, 3)); // 8.0

	}
}


```



<br>

## Chap05. 날짜 관련 클래스

### DATE 클래스
- 시스템으로 부터 현재 날짜, 시간 정보를 가져와서 다룰 수 있게 만들어진 클래스 
- 생성자 2개만 사용 가능하고 나머지는 모두 deprecated (앞으로 없어질 메소드들, 하지만 알아두어야 한다.)
- `Calendar` 클래스 혹은 `GreforianCalendar` 클래스 사용 권장
 
```java
		Date today = new Date();
		System.out.println(today);
		
		// 원하는 시간을 뽑아보자
		System.out.println("getTime() : " + today.getTime());
		System.out.println("getDate() : " + today.getDate());
		System.out.println("getDay() : " + today.getDay());
		System.out.println("getHours() : " + today.getHours());
		System.out.println("getMinutes() : " + today.getMinutes());
		System.out.println("getMonth() : " + (today.getMonth()+1));
		System.out.println("getSeconds() : " + today.getSeconds());
		System.out.println("getYear() : " + (today.getYear() + 1900));

        Date when = new Date(123456789L);
		// long형 정수 값을 가지고 날짜 시간 계산
		//1970년 1월 1일 0시 0분 0초를 기준으로 함
	   
```



### Calendar (캘린더) 클래스
- 생성자가 `protected`이기 때문에 new연산자를 통해 객체 생성 불가능 `getInstance()` 메소드를 통해서 객체 생성
  
  1. `Calendar` 는 월만 -1
  2. TimeZone 기능 제공
  3. field number 개념 도입

```java
//.. 생략
System.out.println("-----calendar -----");
		// 1.  Calendar 는 월만 -1
		// 2. TimeZone 기능 제공
		// 3. field number 개념 도입
		
		Calendar calendar = Calendar.getInstance();
		
		calendar.set(2022, 4-1, 19);
		
		int year = calendar.get(Calendar.YEAR);
		int month = calendar.get(Calendar.MONTH)+1;
		int date = calendar.get(Calendar.DATE);
		int hour = calendar.get(Calendar.HOUR);
		int minute = calendar.get(Calendar.MINUTE);
		int sec = calendar.get(Calendar.SECOND);
		
		System.out.println(year + "년 " 
							+ month + "월 " 
							+ date + "일 " 
							+ hour + ":" 
							+ minute + ":" 
							+ sec);
		
		SimpleDateFormat sdf1 = new SimpleDateFormat("yyyy-MM-dd E hh:mm:ss");
		System.out.println(sdf1.format(calendar.getTime()));

		// add
		// clone을 써서 날짜의 사본을 만들어서 날짜를 더하기 빼기를 해보자
		// calendar  (2022-4-19)
		
		Calendar cal = (Calendar)calendar.clone();
		cal.add(Calendar.YEAR, -9);
		cal.add(Calendar.MONTH, -3);
		cal.add(Calendar.DATE, 14);
		
		System.out.println("9년전, 3달전, 2주후 : " + cal.getTime());
		
		
		// 쓸 수 있는 타임 존
		
//		for(String name : TimeZone.getAvailableIDs()) {
//			System.out.println(name);
//		}
		// Etc/Greenwich(그리니치 천문대)(표준시)
		TimeZone tz = TimeZone.getTimeZone("Etc/Greenwich");
		sdf1.setTimeZone(tz);
		System.out.println(sdf1.format(calendar.getTime()));
//.. 생략
```

### GregorianCalendar (그레고리안 캘린더) 클래스
- 윤년의 개념이 더해져서 계산된다.
- 상위클래스 `Calendar`를 상속받은 클래스이다. 즉 Calendar의 후손클래스

```java
		System.out.println("--------그레고리안 -----------");
		GregorianCalendar gc = new GregorianCalendar();
		// 윤년
		// 1년을 365 >> (율리우스력) 365일 1/4일 >> 4년마다 하루를 더 쳐서 366일 쌓이는 시간을 소모
		
		// 윤년 상세 설명
		// 1. 그 해의 연도가 4의 배수가 아니면 평년으로 2월은 28일만 있다.
		// 2. 만약 연도가 4의 배수면서 100의 배수가 아니면 윤일(2월 29일)을 도입한다.
		// 3. 만약 연도가 100의 배수이면서 400의 배수가 아닐 때 이 해는 평년으로 생각한다.
		// 4. 만약 연도가 400의 배수면 윤일 (2월 29일)을 도입한다.
		
		
		System.out.println(gc.getTime());
		
		System.out.println("YEAR : " + gc.get(Calendar.YEAR));
		System.out.println("MONTH : " + (gc.get(GregorianCalendar.MONTH)+1));
		System.out.println("DATE : " + gc.get(GregorianCalendar.DAY_OF_MONTH));
		System.out.println("HOUR : " + gc.get(GregorianCalendar.HOUR_OF_DAY));
		System.out.println("MINUTE : " + gc.get(GregorianCalendar.MINUTE));
		System.out.println("SECOND : " + gc.get(GregorianCalendar.SECOND));
		
		
		gc.set(2022, 9-1, 18);
		System.out.println(gc.getTime());
```




<br>

## Chap06. 포맷 관련 클래스

### SimpleDateFormat
- Date의 날짜, 시간 정보를 원하는 format으로 출력하는 기능 제공, java.text 패키지에 속해있음
- `E` 는 요일을 나타낸다.
- `MMM` 으로 작성하면 '0월' 이라고 나타난다.

```java
		SimpleDateFormat sdf1 = new SimpleDateFormat("yyyy-MM-dd E hh:mm:ss");
		System.out.println(sdf1.format(calendar.getTime()));
```


### Formatter 클래스
- 값 출력 시 format 적용하여 출력
- `Formatter` 객체 생성 시 변환된 결과를 보낼 곳의 정보를 생성자 인자로 전달
- `printf()` 라고 생각하면 된다.

```java
	Formatter f = new Formatter(System.out);
	f.format("%s, %d, %d \n","String", 10, 20);
```