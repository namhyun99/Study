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
<br>

#### 1. CREATE 명령어
데이터베이스를 새로 생성할 때 사용한다.   
```
    CREATE DATABASE 데이터베이스명;
```

#### 2. DROP 명령어
생성된 데이터베이스를 삭제한다. 
```  
    DROP DATABASE 데이터베이스명 ;
```

#### 3. SHOW 명령어
생성된 데이터베이스를 보여준다.   

```
    SHOW DATABASE;
```

#### 4. USE 명령어
해당 데이터베이스 표를 사용할 것 이라는 명령어다.   
```
    USE 데이터베이스명;
```

#### 5. CREATE TABLE 명령어
데이터베이스 안에 테이블을 생성하는 명령어   

> `NULL` : 공백 허용   
> `NOT NULL`: 공백을 허용하지 않는다.    
> `AUTO_INCREMENT` : 중복되지 않는 숫자 자동 생성   
> `INT()` : 정수형 타입   
> `VARCHAR()` : 문자형 타입 (문자수) 만약 (30) 이면 30자 이상이 되면 자른다.   
> `DATETIME` : 시간, 날짜 타입   
> `PRIMARY KET(id))` : 중복되면 안되는 값을 지정.   

```sql

Mysql [opentutorials]> CREATE TABLE topic(
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

<br>

## 데이터베이스의 핵심 `CRUD` 

#### `Create` : 데이터를 추가하는 것 
```sql
mysql> INSERT INTO topic (title,description,created,author,profile) VALUES('MySQL','MySQL is...',NOW(),'egoing','developer');

+----+------------+------------------+---------------------+--------+-----------+
| id | title      | description      | created             | author | profile   |
+----+------------+------------------+---------------------+--------+-----------+
|  1 | MySQL      | MySQL is...      | 2022-04-08 09:37:54 | egoing | developer |
|  2 | ORACLE     | ORACLE is...     | 2022-04-08 09:40:48 | egoing | developer |
|  3 | JAVA       | JAVA is...       | 2022-04-08 09:41:37 | egoing | developer |
|  4 | Python     | Python is...     | 2022-04-08 09:41:55 | egoing | developer |
|  5 | JavaScript | JavaScript is... | 2022-04-08 09:42:12 | egoing | developer |
+----+------------+------------------+---------------------+--------+-----------+

```

#### `Read` : 데이터를 읽어오는 것

```sql

mysql> SELECT * FROM topic; // a
mysql> SELECT id,title,created,author FROM topic; // b

+----+------------+---------------------+--------+
| id | title      | created             | author |
+----+------------+---------------------+--------+
|  1 | MySQL      | 2022-04-08 09:37:54 | egoing |
|  2 | ORACLE     | 2022-04-08 09:40:48 | egoing |
|  3 | JAVA       | 2022-04-08 09:41:37 | egoing |
|  4 | Python     | 2022-04-08 09:41:55 | egoing |
|  5 | JavaScript | 2022-04-08 09:42:12 | egoing |
+----+------------+---------------------+--------+

mysql> SELECT id,title,created,author FROM topic WHERE author='egoing'; // c

+----+------------+---------------------+--------+
| id | title      | created             | author |
+----+------------+---------------------+--------+
|  1 | MySQL      | 2022-04-08 09:37:54 | egoing |
|  2 | ORACLE     | 2022-04-08 09:40:48 | egoing |
|  3 | JAVA       | 2022-04-08 09:41:37 | egoing |
|  4 | Python     | 2022-04-08 09:41:55 | egoing |
|  5 | JavaScript | 2022-04-08 09:42:12 | egoing |
+----+------------+---------------------+--------+

mysql> SELECT id,title,created,author FROM topic WHERE author='egoing' ORDER BY id DESC; // d

+----+------------+---------------------+--------+
| id | title      | created             | author |
+----+------------+---------------------+--------+
|  5 | JavaScript | 2022-04-08 09:42:12 | egoing |
|  4 | Python     | 2022-04-08 09:41:55 | egoing |
|  3 | JAVA       | 2022-04-08 09:41:37 | egoing |
|  2 | ORACLE     | 2022-04-08 09:40:48 | egoing |
|  1 | MySQL      | 2022-04-08 09:37:54 | egoing |
+----+------------+---------------------+--------+

mysql> SELECT id,title,created,author FROM topic WHERE author='egoing' ORDER BY id DESC LIMIT 2; // e

+----+------------+---------------------+--------+
| id | title      | created             | author |
+----+------------+---------------------+--------+
|  5 | JavaScript | 2022-04-08 09:42:12 | egoing |
|  4 | Python     | 2022-04-08 09:41:55 | egoing |
+----+------------+---------------------+--------+

```

`a` 는 topic전체 출력   
`b` 는 topic에서 보고싶은 타이틀만 출력   
`c` 는 topic에서 보고싶은 데이터중 author의 egoing만 출력 (엑셀의 필터기능과 유사함)   
`d `는 topic에서 보고싶은 데이더중 author의 egoing만 출력해서 오름차순으로 정렬한다.   
`e` 는 topic에서 보고싶은 데이더중 author의 egoing만 출력해서 오름차순으로 정렬한 후, 2개의 데이터만 출력한다.   


#### `Update` : 데이터를 업데이트한다. 

```sql

