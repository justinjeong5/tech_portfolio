## steganography

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


