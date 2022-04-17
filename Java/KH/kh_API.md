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

## Chap03. 오버라이딩 메소드. Wrapper클래스와 자료형 변환, 숫자관련 클래스

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





## Chap04. 날짜 관련 클래스

## Chap05. 포맷 관련 클래스
