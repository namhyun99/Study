# Array 와 ArrayList

<br>


## 배열은 왜 써야 할까?
    정수 20개를 이용한 프로그램을 할 때 20개의 정수 타입의 변수를 선언해야 한다.   
    이렇게 되면, 비효율적이고 변수 관리도 힘들다 
    => 배열은 동일한 자료형의 변수를 한꺼번에 순차적으로 관리 할 수 있다.  
  


## 배열

### 1. 배열 선언 및 사용하기 

배열은 선언과 동시에 초기화 할 수 있다.  
배열을 초기화 할 때는 배열의 개수를 명시하지 않음  
아무런 초기화 값이 없는 경우, 정수는 0, 실수는 0.0 객체 배열은 null로 초기화 됨.


```java

    public static void main(String[] args){

        //arr 배열 초기화
        int[] arr = new int[] {0, 1, 2};
        
        // for 문을 이용하여 배열 출력하기
        // .length 메소드는 배열의 길이를 출력
        for (int i = 0; i < numbers.length; i++) {
			System.out.println(numbers[i]);
		}
    }

```


### 2. 객체 배열 사용하기

참조 자료형을 선언하는 객체 배열   
배열만 생성 한 경우 요소는 null로 초기화됨   


```java

// .. 생략
	public static void main(String[] args) {
		
        //객체 배열 선언
		Book[] library = new Book[5];
		
        // 객체 배열 출력
		for (int i = 0; i < library.length; i++) {
			System.out.println(library[i]); //output: null 값을 반환
		}

	}
// .. 생략

```


각 요소를 new를 활용하여 생성하여 저장해야 함   


```java
// .. 생략
	public static void main(String[] args) {
		
		Book[] library = new Book[5];
		
		library[0] = new Book("태백산맥1", "조정래");
		library[1] = new Book("태백산맥1", "조정래");
		library[2] = new Book("태백산맥1", "조정래");
		library[3] = new Book("태백산맥1", "조정래");
		library[4] = new Book("태백산맥1", "조정래");
		
		for (int i = 0; i < library.length; i++) {
			library[i].showBookInfo();
		}
	}

// .. 생략
```


### 3. 배열 복사 하기

기존 배열과 같은 배열을 만들거나 배열이 꽉 찬 경우 더 큰 배열을 만들고 기존 배열 자료를 복사할 수 있습니다.


```java

    int[] arr1 = {1,2,3,4};
    int[] arr2 = {10,20,30,40};

    System.arraycopy(src,srcPos,dest,destPos,length);
    System.arraycopy(arr1, 0, arr2, 1, arr1.length);


```

|매개변수|설명|
|---|---|
|src|복사할 배열 이름|
|srcPos|복사할 배열의 첫 번째 위치|
|dest|복사해서 붙여 넣을 대상 배열 이름|
|destPos|복사해서 대상 배열에 붙여 넣기를 시작할 첫 번재 위치|
|length|src에서 dest로 자료를 복사할 요소 개수|



### 4. 객체 배열 복사 하기

#### 1. 얕은 복사 : 배열 요소의 주소만 복사 되므로 배열 요소가 변경되면 복사된 배열의 값도 변경됨

```java

	public static void main(String[] args) {

		Book[] bookArr1 = new Book[3];
		Book[] bookArr2 = new Book[3];

		bookArr1[0] = new Book("태백산맥1", "조정래");
		bookArr1[1] = new Book("태백산맥2", "조정래");
		bookArr1[2] = new Book("태백산맥3", "조정래");

		System.arraycopy(bookArr1, 0, bookArr2, 0, bookArr1.length); //얕은복사

		for (int i = 0; i < bookArr2.length; i++) {
			bookArr2[i].showBookInfo();
		}
		
		// arr1 값을 변경했지만
		bookArr1[0].setBookName("나목");
		bookArr1[0].setAuthor("박완서");
		
		
		// 2개의 결과값은 같음
		for (int i = 0; i < bookArr2.length; i++) {
			bookArr2[i].showBookInfo(); 
			bookArr1[i].showBookInfo();
		}	

	}

```


#### 2. 깊은 복사 : 서로 다른 인스턴스의 메모리를 요소로 가지게 됨. 