mysql> UPDATE topic SET author='brian', title='JAVA' WHERE id=3;  //a
mysql> SELECT * FROM topic;
+----+------------+------------------+---------------------+--------+-----------+
| id | title      | description      | created             | author | profile   |
+----+------------+------------------+---------------------+--------+-----------+
|  1 | MySQL      | MySQL is...      | 2022-04-08 09:37:54 | egoing | developer |
|  2 | ORACLE     | ORACLE is...     | 2022-04-08 09:40:48 | egoing | developer |
|  3 | JAVA       | JAVA is...       | 2022-04-08 09:41:37 | brian  | developer |
|  4 | Python     | Python is...     | 2022-04-08 09:41:55 | egoing | developer |
|  5 | JavaScript | JavaScript is... | 2022-04-08 09:42:12 | egoing | developer |
+----+------------+------------------+---------------------+--------+-----------+

```

topic에서 id값 3인 author를 egoing에서 brian으로 변경한다. 위 구문에서 `WHERE`을 안써주면 조건에 만족하는 데이터값 모두가 변경될 수 있으니 조심해야한다. 
 

#### `Delete` : 데이터를 삭제한다. 

```sql

mysql> DELETE FROM topic WHERE id = 5;
mysql> SELECT * FROM topic;
+----+--------+--------------+---------------------+--------+-----------+
| id | title  | description  | created             | author | profile   |
+----+--------+--------------+---------------------+--------+-----------+
|  1 | MySQL  | MySQL is...  | 2022-04-08 09:37:54 | egoing | developer |
|  2 | ORACLE | ORACLE is... | 2022-04-08 09:40:48 | egoing | developer |
|  3 | JAVA   | JAVA is...   | 2022-04-08 09:41:37 | brian  | developer |
|  4 | Python | Python is... | 2022-04-08 09:41:55 | egoing | developer |
+----+--------+--------------+---------------------+--------+-----------+

```
topic에서 id값 5인 아이를 지운다.

<br>

## JOIN 알아보기 (두 개의 테이블 결합하기)

topic 테이블과 author 테이블의 id값으로 join해보자.

```sql

mysql> SELECT * FROM topic;
+----+--------+--------------+---------------------+-----------+
| id | title  | description  | created             | author_id |
+----+--------+--------------+---------------------+-----------+
|  1 | JAVA   | JAVA is...   | 2022-04-08 11:01:20 |         1 |
|  2 | Python | Python is... | 2022-04-08 11:01:34 |         1 |
|  3 | MySQL  | MySQL is...  | 2022-04-08 11:01:48 |         1 |
|  4 | JINI   | JINI is...   | 2022-04-08 11:02:03 |         3 |
|  5 | SIRI   | SIRI is...   | 2022-04-08 11:02:14 |         2 |
+----+--------+--------------+---------------------+-----------+
5 rows in set (0.000 sec)

mysql> SELECT * FROM author;
+----+-------+-----------+
| id | name  | profile   |
+----+-------+-----------+
|  1 | brian | developer |
|  2 | siri  | appleAI   |
|  3 | jini  | KTAI      |
+----+-------+-----------+
3 rows in set (0.000 sec)

```


아래와 같은 명령어들을 사용하면 두개의 테이블을 합쳐서 보여준다.   
아래 명령어는 mysql에게 topic.author_id와 author.id가 같은것 끼리 합쳐주고 모든 데이터 값을 출력하라는 명령이다.

```sql
mysql> SELECT * FROM topic LEFT JOIN author ON topic.author_id = author.id;
+----+--------+--------------+---------------------+-----------+------+-------+-----------+
| id | title  | description  | created             | author_id | id   | name  | profile   |
+----+--------+--------------+---------------------+-----------+------+-------+-----------+
|  1 | JAVA   | JAVA is...   | 2022-04-08 11:01:20 |         1 |    1 | brian | developer |
|  2 | Python | Python is... | 2022-04-08 11:01:34 |         1 |    1 | brian | developer |
|  3 | MySQL  | MySQL is...  | 2022-04-08 11:01:48 |         1 |    1 | brian | developer |
|  4 | JINI   | JINI is...   | 2022-04-08 11:02:03 |         3 |    3 | jini  | KTAI      |
|  5 | SIRI   | SIRI is...   | 2022-04-08 11:02:14 |         2 |    2 | siri  | appleAI   |
+----+--------+--------------+---------------------+-----------+------+-------+-----------+
5 rows in set (0.005 sec)
```

위에서 `SELECT * FROM` 이라는 명령어는 데이터값을 모두 출력하라는 문구이지만 원하는 값만 출력하려면 *값 대신 아래처럼 입력해주면 원하는 값만 나타낼 수 있다.

```sql
mysql> SELECT topic.id,title,description,created,name,profile FROM topic LEFT JOIN author ON topic.author_id = author.id;
+----+--------+--------------+---------------------+-------+-----------+
| id | title  | description  | created             | name  | profile   |
+----+--------+--------------+---------------------+-------+-----------+
|  1 | JAVA   | JAVA is...   | 2022-04-08 11:01:20 | brian | developer |
|  2 | Python | Python is... | 2022-04-08 11:01:34 | brian | developer |
|  3 | MySQL  | MySQL is...  | 2022-04-08 11:01:48 | brian | developer |
|  4 | JINI   | JINI is...   | 2022-04-08 11:02:03 | jini  | KTAI      |
|  5 | SIRI   | SIRI is...   | 2022-04-08 11:02:14 | siri  | appleAI   |
+----+--------+--------------+---------------------+-------+-----------+
5 rows in set (0.001 sec)

```



