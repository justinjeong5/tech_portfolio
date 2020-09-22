# SECURITY
- [SECURITY](#security)
  - [SQL Injection](#sql-injection)
    - [blind sql injection](#blind-sql-injection)
    - [sql injection 방어](#sql-injection-방어)
- [number Theory](#number-theory)
  - [primality test](#primality-test)
    - [trial division](#trial-division)
    - [fermat test](#fermat-test)
    - [Miller-Rabin test](#miller-rabin-test)
    - [deterministic algorithm](#deterministic-algorithm)
    - [byhrid](#byhrid)
  - [공개키 암호의 설계 기본 원리](#공개키-암호의-설계-기본-원리)
    - [Descrete Logarithm](#descrete-logarithm)
    - [integer factorization](#integer-factorization)
- [classical encryption](#classical-encryption)
  - [Symmetric Cipher Model](#symmetric-cipher-model)
  - [Substitution thechniques](#substitution-thechniques)
    - [Monoalphabetic ciphers](#monoalphabetic-ciphers)
    - [Polyalphabetic ciphers](#polyalphabetic-ciphers)
  - [Transposition thechniques](#transposition-thechniques)
  - [Rotor machines](#rotor-machines)
  - [Steganography](#steganography)
    - [상용 스테가노그래피(steganography)](#상용-스테가노그래피steganography)
    - [steganography 구현](#steganography-구현)
      - [개요](#개요)
      - [사용방법](#사용방법)
      - [실행결과](#실행결과)
      - [구현원리](#구현원리)
      - [소스코드](#소스코드)



## SQL Injection
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




# number Theory

## primality test

### trial division
가능한 모든 소수에 대해서 나누어 보는 방식이다. 단 p에 대해서는 <img src="https://user-images.githubusercontent.com/44011462/93845751-fca68e80-fcdc-11ea-8824-1901309d16f5.png" height=20px>만큼만 확인해보면 test가 가능하다.

### fermat test
일종의 확률적인 방법이다. p가 소수인지 아닌지 알기위해 GCD(a, p) = 1을 만족하는 a를 뽑는다. a에 대해서 <img src="https://user-images.githubusercontent.com/44011462/93845784-18119980-fcdd-11ea-8ac0-5d8fa60beead.png" height=20px>의 계산 결과가 1인지 아닌지를 확인하는 방식이며, 이를 만족하는 a를 하나라도 얻을 수 있다면 p는 소수가 아니다.

### Miller-Rabin test
기본적으로는 이 방식도 일종의 fermat test이다. 여기에 NSR test를 더하여 Miller-Rabin test가 된다. p가 만약에 홀수인 소수라면 <img src="https://user-images.githubusercontent.com/44011462/93845856-42635700-fcdd-11ea-96c2-dcca2811eeab.png" height=20px>을 만족하는 해는 x = 1, p - 1(<img src="https://user-images.githubusercontent.com/44011462/93845912-750d4f80-fcdd-11ea-95cd-f2a0c7bbc384.png" height=20px>)밖에 없다. 따라서 이 두수에 대해서 x = 1, p - 1이 아닌 해를 구할 수 있다면 p는 소수가 아니다. 
기본적으로 Miller-Rabin test는 fermat test를 기반으로 하기떄문에 확률적인 방식이다. 알고리즘이 확률적이라는 것은 아래와 같은 의미이다.
1. 항상 답은 얻지만, 입력에 따라 수행시간이 확률적으로 들쭉날쭉한 방식
2. 알고리즘의 수행시간은 항상 일정하지만, 입력에 따라 결과가 틀릴 수 있는 방식

Miller-Rabin test는 2번의 방식이다. 그렇다면 이런 방식을 믿고 사용할 수 있는가? 
그렇다. 2번의 방식을 어느정도 반복하면 오답의 확률이 매우 적어지기 떄문에 활용이 가능하다.

```javascript
// Miller-Rabin Algorithm

Miller_Rain(n, s)   // Test if n is prime with error probability << 2 ^-s
For j = 1 to s
    a = random positive integet < n
    if Test(a, n) = Composite, then return Composite.    // definitely
End For
Return Prime    // almost sure

Subroutine Test(a, n)
Let t and u mbe s.t. t >= 1, u is odd, and n - 1 = (s ^ t) * u.
x0 = a^u mod n.
For i = 1 to t
    xi = (xi-1)^2 mod n.
    if xi = 1 and xi-1 != 1 and xi-1 != n - 1, then return Composite.    // NSR test
End For
if xt != 1, then return Composite.   // Fermat test
Return Prime

// 출처: Cormen, Leiserson, Rivest, and Stein, Introduction ro ALgorithms, 3rd ed., MIT Press
```
위 방식에서 subroutine Test의 정답 확률은 50%정도로 알려져 있다. 따라서 s번을 반복한다면 Test의 오답 확률은 2^s정도로 작아진다. 보통 s정도는 100회정도로 하도록 권장되며 이는 약 1.2676506e+30이며 0에 근접한다. 

### deterministic algorithm
trial division방식은 상당히 비효율적이고 miller-rabin test는 정답을 보장할수는 없다. trial division보다 효율적이고 정답이 보장되는 방법을 찾으려는 시도는 항상 있었다. AKS algorithm이라는 방식이 제안되었지만 miller-rabin 방식보다 상당히 비효율적이다. 현재 2020년까지는 deterministic 하면서 miller-rabin보다 빠른방식은 알려지지 않았다. 

### byhrid 
실무에서는 trial division과 Miller-Rabin을 조합하여 사용한다. 100자리 이상의 거대한 숫자가 prime인지 확인하는 문제에서 2, 3, 5, 7, 11등의 아주 작은 prime까지만 trial division으로 확인해보고 이후로 MR방식으로 확인한다. trial 방식을 사용하면서 상당히 많은 후보군을 줄일 수 있으므로 MR의 확률이 매우 좋아진다.

## 공개키 암호의 설계 기본 원리
공개키 암호는 descrete logarithm과 integer factorization의 두가지 '풀이가 매우 어려운' 두가지 방식을 이용하여 설계된다. descrete logarithm은 어떤수 a를 p에 대해서 module연산을 수행할 때 a를 몇번 제곱해야 n이 나오는지 계산하는것이다. integer factorization은 어떤 수를 두개의 소수로 소인수분해를 하는 방식이다. 위 두가지 방식은 4096bits에서 8192bits의 소수에 대해 계산하는 것이 매우 어렵다는 점을 이용한다.
### Descrete Logarithm
<img src="https://user-images.githubusercontent.com/44011462/93839674-a92a4580-fcc8-11ea-8d68-1ac3a260a078.png" width=600px>

### integer factorization
<img src="https://user-images.githubusercontent.com/44011462/93840155-55206080-fcca-11ea-9770-c91273af67c1.png" width=600px>
 
공개키 암호에 대한 자세한 이야기는 다른곳에서 다루기로 한다.

# classical encryption
1950-60년대에 주로 사용되었던 고전 암호이다. 

## Symmetric Cipher Model
<img src="https://user-images.githubusercontent.com/44011462/93840586-df1cf900-fccb-11ea-8cb5-2b77ce605096.png" width=300px><br><img src="https://user-images.githubusercontent.com/44011462/93840804-9fa2dc80-fccc-11ea-9a01-738f400208b5.png" width=300px>  

암호화할떄의 key와 복호화할떄의 key가 같도록 하는 방식이다. 따라서 공격자는 cipher text를 갈취한다고 하여도 plain text를 알수 없다. 이런 암호화 방식은 표준화가 되어있어서 이루어지는 모든 방식을 누구라도 알수 있다. 따라서 사용되는 key가 안전해야한다. 

## Substitution thechniques
plain text를 구성하는 글자, 숫자, 기호가 다른것으로 대체되는 것을 말한다. 

### Monoalphabetic ciphers
이런 방식중 가장 고전적인 방식이 고대 로마시절에 율리우스 시저황제가 사용했던 방식인 Caesar Cipher이다. alphabet을 n칸씩 오른쪽으로 이동시키는 방식으로 가능성이 26가지로 규칙이 매우 단순한 것이 특징이다.   

<img src="https://user-images.githubusercontent.com/44011462/93841242-e7763380-fccd-11ea-98e9-775a913d1c46.png" width=300px>

다음은 Caesar 방식의 단순함을 어느정도 극복한 Permutation Cipher이다. 알파벳이 대응되어야 하는 부분을 n칸씩 정한것이 아닌, 모든 알파벳에 대하여 임의로 알바펫을 대응시키는 방식이다. 

<img src="https://user-images.githubusercontent.com/44011462/93841511-c6faa900-fcce-11ea-8106-feb4699d7c09.png" width=300px> 

26!개의 방식이 있기때문에 당시에는 일일이 해보기에는 어려운 방식이었다. 하지만 영어에는 자주 쓰이는 알파벳이 정해져있다. plain text에서 많이나오는 E와 같은 알파벳은 Cipher text에도 가장 많이 등장하는 알파벳이라는 것을 알 수 있다.

<img src="https://user-images.githubusercontent.com/44011462/93841683-53a56700-fccf-11ea-859e-75b398f5f060.png" width=300px>

게다가 영어에는 the, th, ph등등 주로 짝지어서 사용하는 몇몇 알파벳이 있다. 이런 조합을 사용하여 추측해보면 충분히 key가 없이도 해독이 가능하다는 한계가 있다.

### Polyalphabetic ciphers
위 방식이 한계를 갖는 점을 극복하자는 방식이다. 

## Transposition thechniques

## Rotor machines

## Steganography

**스테가노그래피(steganography)** 는 데이터를 은폐 기술로 그리스어로 감추어져 있다는 뜻인 *'stegano'* 와 쓰다, 그리다는 뜻의 *'graphos'* 의 합성어이다. 이는 사진, 음악, 동영상 등의 일반적인 파일 안에 데이터를 숨기는 기술이다. **크립토그래피(cryptography)** 는 데이터의 내용을 읽을 수 없도록 하는 기술이라면 **strganography**는 데이터의 존재 자체를 감추는 기술이다. 

<img src="https://user-images.githubusercontent.com/44011462/92374126-49656380-f13a-11ea-9cb5-82189412bc38.jpg" width=300px> <img src="https://user-images.githubusercontent.com/44011462/92374126-49656380-f13a-11ea-9cb5-82189412bc38.jpg" width=300px>  

*사람의 눈에는 서로 다르지 않은 동일한 파일로 인식된다.*

**steganography가 악용된 실제 사례**
* 미국의 911테러를 이르킨 빈라덴이 테러지령을 *steganography*로 전달한 것이 밝혀지며 유명한 정보 은닉 기술로 알려졌다.
* 한때 악망 높았던 *랜섬웨어(ransomware)* 가 *steganography*를 이용하였다. 이메일을 통해 *window script file(.wsf)* 형식의 악성 스크립트 파일을 첨부하고, 그 해당 *wsf*이 특정 URL로 접속하게하여 이미지 파일을 다운로드시켰다. 그 이미지에는 랜섬웨어 관련 파일들이 *zip*의 형태로 숨겨져 있었다.  
    <img src="https://user-images.githubusercontent.com/44011462/92375171-b5949700-f13b-11ea-9141-1ab9bb6f020a.png" width=300px>  

    *랜섬웨어에 감염된 화면*

* 배너 광고에 *malware*가 숨겨진 사례로, Internet Explorer와 adobe Flash player의 *보안 취약점*을 이용하여 악성 javascript code가 *steganography*를 이용하여 숨겨진 이미지가 배너에 사용되었다.   
    <img src="https://user-images.githubusercontent.com/44011462/92381313-900c8b00-f145-11ea-8fec-7ce8d48ff50e.png" width=300px>  
    
    *스테가노그래피 기법으로 JPG파일에 숨겨져 있는 랜섬웨어 압축 포멧*

### 상용 스테가노그래피(steganography)

[openStegano](https://www.openstego.com/index.html)
개발언어: Java([open source](https://github.com/syvaidya/openstego))
사용환경: Window, Linux

<img src="https://user-images.githubusercontent.com/44011462/92380767-aa923480-f144-11ea-8c41-5ccefa210478.png" width=300px> <img src="https://user-images.githubusercontent.com/44011462/92380858-cac1f380-f144-11ea-8ece-5e3a18f7aefe.png" width=300px>

<img src="https://user-images.githubusercontent.com/44011462/92380942-eb8a4900-f144-11ea-85e5-aa202fd67302.png" width=300px> <img src="https://user-images.githubusercontent.com/44011462/92380993-065cbd80-f145-11ea-9e14-9af3187d274d.png" width=300px>

### steganography 구현

#### 개요
개발환경: window 10, visual studio 2019 
사용언어: c++ 14
프로그램 설명: window bitmap file(.bmp) 형태의 파일에 원하는 message를 숨기고 열어볼 수 있는 프로그램

#### 사용방법
우선 숨기기를 원하는 최대 500자의 text와 bmp확장자를 가진 이미지가 필요하다.
이미지와 실행파일을 동일한 directory에 위치시키고 아래와 같은 명령을 수행한다. 
실행파일의 이름은 stego.exe로 이미지의 이름은 origin.bmp로 지정한다.  


<img src="https://user-images.githubusercontent.com/44011462/93299549-2e78aa80-f830-11ea-835a-9b77f37c3c89.png" width=300px>  <img src="https://user-images.githubusercontent.com/44011462/93299548-2e78aa80-f830-11ea-8b06-68c472185b89.png" width=300px>

*window command를 이용한 encoding과 decoding의 모습*


#### 실행결과

<img src="https://user-images.githubusercontent.com/44011462/92374126-49656380-f13a-11ea-9cb5-82189412bc38.jpg" width=300px> <img src="https://user-images.githubusercontent.com/44011462/93299536-2b7dba00-f830-11ea-9f6a-9e440eb4b009.png" width=300px>  


<img src="https://user-images.githubusercontent.com/44011462/92374126-49656380-f13a-11ea-9cb5-82189412bc38.jpg" width=300px> <img src="https://user-images.githubusercontent.com/44011462/93299550-2f114100-f830-11ea-8cff-a324be282500.png" width=300px>  

*origin.bmp와 stego.bmp의 모습*

#### 구현원리

24 비트맵 이미지의 경우, bitmap 이미지 데이터는 offset 0x36인 곳에서부터 blue, green, red 순서로 된 튜플들을 순차적으로 저장하고 있다. 각 색상의 크기는 1바이트. 따라서, 픽셀당 **3bytes**를 사용한다. 이 각 픽셀이 가진 정보는 *2^24(약 1600만개)* 가지로, 아주 작은 차이는 *사람의 눈으로는 구분이 불가능*하다.
이 점을 이용하여 3byte의 *마지막 비트를 steganography를 위한 공간으로 활용*한다. 따라서 숨길 수 있는 데이터의 크기는 비트맵 헤더파일이 담고 있는 정보중 offset 0x02에 있는 **BMP 파일 크기** 정보에서 헤더 파일의 전체 크기인 **54bytes**를 제외한 크기만큼 가능하다.  
**encoding**은 3bytes중에서 가장 마지막 비트마다 message의 binary정보를 순서대로 넣었다.
**decoding**은 encoding의 반대개념으로, BMP를 탐색하면서 3bytes의 마지막 비트를 차례대로 수집하였다. encoding시에는 다루고자하는 message의 길이르 알수 있었지만, decoding때는 길이를 알수 없어서 최대길이를 1000자로 제한하는 조건을 걸어두어 해결하였다.


**bitmap, BMP의 파일 format**
|offset(dec) |offset(hex)|크기 |목적 |
|:---:|:---:|:---:|---|
|0  |0x0 |2 |**BMP 파일을 식별**하는 데 쓰이는 매직 넘버: 0x42 0x4D (B와 M에 대한 ASCII 코드 포인트)|
|2	|0x2 |4	|**BMP 파일 크기** (바이트 단위)
|10	|0xA |4	|비트맵 데이터를 찾을 수 있는 **시작 오프셋** (바이트 단위)
|14	|0xE |4	|이 헤더의 크기 (40 바이트)
|18	|0x12 |4 |**비트맵 가로** (단위는 화소, signed integer).
|22	|0x16 |4 |**비트맵 세로** (단위는 화소, signed integer).
|54	|0x36 |- |**실제 데이터**


#### 소스코드

<details>
    <summary><span style="color:grey">클릭하여 소스보기</span></summary>

```c++
/*
 * 2020.9.15
 * 컴퓨터보안 project #1 - Steganography
 * 12164720 정경하 컴퓨터공학과
 */
#define _CRT_SECURE_NO_WARNINGS

#include <iostream>
#include <fstream>
#include <bitset>
#include <string>
#include <Windows.h>

using namespace std;

BITMAPFILEHEADER bf;
BITMAPINFOHEADER bi;

const int COLOR_SIZ = 3;
const int MAX_TXT_SIZ = 500;

unsigned char *bmp_img;

string txt;
string txt_in_bin;

void print_message(const string &msg);

void decode();

void read_file(const char *img);

void convert_bin_to_txt();

void encode();

void write_file();

void convert_txt_to_bin();

int main(int argc, char **argv) {

    if (argc != 2) {
        print_message("usage error! usage : mystego.exe e/d");
        exit(-1);
    }

    if (argv[1][0] != 'e' && argv[1][0] != 'd') {
        print_message("usage error! usage : mystego.exe e/d");
        exit(-1);
    }

    switch (argv[1][0]) {
        case 'e':
            encode();
            print_message("encoding success!!");
            break;

        case 'd':
            decode();
            print_message("decoding success!!");
            break;

        default:
            print_message("argument error! argument should be e or d");
    }
    cout << " ...program exit\n";
    return 0;
}


void print_message(const string &msg) {
    cout << msg << endl;
}

void decode() {
    //TODO: BMP파일에 숨겨진 messge를 읽음
    txt_in_bin = "";
    read_file("stego.bmp");

    for (int i = 0; i < bi.biSizeImage; i = i + COLOR_SIZ) {
        txt_in_bin.push_back(bmp_img[i]);
    }
    convert_bin_to_txt();
}

void convert_txt_to_bin() {
    //TODO: 사용자로부터 message를 입력받아 binary 코드로 변환
    cout << "insert message to Steganograph" << endl;
    getline(cin, txt);
    int txt_len = txt.size();
    if (txt_len > MAX_TXT_SIZ) {
        print_message("message is too long. (smaller than 1000 characters)");
        exit(-1);
    }

    for (int i = 0; i < txt_len; ++i) {
        string temp = bitset<8>(txt[i]).to_string();
        for (int j = 0; j < 8; ++j) {
            txt_in_bin.push_back(temp[j]);
        }
    }
}

void read_file(const char *img) {
// TODO: BMP형식의 파일을 읽어 메모리 heap영역에 저장함
    ifstream read_bmp;

    read_bmp.open(img, ios_base::in | ios_base::binary); //a.bmp파일을 바이트로 읽어 들인다..
    read_bmp.read((char *) &bf, sizeof(BITMAPFILEHEADER)); // 사이즈..

    FILE *fileDescriptor = fopen(img, "rb");
    if (fileDescriptor == NULL) {
        print_message("read_file error! cannot read image(.bmp) file");
        exit(-1);
    }

    fread(&bf, sizeof(unsigned char), sizeof(BITMAPFILEHEADER), fileDescriptor);// 사이중 헤더에 사이즈 14바이트..
    fread(&bi, sizeof(unsigned char), sizeof(BITMAPINFOHEADER), fileDescriptor);// 나머지 정보 40바이트 저장..

    bmp_img = new unsigned char[bi.biSizeImage];
    fread(bmp_img, sizeof(unsigned char), bi.biSizeImage, fileDescriptor);

    fclose(fileDescriptor);
}

void encode() {
    //TODO: BMP파일에 message를 숨김

    read_file("origin.bmp");
    convert_txt_to_bin();

    const int MAX_COUNT = txt_in_bin.size();
    int count = 0;
    for (int i = 0; i < bi.biSizeImage, count < MAX_COUNT; i = i + COLOR_SIZ) {
        //TODO: 그림에 binary로 처리한 message를 입력
        bmp_img[i] = txt_in_bin[count++];
    }

    write_file();
}

void write_file() {
// TODO: 파일을 BMP형식으로 내보냄

    FILE *fileDescriptor = fopen("stego.bmp", "wb");
    if (fileDescriptor == NULL) {
        print_message("write_file error! cannot write stego.bmp");
        exit(-1);
    }

    fwrite(&bf, sizeof(unsigned char), sizeof(BITMAPFILEHEADER), fileDescriptor);
    fwrite(&bi, sizeof(unsigned char), sizeof(BITMAPINFOHEADER), fileDescriptor);
    fwrite(bmp_img, sizeof(unsigned char), bi.biSizeImage, fileDescriptor);

    fclose(fileDescriptor);
    delete[] bmp_img;
}


void convert_bin_to_txt() {
// TODO: binary를 글자로 출력함

    int index = 0;
    for (int i = 0; i < bi.biSizeImage; ++i) {
        if (txt_in_bin[index] != '1' && txt_in_bin[index] != '0') break;
        bitset<8> temp(00000000);
        for (int j = 0; j < 8; ++j) {
            temp.set(7 - j, txt_in_bin[index++] == '1');
        }
        cout << (char) temp.to_ulong();
    }
    cout << endl;
}

```
</details>