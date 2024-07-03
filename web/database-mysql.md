---
description: '부재 : phpMyAdmin과 sql사용법'
---

# Database (MySql)

DB는 데이터를 관리하는 곳으로. 엑셀 프로그램과 유사하다.

&#x20;

### \[구조]

엑셀과 비교하면 구조는 다음과 같다.

&#x20;

Database : 엑셀 파일

Table : 엑셀 시트

Column : 열, 데이터 종류, 카테고리, 세로 데이터

&#x20;   예시) 순번, 이름, 점수 등등

Row : 행, 가로데이터

<figure><img src="https://blog.kakaocdn.net/dn/OP9wg/btsAaV3YEGA/vABmOOfIOLpbeD91O6vfzk/img.png" alt=""><figcaption></figcaption></figure>

&#x20;&#x20;

&#x20;

### \[phpMyAdmin]

&#x20;

mySql이라는 DB를 웹페이지에서 관리할 수 있게한 시스템

&#x20;

로그인 시 DB의 계정으로 접속하면 된다.

예) admin / student1234

&#x20;

<mark style="color:red;">DB생성하기</mark> : **새로운** (선택)

<mark style="color:red;">새 테이블 만들기</mark> : 다음 예시와 같이 설정한다.

&#x20;

이름, 종류, 길이(값)

idx int(숫자) 10 >> 인덱스 : 프라이머리 AI(오토인크리먼트, 자동 늘려주겠다) 체크\
name varchar(가변적인 문자) 20\
score varchar 20\
pass varchar 20

&#x20;

<mark style="color:red;">데이터 넣기</mark> : 삽입

idx는 알아서 숫자가 늘어나니 비워둔다. (입력하지 않아도 된다.)

name, score, pass 등에는 데이터를 입력한다.

&#x20;

***

&#x20;

&#x20;

### \[SQL]

&#x20;

SQL은 WAS가 DB랑 대화하는 언어다.

&#x20;

위와같은 GUI 말고, 코드로 대화하는 방식이라 몇 가지 명령어를 알아야 한다.

&#x20;

#### 1. select

: 데이터를 가져온다.

&#x20;

select \[칼럼 이름] from \[테이블 이름]

의 방식으로 사용한다.

&#x20;

예시) select name,pass from test\_table

이렇게 치면 결과값이 나온다.

&#x20;

#### 2. insert

: 데이터를 넣는 명령어

회원가입이나 게시판에 글 작성 등 저장할 때 사용한다.

&#x20;

insert into \[테이블 이름] (컬럼 이름) value (값)

의 방식으로 사용한다.

&#x20;

예시) insert into test\_table (name, score, pass) value ('doldol', '80', '2222')

혹은 컬럼 이름 없이 다음과 같이 바로 명령할 수도 있다.

insert into test\_table (NULL, 'dalgdol', '70', '3333')

&#x20;

#### 3. 정교하게 select 하기 : <mark style="color:red;">where</mark>

구체적으로 내가 원하는 데이터를 가져오기 위해서 사용한다.

&#x20;

사용방법은 select 맨 마지막에 조건을 붙여주면 된다.

&#x20;

예시) select <mark style="color:red;">Column</mark> from <mark style="color:red;">\[테이블 이름]</mark> where <mark style="color:red;">\[조건]</mark>

조건이 name='pooh' 라면

select name from test\_table <mark style="background-color:orange;">where</mark> name='pooh'

&#x20;

이름과 비밀번호를 가져오고 싶은데, 이름이 pooh인 것만 찾을 때는

select name,pass from test\_table <mark style="background-color:orange;">where</mark> name='pooh'

&#x20;

조건을 다음과 같이 여러개를 쓸 수도 있다.

... <mark style="background-color:orange;">where</mark> name='pooh' <mark style="color:red;">and</mark> pass='1234'

&#x20;

참고로 and는 앞과 뒤를 동시에 만족시켜야 작동한다.

반면 or은 앞이나 뒤 중 하나만 맞아도 데이터를 가져온다.

&#x20;

***

&#x20;

### \[php - mysql 연동하기]

&#x20;

<mark style="color:red;">WAS가 DB와 통신하기 위해선, DB의 ID, PW를 WAS가 알고 있어야 한다.</mark>

<mark style="color:red;">그리고 SQL언어로 말을 걸어줘야 한다.</mark>

php 코드에 이를 적어준다.

&#x20;

로그인에 성공하게 되면, 출입이 가능한 티켓을 받게된다. (커넥터)

sql 언어를 사용할 때는 이 티켓을 사용하여 질문한다.

&#x20;

티켓을 잘 받았는지 체크한다.

```
<?php
	define('DB_SERVER', 'localhost');
	define('DB_USERNAME', 'admin');
	define('DB_PASSWORD', 'student1234');
	define('DB_NAME', 'test');

	$db_conn = mysqli_connect(DB_SERVER, DB_USERNAME, DB_PASSWORD, DB_NAME);

	if($db_conn){
		echo "DB Connect OK";
	}else{
		echo "DB Connect Fail"
	}
?>
```

&#x20;

$db\_conn = 변수에 넣어준다. 그래야 티켓을 버리지 않고 계속 쓸 수 있다.

&#x20;

WAS는 DB의 아이디와, 비밀번호가 있을 수 밖에 없다. (DB와 통신하기 위하여)

따라서 <mark style="background-color:purple;">웹서버의 shell이 털렸다는건 DB도 같이 털렸다는 것을 의미</mark>한다.

&#x20;

반대로 <mark style="background-color:purple;">해커 입장에서 web shell을 따면 가장 먼저 해야할 것은 DB추출</mark>이다.

&#x20;

&#x20;

### \[select 하는 방법]

&#x20;

우리가 사용할 sql을 선언해준다.

$sql = "select \* from test\_table";

result = mysqli\_query($db\_conn, $sql);

&#x20;

<mark style="color:red;">Vardump</mark>로 result를 호출하면 다음과 같은 선물 꾸러미를 준다.

<figure><img src="https://blog.kakaocdn.net/dn/bDPNpP/btsAaiSEHv9/Z1dQq1HG4gpnjIBTPAbMy0/img.png" alt="" height="225" width="418"><figcaption></figcaption></figure>

&#x20;&#x20;

위 결과를 row에 담고, fetch를 통해 데이터를 꺼낸다. (fetch는 맨 위에 순서부터 하나씩 꺼내준다.)

&#x20;

$row = mysqli\_fetch\_array($result);

&#x20;

fetch는 맨상위부터 순서대로 한 행씩 가져온다.

&#x20;

<mark style="color:red;">row에서 이름만 가져오는 방법</mark>은 다음과 같이 한다.

echo "Name :" . $row\['name'];

&#x20;

<mark style="color:red;">돌돌의 비번</mark>은 다음과 같이 가져온다.

$sql = "select \* from test\_table where name='doldol'";\
\~\~\
echo "Pass :" . $row\['pass'];

&#x20;

<mark style="color:red;">DB에 저장된 비밀번호</mark>를 가져오는 방법

$db\_pass = $row\['pass'];

&#x20;

if ($\_POST\['userpass'] == $db\_pass){

&#x20;   // login OK

}

&#x20;

&#x20;

\[]요 딱딱이 괄호는,

오브젝트, 선물꾸러미, 객체, 사전형타입의 배열, 구조화되어있는 데이터 패키지를 다 담을 수 있다.

따라서 컬럼 정보를 가져오려면 컬럼 이름을 쓰는 것이다.

&#x20;

$row\['pass'];

이런 방식으로!
