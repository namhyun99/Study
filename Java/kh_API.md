# Chap01. API의 개념
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

## String 관련 클래스

    1. String 클래스
        - 문자열 값 수정 불가능, immutable(불변)
        - 수정 시 수정된 문자열이 새로 할당 되어 새 주소를 넘기


    2. StringBuffer 클래스
        - 문자열 값 수정 가능, mutable(가변)
        - 수정 삭제 등이 기존 문자열에 수정되어 적용
        - 기본 16문자 크기로 지정된 버퍼를 이용하며 크기 증가 가능
        - 쓰레드 safe기능 제공(성능 저하 요인)


    3. StringBuilder 클래스
        - StringBuffer와 동일하나 쓰레드 safe기능을 제공하지 않음



# Chap02. 문자열 관련 클래스


# Chap03. 오버라이딩 메소드

# Chap04. Wrapper클래스와 자료형 변환

# Chap05. 날짜 관련 클래스

# Chap06. 포맷 관련 클래스
