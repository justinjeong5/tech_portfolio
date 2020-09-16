
## AI SUMMIT 2020, SEOUL

[AI Summit 2020 seoul](https://aisummit.co.kr/) COEX

<img src="https://user-images.githubusercontent.com/44011462/93159771-e3846780-f749-11ea-90af-ab196b8b512f.jpg" width=300px>
<img src="https://user-images.githubusercontent.com/44011462/93159843-13336f80-f74a-11ea-8a07-c7b64e4e58be.jpg" width=300px>

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

