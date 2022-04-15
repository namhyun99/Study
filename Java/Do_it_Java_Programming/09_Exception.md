# 예외처리

## 오류란 무엇일까?
- 컴파일오류(compile error) : 프로그램 코드 작성 중 발생하는 문법적 오류
- 실행 오류(runtime error) : 실행 중인 프로그램이 의도하지 않은 동작을 하거나(bug) 프로그램이 중지되는 오류
- 실행 오류 시 비정상 종료는 서비스 운영에 치명적
- 오류가 발생할 수 잇는 경우 로그(log)를 남겨 추후 이를 분석하여 원인을 찾아야 함
- 자바는 예외 처리를 통하여 프로그램의 비정상 종료를 막고 log를 남길 수 있음

## 오류와 예외 클래스
- 시스템 오류(error) : 가상 머신에서 발생, 프로그래머가 처리 할 수 없음, 동적 메모리 없느 ㄴ경우, 스택 오버 플로우 등
- 예외 (`Exception`) : 프로그램에서 제어할 수 있는 오류. 읽어 들이려는 파일이 존재하지 않는 경우, 네트워크 연결이 끊어진 경우
  

## 예외 클래스의 종류
- 모든 예외 클래스의 최상위 클래스는 `Exception`


## try-with-resources문
- 리소스를 자동 해제 하도록 제공해주는 구문 (자바 7부터 제공)
- close()를 명시적으로 호출하지 않아도 try{} 블록에서 열린 리소스는 정상적인 경우, 예외가 발생한 경우 모두 자동 해제  된다.
- 해당 리소스가 `AutoCloseable`을 구현 해야 함
- `FileInputStream`의 경우 `AutoCloseable`을 구현하고 있음

```java
public class ExceptionTest {

	public static void main(String[] args) {

		try (FileInputStream fis = new FileInputStream("a.txt")) {

		} catch (IOException e) {
			System.out.println(e);
		}

		System.out.println("end");

	}

}

```

## 예외 처리 미루기 
- `throws` 를 사용하여 예외처리 미루기
- 메서드 선언부에 `throws`를 추가
- 예외가 발생한 메서드에서 예외 처리를 하지 않고 이 메서드를 호출한 곳에서 예외 처리를 한다는 의미
- main()에서 `throws`를 사용하면 가상머신에서 처리 됨.
- `throw` 는 예외를 발생시키는 예약어, `throws`는 예외를 미루는 예약어이다.


## 사용자 정의 예외
- 사용자가 직접 만드는 오류 예외처리

```java
public class IDFormatException extends Exception {
	
	public IDFormatException(String message) {
		super(message);
	}

}
```

```java
package exception;

public class IDFormatTest {

	private String userID;

	public String getUserId() {
		return userID;
	}

	public void seUserID(String userID) throws IDFormatException {
		if (userID == null) {
			throw new IDFormatException("아이디는 null 일 수 없습니다.");
		} else if (userID.length() < 8 || userID.length() > 20) {
			throw new IDFormatException("아이디는 8자 이상 20자 이하로 쓰세요.");
		}

		this.userID = userID;
	}

	public static void main(String[] args) {

		IDFormatTest idTest = new IDFormatTest();
		String userID = null;

		try {
			idTest.seUserID(userID);
		} catch (IDFormatException e) {
			System.out.println(e); // 아이디는 null일 수 없습니다.
		}

		userID = "1234567";
		try {
			idTest.seUserID(userID);
		} catch (IDFormatException e) {
			System.out.println(e); // 아이디는 8자 이상 20자 이하로 쓰세요.
		}

	}

}

```