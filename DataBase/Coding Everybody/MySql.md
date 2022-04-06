# 생활코딩 MySql

데이터베이스 Mysqp공부를 위해 유명한 이고잉님의 생활코딩강의로 시작하고자 한다.
내가본 강의는 아애와 같으며, 해당 글은 강의에 대한 내용을 요약정리한 내용이다.

    강의URL : https://opentutorials.org/course/3161   

<br>
   
   
## Mysql 설치

검색창에 'mysql community edition download' 로 검색한 후 공식사이트에 접속한다. 그리고 자신의 맞는 운영체제를 선택하여 설치한다. 설치링크는 아래와 같다.

    설치링크 : https://dev.mysql.com/downloads/mysql/



## bitnami WAMP 를 이용한 설치





## mysql 설치확인

 - 윈도우 (Windows)
   - cmd창을 켜준 후 아래와 같이 이동해주면 된다.
  
```cmd

> cd \경로주소

..\bin> mysql -uroot -p
..\bin> Enter password : 


```


## mysql 명령어

### CREATE 명령어
데이터베이스를 새로 생성할 때 사용한다.   
```
    CREATE DATABASE 데이터베이스명;
```

### DROP 명령어
생성된 데이터베이스를 삭제한다. 
```  
    DROP DATABASE 데이터베이스명 ;
```

### SHOW 명령어
생성된 데이터베이스를 보여준다.   

```
    SHOW DATABASE;
```

### USE 명령어
해당 데이터베이스 표를 사용할 것 이라는 명령어다.   
```
    USE 데이터베이스명;
```

### exit 명령어
데이터베이스 종료  or Ctrl+c 


### CREATE TABLE 명령어
데이터베이스 안에 테이블을 생성하는 명령어   

> `NULL` : 공백 허용   
> `NOT NULL`: 공백을 허용하지 않는다.    
> `AUTO_INCREMENT` : 중복되지 않는 숫자 자동 생성   
> `INT()` : 정수형 타입   
> `VARCHAR()` : 문자형 타입 (문자수) 만약 (30) 이면 30자 이상이 되면 자른다.   
> `DATETIME` : 시간, 날짜 타입   
> `PRIMARY KET(id))` : 중복되면 안되는 값을 지정.   

```cmd
MariaDB [opentutorials]> CREATE TABLE topic(
    -> id INT(11) NOT NULL AUTO_INCREMENT,
    -> title VARCHAR(100) NOT NULL,
    -> description TEXT NULL,
    -> created DATETIME NOT NULL,
    -> author VARCHAR(30) NULL,
    -> profile VARCHAR(100) NULL,
    -> PRIMARY KEY(id));
    )
Query OK, 0 rows affected (0.025 sec)

```
