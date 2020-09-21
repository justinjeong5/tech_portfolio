# SECURITY

## AI SUMMIT 2020, SEOUL

[AI Summit 2020 seoul](https://aisummit.co.kr/) COEX

<img src="https://user-images.githubusercontent.com/44011462/93159771-e3846780-f749-11ea-90af-ab196b8b512f.jpg" width=300px> <img src="https://user-images.githubusercontent.com/44011462/93159843-13336f80-f74a-11ea-8a07-c7b64e4e58be.jpg" width=300px>

일시 : 2020년 12월 9-11일  
장소 : Korea seoul, COEX (Grand Ballroom)  
대상 : 보안기업(안랩, 소만사, 이스트시큐리티 등), 국가기관(한국인터넷진흥원, 금융보안원 등), 제조업(현대자동차), 연구기관(대학 등)  
기획/주관 : AI Summit 사무국, DMK  
방식 : 온라인 스트리밍 (현장 등록 50명 선착순)  

### EPP(Endpoint Protection Platform)
주로 클라이언드와 같은 네트워크의 양 끝단에서 보호하는 기술을 일컷는다. *antivirus, data encryption, intrusion prevention*등을 모두 포함하여 말한다.

### EDR(Endpoint Detection and Response)
EPP가 조금더 보완된 형태를 EDR이라고 부르며, 대응방식을 포함한 개념이다. 이 대응방식에는 **Cyber Threat Intelligence(CTI)** 라고 불리는 방식이 있으며 여러 기관, 집단에서 수집한 공격정보를 한곳에 모아 정보를 공유하고 이를 대응에 활용하도록 하는 방식이다. 이때 CTI에 도움이되는 정보는 여러가지가 있으며 대표적으로 *IP Address, C&C server, IOC(Indecator Of Compromise)* 가 있다. 이런 정보를 CTI방식으로 모아 AI를 이용하여 정보를 분석하고 해석한다.

### AI for Malware Detection
AI를 이용한 *malware detection* , 악성 코드 자체를 탐지하는 것보다는 발견된 악성코드를 분류하고 해석하는데 더 효과적으로 알려져 있다. *Ransomware* 등의 일부 특정유형의 악성코드에서는 탐지하는 능력이 매우 뛰어나다. 코드상에서 악성코드임을 알아낼 수 있는 패턴이 존재하기 떄문이다. 하지만 보편적이지 않은 언어로 작성되었거나 packing이 되어있는 경우 등, *training data* 가 많지 않다면 탐지는 어려운 경우가 많다.

### AXI(Explainable AI) for Security
AI를 이용하여 악성코드를 분류하였다면 *deep learning* 모델을 신뢰할 수 있을까. 주로 이미지 처리 분야에서 많이 사용되는 방식으로 *Class Activation Model(CAM)* 이 있다. *deep learning* 의 결과가 실제 전문가(사람)이 보는 것과 비슷하게, '정보로써 의미 있는 영역을 탐지'하는 방식이다. 이런 비슷한 방식을 AI로 악성코드를 탐지하는데 사용되도록 하는 것이 **AXI**이다. AI가 전체 코드중에서 어떤 부분을 읽고 판단했는지를 보여주는 기능이 있다면 더 좋은 **AI for Malware Detection**로 동작할 수 있는 것이다.

### Security for AI
인공지능을 보호하기 위해서, 혹은 인공지능을 공격하기 위해서 사용되는 방식이다.
#### Evasion Attack(Adversarial Example)
이미 trained되어 있는 인공지능 모델의 정보를 이용하여 속이는 방식이다. 사람의 사진에 안경을 씌워 다른 사람으로 인식하게 하거나, 자율주행 자동차에게 표지판의 일부를 바꾸어 사고를 유발하게 하는 방식이다. 
#### Data Poisoning
Evasion Attack이 test단계에서 공격하는 것이라면 **Data Poisoning** 은 *training 단계* 에서 공격하는 기법이다. 대표적인 사례로 마이크로소프트의 Tay라고 불리는 *chatbot* 이다. 2016년 3월 23일에 마이크로소프트사에서 사람의 채팅을 분석하여 특정 연령대의 인물을 모방하도록 하는 서비스의 *chatbot* 이었는데, 최초 16시간의 training과정에서 **의도적으로 잘못된 data**를 입력한 사용자들의 '공격'에 의해 결과적으로 욕설과 인신공격을 일삼는 *chatbot* 이 되어 서비스가 종료된 사례이다.
#### AI Model Stealing
AI Model Stealing은 AI의 구동방식을 이용한 공격이다. 가정에 기가지니를 놓는다면, 동작방식은 회사의 자연어처리 서버를 오고가며 정보를 분석하는 방식이다. 이를 이용하여 의도적으로 임의의 상당히 많은 query를 요청하여 AI model 내부적으로 어떻게 동작하는지 유추하는 방식이다. 이를 **Model Inversion Attack** 이라고 부른다.

<img src="https://user-images.githubusercontent.com/44011462/93162227-3f9dba80-f74f-11ea-9f63-37feb3a0272f.png" width=400px>

자율주행 자동차가 정지 표지판을 보고도 멈추지 않도록 공격하는 시나리오를 작성한다면 아래와 같다.

1. Queries
    수와 종류가 매우 많은 도로교통 표지판의 사진을 보내어 Model inversion attack을 수행한다.
2. Extract Information
    query의 요청과 응답을 비교하여 모델이 가진 class를 유추한다.
3. A proxy of the target AI model
    모델의 class를 보고 취약점(class의 경계가 모호하거나 근접한 부분)을 알아낸다
4. Create Adversarial Examples
    Model이 stop 표지판을 무시하거나 출발하는 신호로 인식할 수 있는 Adversarial Example를 만든다
5. 자율 주행 자동차는 멈추는 신호를 인식하지 못하고 교통사고가 일어난다.



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


## STEGANOGRAPHY

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

### 상용 스테가노그래피(steganopraphy)


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
