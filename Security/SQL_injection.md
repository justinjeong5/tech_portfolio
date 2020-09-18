## WEB_HACKING SQL Injection
SQL injection은 응용 프로그램 보안상의 취약점을 이용하여 개발자가 의도하지 않은 sql을 실행하여 데이터베이스를 비정상적으로 조작하는 code injection attack 방식이다.

**일반적인 웹앱의 호출방법**은 아래와 같다.

    http://www.abc.com/home.php?id=admin&password=1234

**일반적인 mysql을 다루는 php 파일구조**는 아래와 같다.

```php
<?php
  include("./lib/mysql-lib.php");
  mysql_open();
  $query_results = mysql_query("SELECT * FROM usr 
                                WHERE id = `".$_GET['id]"`
                                AND pw = `"$_GET['password']"`");
  $USER_INFO = mysql_fetch_array($query_results);
  ...
?>
```

**정상적인 query**
id가 admin이고 password가 1234인 경우, 아래와 같이 query가 사용되고 query가 참일 경우 로그인에 성공한다.

```sql
SELECT * FROM usr WHERE id = 'admin' AND pw = '1234';
```

**sql injection을 위한 웹앱 호출의 예시**
    
    http://www.abc.com/home.php?id=admin%27%20OR%201%3D1%20--&password=d0be2cd421

**sql injection의 결과 실제 호출외는 sql query**
```sql
SELECT * FROM usr WHERE id = 'admin' OR 1=1 -- AND pw = 'd0be2cd421';
```
'--'sms MySql의 주석(comment)기호이다. 따라서 뒤에 이어지는 Sql query를 무효화한다.
위의 결과는 언제나 참이므로 admin 계정으로 로그인 하게 된다.

**하지만** 일반적인 경우 서버의 소스코드는 공개되지 않기 때문에 해커의 감각이나, 내부구조를 파악할 수 있도록 의도된 공격을 통해 sql injection이 가능하도록 알아내야한다

### blind sql injection
공격하려는 웹앱이 sql injection에 취약하지만 데이터베이스 메세지가 공격자에게 보이지 않을 때 사용하는 공격방식이다. query에 대한  true/false의 응답을 통해 sql injection이 가능하도록 추측한다.

**blind sql injection 예시**

1. 취약점 확인 절차
    ```sql
    SELECT * FROM usr WHERE id = 1 and 1=1 #;
    SELECT * FROM usr WHERE id = 1 and 1=2 #;
    ```
    만약 위 2개의 query결과로 true, false를 얻었다면 위 웹앱은 blind sql injection에 대해 취약점이 있다고 판단 할 수 있다.  

2. 공격 세부절차 1
    ```sql
    SELECT * FROM usr WHERE id = 1 and length(pw)=? #;
    ```
    위 공격의 결과로 얻는 숫자값이 id가 1인 유저의 비밀번호의 길이이다.  

3. 공격 세부절차2
    ```sql
    SELECT * FROM usr WHERE id = 1 and substr(pw,0,1)='a' #;
    ```
    위 공격의 결과가 true이면 비밀번호의 첫번째 글자는 a라는 것을 알 수 있다.  

4. 공격 세부절차3
    모든 경우의 수에 대해 비밀번호를 확인해보면 id가 1인 유저의 비밀번호를 획득할 수 있다.

### sql injection 방어

1. 변수확인
    sql injection을 방어하는 기본적인 방법은 **변수확인**이다. *blind sql injection 공격*을 위한 sql query는 일반적인 query보다 **비정상적으로 긴 경우가 대부분**이다. 변수의 길이를 확인하여 유효성을 체크한다면 상당부분의 sql injection공격을 무력화 하는 것이 가능하다. php의 경우 *mysql_real_escape_string()* 과 같은 함수를 이용할 수 있다.

2. 필터링
    sql query에 사용되는 문자 및 단어들을 전부 **필터링**한다. 공백문자, '#', '=', 'and', 'or', '--'등의 sql **예약어를 사용하지 못하게** 한다.
    1. 필터링을 회파하는 방법 1
        주석기호(/**/)를 이용하여 공백을 제거하는 필터링을 회피한다. sql에 입력되기전에 처리하는 서버언어에서 주석기호를 공백으로 바꾸기 때문에 가능하다.
        ```sql
        /**/UNION/**/SELECT/**/*/**/FROM/**/usr--
        ```
    2. 필터링을 회파하는 방법 2
        char() 함수를 이용하여 문자를 입력하는 방식이다.
        ```sql
        UNION SELECT*(load_file(char(47,112,97,115,115,119,100)))--
        ```
    3. 필터링을 회파하는 방법 3
        16진수 표현을 이용하여 문자를 입력하는 방식이다.
        ```sql
        UNION SELECT*(load_file(0x6333a2f626f6f742e696e69))--
        ```  

3. 비노출 에러메세지
    sql query에 대한 응답의 결과만으로도 상당히 **민감한 정보**가 된다는 것을 알았다. 이런 정보를 사용자에게 **보여주지 않는** 방법이다.  

4. 최소 권한 유저로 데이터베이스 운영
    혹여나 공격자가 sql injection에 성공하더라도 다른 table이나 다른 database에 접근하지 못하도록 **권한을 최소한** 으로 유지해야한다.  

5. 신뢰할 수 있는 네트워크, 서버에 대한 접근 허용
    특정 기관 또는 회사등에서 사용하는 경우라면 **외부의 접근** 자체를 막을 수도 있다. 이는 **공격자가 외부**에서 접근하려는 경우에 사용가능하다.   