```java

	public static void main(String[] args) {

		Book[] bookArr1 = new Book[3];
		Book[] bookArr2 = new Book[3];

		bookArr1[0] = new Book("태백산맥1", "조정래");
		bookArr1[1] = new Book("태백산맥2", "조정래");
		bookArr1[2] = new Book("태백산맥3", "조정래");
		
		// 깊은 복사를 위해 새로운 인스턴스를 생성해준다.
		bookArr2[0] = new Book();
		bookArr2[1] = new Book();
		bookArr2[2] = new Book();


		// 깊은 복사
		for (int i = 0; i < bookArr2.length; i++) {
			bookArr2[i].setAuthor(bookArr1[i].getAuthor());
			bookArr2[i].setBookName(bookArr1[i].getBookName());
		
		}
		// arr1 값을 변경했지만
		bookArr1[0].setBookName("나목");
		bookArr1[0].setAuthor("박완서");
		
		
		// 2개의 결과값 다름.
		for (int i = 0; i < bookArr2.length; i++) {
			bookArr1[i].showBookInfo();
		}	
		
		for (int i = 0; i < bookArr2.length; i++) {
			bookArr2[i].showBookInfo();
		}	

	}

```


#### 3. 향상된 for문 

배열 요소의 처음 부터 끝까지 모든 요소를 참조 할 때 편리한 반복문
배열의 모든 요소를 출력할 때 유용하게 사용

```java

		String[] strArr = {"JAVA", "Android", "C"};
		
		for(String s : strArr) {
			System.out.println(s);
            // outpur : JAVA Android C 반환
		}

```


## 다차원 배열 

2차원 이상의 배열   
지도, 게임, 등 평면이나 공간을 구현 할 때 많이 사용됨   
2차원 배열의 선언과 구조는 아래와 같다.

```java

int[][] arr = {{1,2,3},{4,5,6};

```

위 코드의 구조를 표로 나타내자면 아래와 같다. (행을 기준으로 잡고 열을 돌림)

||0열|1열|2열|
|:---:|:---:|:---:|:---:|
|0행|1|2|3|
|1행|4|5|6|


알파벳을 순차적으로 이차원 리스트에 넣고 출력하기

```java

	public static void main(String[] args) {

		char[][] alphabets = new char[2][13];
		char ch = 'A';

        //이중 for문을 돌려서 2차원 배열 접근
		for (int i = 0; i < alphabets.length; i++) {
			for (int j = 0; j < alphabets[i].length; j++) {
				alphabets[i][j] = ch; // 이차원 배열에 ch값을 넣어준다
				ch++; // 넣고난 후 하나씩 증가한다
			}
		}

        // 이중  for문을 이용하여 이차원 배열 출력하기	
		for (int i = 0; i < alphabets.length; i++) {
			for (int j = 0; j < alphabets[i].length; j++) {
				System.out.print(alphabets[i][j] + " ");
			}
			System.out.println(); // 개행
		}
		

	}

```



## ArrayList 클래스 

기존 배열은 길이를 정하여 선언하므로 사용 중 부족한 경우 다른 배열로 복사하는 코드를 직접 구현해야 한다.   
중간의 요소가 삭제되거나 삽입되는 경우도 나머지 요소에 대한 조정하는 코드를 구현해야 한다.   

ArrayList 클래스는 자바에서 제공되는 객체 배열이 구현된 클래스.   
여러 메서드와 속성등 사용하여 객체 배열을 편리하게 관리 할 수 있다.   
**가장 많이 사용하는 객체 배열 클래스**

```java

import java.util.ArrayList;

public class ArrayListTest {

	public static void main(String[] args) {
		
		ArrayList<String> list = new ArrayList<String>();
		
		list.add("aaa");
		list.add("bbb");
		list.add("ccc");
		
		for(String s : list) {
			System.out.println(s);
		}
		
	}
}
		
```


#### 1. 주요 메서드

|메서드|설명|
|---|---|
|.boolean add(E e)|요소 하나를 배열에 추가. E는 요소의 자료형을 의미|
|.size()|배열에 추가된 요소 전체 개수를 반환|
|.get(int index)|배열의 index 위치에 있는 요소 값을 반환|
|.remove(int index)|배열의 index 위치에 있는 요소 값을 제거하고 그 값을 반환|
|.boolean isEmpty()|배열이 비어 있는지 확인|