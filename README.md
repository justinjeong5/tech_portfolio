**컴공 주요 5과목에 대한 주요 개념 정리**

- [FRONT-END](#front-end)
- [ALGORITHM](#algorithm)
- [DATABASE](#database)
  - [Transaction](#transaction)
    - [ACID](#acid)
      - [Atomicity](#atomicity)
      - [Consistency](#consistency)
      - [Isolation](#isolation)
      - [Durability - Persistency](#durability---persistency)
    - [transaction의 장단점](#transaction의-장단점)
      - [Advantages](#advantages)
      - [Disadvantages](#disadvantages)
    - [COMMIT과 ROLLBACK](#commit과-rollback)
- [OPERATING SYSTEM](#operating-system)
- [NETWORK](#network)
  - [UDP (User Datagram Protocol), TCP (Transmission Control Protocol)](#udp-user-datagram-protocol-tcp-transmission-control-protocol)
    - [다중화(Multiplexing)와 역다중화(Demultiplexing)](#다중화multiplexing와-역다중화demultiplexing)
      - [비연결형 다중화](#비연결형-다중화)
      - [연결형 다중화](#연결형-다중화)
      - [web-server와 TCP](#web-server와-tcp)
    - [UDP](#udp)
      - [UDP가 TCP보다 좋은점](#udp가-tcp보다-좋은점)
      - [UDP segment 구조](#udp-segment-구조)
      - [UDP check-sum](#udp-check-sum)
    - [신뢰적인 데이터 전달(reliable data transfer)의 원리](#신뢰적인-데이터-전달reliable-data-transfer의-원리)
    - [TCP](#tcp)
      - [TCP segment 구조](#tcp-segment-구조)
      - [흐름 제어 (Flow Control)](#흐름-제어-flow-control)
      - [TCP 연결 관리](#tcp-연결-관리)
      - [혼잡 제어 (Congestion Control)](#혼잡-제어-congestion-control)
- [COMPUTER ARCHITECTURE](#computer-architecture)


# FRONT-END

# ALGORITHM


# DATABASE

## Transaction

transaction은 한 단위로 간주되어야 하는 작업의 단위를 말한다. 주로 은행거래의 예시를 들어 설명하는데, 만약 고객1이 고객2에게 100만원을 전송하려는 시도를 한다고 가정하자. 고객1의 계좌에서 잔액이 100만원이 줄어들면서 고객2의 계좌에서는 잔액이 100만원이 올라야한다. 어떤 특정한 이유에서 고객1의 잔액을 줄었지만 고객2의 잔고가 늘지 않는다면 이는 큰 문제가 된다. 이는 마치 고객1에서 빠져나간 돈이 아무에게도 전해지지 않고 사라지는 것과 같은 상황이다. 위 예시보다 합리적이라면 실제로 100만원이 이체되거나 혹은 아무일도 일어나지 않아야 한다. 정보를 생성하고 참조하며 수정하거나 지우는 역할을 수행하는 Database에서도 이와 동일한 이슈가 있다. 

### ACID

#### Atomicity
atomicity는 여러가지 논리적으로 의미있는 단위의 일을 하나로 묶어서 반드시 한번에 일어나거나 전혀 일어나지 않아야 하는 일의 속성을 말한다. 어떤 행위에 atomicity가 있다고 하면 더이상 쪼개어질수 없다는 것과 같은 의미이다. 이는 database뿐 아니라 operating system의 critical section에서도 적용되는 개념이다. atomicity가 보장되려면 다음과 같은 일이 하나의 작업으로 보장되어야 한다.
1. 고객1이 10만원을 출금하려고 한다.
2. database에서 잔고를 확인하고 10만원을 뺸다
3. 고객1에게 10만원을 지급한다.

위 모든 과정이 마치 하나의 일로 발생해야하고, 중간에 어떤 특정한 이유로 모두 마무리 지을 수 없다면 아주 없던일로 해야한다. 

#### Consistency
data는 constraint와 rule이 있어서 의미가 발생한다. 통장계좌는 항상 0와 같거나 큰 정수라고 정해져 있어야만 있지도 않은 돈을 주는 일을 막을 수 있다. 따라서 transaction간에 해당 data가 rule과 constraint를 지키도록 보장하는 것을 말한다. 만약 고객1이 있지도 않은 금액을 출금하려고 한다면 해당 trasaction은 중단되어야햔다.  
1. 고객1의 잔고는 100만원이다.
2. 고객1은 200만원을 출금하려고 시도한다.
3. 2번은 1번에대한 consistancy를 위반한다.
4. 해당 transaction은 중단된다


#### Isolation
isolation은 transaction의 수행이 다른 transaction의 영향을 받지 않아야 한다는 것을 의미한다. 만약 고객1과 고객2가 동일한 계좌에서 금액을 출금하려고 한다고 하자. 계좌에는 100만원이 들어있고 고객1과 고객2는 각각 10만원, 25만원을 출금하려는 상황이다. 다음과 같은 일이 순서대로 일어났다고 가정하자. 
1. 고객1이 10만원을 출금한다.
2. 계좌에서 10만원이 줄어들고 이 내용이 database에 반영되려고 한다.
3. 2의 내용이 반영되기 전에 고객2가 25만원을 출금하려 시도했다.
4. 고객2가 25만월 지급받았다.
5. 고객2가 출금한 25만큼 database에서 잔액이 줄어들었다.
6. 2에서 반영된 내용이 이제서야 database에 반영된다.
7. 고객1과 고객2가 모두 출금하였고 은행 잔고는 90만원이다.

고객1이 10만원을 출금하는 transaction이 isolation을 보장받지 못한다면 고객2가 25만원을 출금하더라도 잔액이 여전히 90만원일 수 있다. 이런 일이 일어나지 않도록 보장하는 속성이 ioslation이다. 

#### Durability - Persistency
persistency로도 불리는 durability는 문제없이 수행된 transaction에 대해서는 영원하게 자료의 내용이 보장되어야 한다는 의미이다. 특정 시스템의 문제가 발생하더라도 자료의 내용이 유지되어야 한다. 따라서 영구적으로 저장이 가능한 매체에 기록되어 저장되어야한다. 소프트웨어적으로도 이를 보장해야한다.

### transaction의 장단점
#### Advantages
1. 동시에 여러 사용자가 computer resource를 공유할 수 있다
2. computing resource가 이미 점유중이라면 job processing의 순서를 조정하여 유연하게 사용할 수 있다.
3. 사람의 개입 없이도 작업이 가능하여 computer resource를 효율적으로 사용할 수 있다.
#### Disadvantages
1. 비용이 상대적으로 비싸다
2. 하드웨어와 소프트웨어가 호환성이 매우 떨어진다

_references_

[What is a database transaction? (stackoverflow)](#https://stackoverflow.com/questions/974596/what-is-a-database-transaction)  
[What is a Transaction? (microsoft)](#https://docs.microsoft.com/ko-kr/windows/win32/ktm/what-is-a-transaction?redirectedfrom=MSDN)  
[Transaction processing (wikipidea)](#https://en.wikipedia.org/wiki/Transaction_processing#ACID_criteria)  

### COMMIT과 ROLLBACK

Transaction의 구분은 commit을 이용하여 가능하다. commit은 모든 일의 과정이 정상적으로 진행되었으므로 모든 사항을 database에 반영하라는 의미이다. 이와 대응되는 개념은 rollback으로 data처리 과정에서 하나의 transaction을 마무리 할 수 없으므로, transaction을 되돌려 없던 일로 만들겠다는 의미이다. 이 commit과 rollback은 transaction이 가능하게 해주는 명령어이다. 비교하자면 문서작업을 할때 ctrl+z를 눌러서 일을 되돌리는 것을 rollback에, 문서작업이 어느정도 진행되면 ctrl+s를 눌러 저장하는 것을 commit에 비유할 수 있다. 다만 둘간에 매우 큰 차이가 있다면 ctrl+s를 누르면 더이상 ctrl+z를 사용하지 못하게 된다는 점이 다르다. 
commit을 하게되면 rollback으로 정보를 되돌릴 수 없다. 

# OPERATING SYSTEM

# NETWORK
## UDP (User Datagram Protocol), TCP (Transmission Control Protocol)

<img width="648" alt="Screen Shot 2020-07-08 at 1 03 02 PM" src="https://user-images.githubusercontent.com/44011462/86874488-6fab5a80-c11b-11ea-86ef-186410754bff.png">
 
<details>
    <summary><span style="color:grey">클릭하여 출처보기</span></summary>
Computer Networking: A Top-Down Approach, Global Edition  <br>
Publisher: Pearson Higher Education; 7th edition (November 21, 2016)  <br>
ISBN-10: 1292153598  <br>
ISBN-13: 978-1292153599  <br>
173Page, 그림3-1 트랜스포트 계층은 애플리케이션간의 물리적 통신보다는 논리적 통신을 제공함 <br> 
</details>

### 다중화(Multiplexing)와 역다중화(Demultiplexing)
Transport layer는 전달받은 segment의 data를 application layer의 어떤 socket에 넘겨줄지 확인하기 위해서 segment의 field를 검사하는 것을 말한다. 그리고 이 segment를 해당 socket으로 보낸다. 이를 **Multiplexing**이라고 한다. 반대로 출발지 host에서 socket으로부터 data를 모으고 이에 대한 segment를 생성하기 위해서 각 데이터에 header정보로 캡슐화하고 그 segment를 network layer로 보내는 것을 **Demultiplexing**이라고 한다.

#### 비연결형 다중화

    clientSocket = socket(AF_INET, SOCK_DGRAM);

위와 같은 선언을 통해 비연결형(UDP) socket을 생성할 수 있다. 이 방법으로 socket을 생성할 때 transport layer는 자동으로 port번호를 socket에 할당한다. 하지만 직접 원하는 port 번호를 socket에 할당할 수 있다.

    clientSocket.bind('', 19157);

일반적으로 application의 client는 자동으로 port번호를 할당 받는 것에 반하여, application의 server측은 특정 번호를 할당한다. 이 포트정보를 가지고 segment를 생성하고 application layer에서 multiplexing, demultiplexing 과정을 거쳐 data가 보내지고 되돌아온다. 

<img width="516" alt="Screen Shot 2020-07-08 at 1 43 32 PM" src="https://user-images.githubusercontent.com/44011462/86877082-0f1f1c00-c121-11ea-8568-9f9fb3de8a54.png">
<details>
    <summary><span style="color:grey">클릭하여 출처보기</span></summary>
Computer Networking: A Top-Down Approach, Global Edition  <br>
Publisher: Pearson Higher Education; 7th edition (November 21, 2016)  <br>
ISBN-10: 1292153598  <br>
ISBN-13: 978-1292153599  <br>
180Page, 그림3-4 출발지와 목적지 포트 번호의 전환  <br> 
</details>

UPD segment에는 목적지의 IP주소와 port번호가 있어서 대상지를 특정할 수 있다. 출발지가 다르더라도 목적지의 주소를 가지고 통신할 수 있다. 이점이 TCP와 다른점이며 이 차이로 인해 여러가지 논리적인 기능의 차이가 생긴다.

#### 연결형 다중화

TCP segment는 목적지 IP주소, 목적지 port번호, 출발지의 IP주소, 출발지 port번호에 의해서 식별된다. host에 TCP segment가 도착하면 위 4가지의 정보를 가지고 multiplexing, demultiplexing을 한다. 

TCP server application은 12000번의 주소를 갖는 welcoming socket을 가지고 있다. 

    clientSocket = socket(AF_INET, SOCK_STREAM);
    clientSocket.connect((serverName, 12000));

server는 host의 연결 요청을 기다리고 있다가 요청을 받으면 앞서 받은 목적지 IP주소, 목적지 port주소, 출발지 IP주소, 출발지 port가 일치하면 segment는 이 socket으로 demultiplexing된다. 

    connectionSocket, addr = serverSocket.accept();

이제 host와 servet는 적절히 통신할 수 있다.

#### web-server와 TCP

<img width="587" alt="Screen Shot 2020-07-08 at 2 07 00 PM" src="https://user-images.githubusercontent.com/44011462/86878623-5064fb00-c124-11ea-87c2-6e71c56caf3e.png">

<details>
    <summary><span style="color:grey">클릭하여 출처보기</span></summary>
Computer Networking: A Top-Down Approach, Global Edition  <br>
Publisher: Pearson Higher Education; 7th edition (November 21, 2016)  <br>
ISBN-10: 1292153598  <br>
ISBN-13: 978-1292153599  <br>
183Page, 그림3-5 같은 웹 서버애플리케이션과 통신하기 위해 같은 목적지 포트 번호(80)를 이용하는 두 클라이언트  <br> 
</details>

apache tomcat과 같은 web server는 port 번호 80에서 동작한다. 위 그림은 web server가 각각의 연결에 따라서 새로운 process를 fork하는 것을 보여준다. 이 연결을 통해서 HTTP 요청을 수신하고 HTTP 응답을 전송한다. 그러나 socket과 process가 모두 일대일 대응 관계를 갖는 것은 아니다. 실제로 고성능 웹 서비스들은 하나의 process를 사용하면서 여러개의 **thread**를 생성하여 서비스에 응답한다. 만약 client와 server가 **지속적인(persistent)** HTTP를 사용한다면 해당 기간동안 동일한 socket을 이용하여 통신한다. 이에 반하여 비지속적인(non-persistent) HTTP를 사용한다면 모든 응답/요청마다 새로운 TCP연결이 생성되고 종료되며 이는 overhead에 따른 성능상의 큰 문제가 된다. 

### UDP 

UDP는 transport layer protocol이 할수 있는 **최소 기능**으로 동작한다. UDP는 multiplexing, demultiplexing등의 기능과 간단한 오류검사를 제외하면 IP에 아무것도 추가하지 않는다. 사실 application 개발자가 TCP대신 UDP를 채택하여 개발한다면 application은 거의 IP와 직접 통신하는 셈이다.
UPD는 segment를 송신하기 저에 transport layer사이에서 handshake를 사용하지 않는다. 이런 이유로 UPD를 **비연결형(connectionless)** 이라고 부른다.

**DNS**는 일반적으로 UDP를 사용하는 application layer의 protocol이다. host에서 DNS application query를 생성할 때, DNS query 메세지를 작성하고 UDP에 넘겨준다. 목적지 종단 시스템에서 동작하는 UDP와 host는 어떠한 handshake도 수행 하지 않고 메세지에 header를 추가한 후에 segment를 network layer에 넘겨준다. 이때 query를 보낸 DNS application은 query에 대한 응답을 기다린다. 만약 host가 응답을 수신하지 못하면 query를 다른 Name server로 송신하거나 application에게 응답을 수신할 수 없다는 것을 알려준다.

#### UDP가 TCP보다 좋은점
TCP는 신뢰적인 데이터 전송 (reliable data transfer) 서비스를 제공하지만, UDP는 그렇지 않으므로 항상 TCP가 더 좋을까? 답은 '**아니오**'이다. 다음과 같은 이유에서 UDP를 사용하는 것이 더 적합하다.

**실시간성이 중요한 application**  
UDP에서는 application process가 data를 UPD에게 전달하면 UDP는 data를 UDP segment로 만들어서 바로 network layer로 전송한다. 이에 반하여 TCP는 congestion control을 실시하여 목적지 host와 출발지 host 사이가 과도하게 혼잡하면 전송속도를 조절한다. 또한 TCP는 신뢰적인 전달을 중요시 하므로 응답시간의 빠르기와 상관없이 목적지 host가 segment의 수신여부를 확인할때까지 기다리거나 재송신한다. UDP를 사용하면 조금의 data loss는 발생할 수 있지만, application 개발자가 일부 data loss는 허용할 수 있으므로 TCP는 위의 조건에 맞지 않는다.

**연결 설정이 없다**  
TCP는 data 전송을 시작하기전에 3-way handshake를 사용한다. 반면에 UDP는 형식적인 예비동작없이 data를 전송한다. 그러므로 UDP는 연결을 설정하기 위한 어떠한 지연도 없다. 이 이유로 DNS가 TCP가 아닌 UDP를 사용한다. DNS가 TCP를 사용한다면 많이 느려진다. 하지만 HTTP로 작성된 웹 페이지는 신뢰성이 중요하기 때문에 보통은 TCP로 동작한다. 

**연결 상태가 없다**  
TCP는 종단 시스템에서 연결 상태를 유지한다. 이 연결상태는 수신버퍼, 송신버퍼, 혼잡제어의 변수, 순서번호와 응답확인번호 등등의 정보를 포함한다. 따라서 동일한 조건이라면 TCP보다 UDP가 조금더 많은 client를 보유할 수 있다.

**작은 packet header overhead**  
TCP가 segment마다 20bytes의 header overhead를 갖는 반면에 UDP는 단지 8bytes의 header overhead를 갖는다.

#### UDP segment 구조

<img width="213" alt="Screen Shot 2020-07-09 at 6 05 17 PM" src="https://user-images.githubusercontent.com/44011462/87020400-c34aa080-c20e-11ea-8e21-42dccc9db59f.png">

<details>
    <summary><span style="color:grey">클릭하여 출처보기</span></summary>
Computer Networking: A Top-Down Approach, Global Edition  <br>
Publisher: Pearson Higher Education; 7th edition (November 21, 2016)  <br>
ISBN-10: 1292153598  <br>
ISBN-13: 978-1292153599  <br>
187Page, 그림3-7 UDP 세그먼트 구조<br> 
</details>

#### UDP check-sum

UDP check-sum은 error detection을 제공한다. segment가 출발지에서 목적지로 이동핸 후 UDP segment의 정보가 손실 또는 변경되었는지 확인하는 것이다. 매우 다양한 방식의 UPD check-sum이 존재한다. 그중 보수를 이용하여 모든 자리의 합이 1이 되는지 확인하는 방법이 있다. UDP는 TCP에 비해 가볍고 빠르며 신뢰성을 보장하는 것이 장점으로 여겨질 때가 많다. 또한 많은 링크 계층의 protocol이 error detection을 제공하는데 굳이 UDP가 check-sum을 제공하는 이유는 무엇일까. 바로 출발지와 목적지 사이에 있는 모든 링크가 error detection을 제공하는 것은 아니기 때문이다. 그러므로 segment가 정확하게 도착하였다고 하더라도 비트가 일부분 변경되어 전송되는 것이 가능하다. 이때 UDP header에 있는 check-sum을 확인하여 error를 detect한다. 하지만 이를 고치는 방법(error correction)을 포함하고 있지 않아서 단순히 참고만 할 수 있다. 


### 신뢰적인 데이터 전달(reliable data transfer)의 원리
**비트 오류와 손실 있는 채널 상에어싀 신뢰적 데이터 전송: rdt3.0**  

<img width="528" alt="Screen Shot 2020-07-09 at 6 19 47 PM" src="https://user-images.githubusercontent.com/44011462/87021960-ca72ae00-c210-11ea-939c-b00583924adb.png">

<details>
    <summary><span style="color:grey">클릭하여 출처보기</span></summary>
Computer Networking: A Top-Down Approach, Global Edition  <br>
Publisher: Pearson Higher Education; 7th edition (November 21, 2016)  <br>
ISBN-10: 1292153598  <br>
ISBN-13: 978-1292153599  <br>
199Page, 그림3-15 rdt3.0 송신자<br> 
</details>


비트가 손상되는 것 외에도 하위 채널이 패킷을 손실하는 경우를 생각할 수 있다. 

1. 어떻게 패킷 손실을 검출할 것인가
2. 패킷 손실이 발생했을 때 어떤 행동을 할 것인가

패킷 손실은 송신자에게 손실된 패킷과 회복 책임을 부여하여 해결 가능하다. 송신자가 데이터 패킷을 전송하고 ACK을 손실했다고 가정하자. 송신자에게 응답이 오지 않는다. 만약 송신자가 패킷을 잃어버렸다는 것을 확신할 수 있을 정도로만 기다릴 수 있다면 패킷은 재전송될 수 있다. 

**그렇다면 얼마나 기다려야 하는가??** 송신자는 적어도 송신자와 수신자 사이의 왕복시간 지연에 수신 측에서 패킷을 처리하는데 걸리는 시간을 더한 만큼 기다려야한다. 하지만 이 시간은 예측하기도 어려울 뿐 아니라, 시간을 기다리는 것은 오류 복구가 시작될 떄까지 기다려야하는 시간이 길수도 있다는 점에서 이는 좋지 못한 방법이다. 

실제 네트워크 상황에서는 송신자가 패킷 손실을 헀다는 보장은 없지만 만일 ACK이 일정 시간안에 도착하지 않는다면 재전송한다. 중복 데이터에 대한 해결은 순서번호를 이용하여 가능하다. 

### TCP

TCP는 application process가 data를 다른 process에게 보내기 전에 서로 먼저 "**handshake**"를 하기 때문에 연결 **지향적 (Connection-oriented)** 이다. TCP연결은 회선 교환 네트워크에서와 같은 종단간의 TDM (Time Division Multiplexing)이나 FDM (Frequenct Division Multiplexing)이 아니다. 연결 상태가 두 종단 시스템에만 존재하므로 가상회선도 아니다. TCP protocol은 오직 종단 시스템에만 동작하고 중간의 네트워크 요소에서는 동작하지 않으므로, 중단의 네트워크 요소들은 TCP 연결 상태를 유지하지 않는다. 물론 중간 router들은 TCP연결을 전혀 감지하지 못한다. 

TCP연결은 **전이중(Full-duplex)** 서비스를 제공한다. 또한 TCP 연결은 항상 **점대점(point to point)** 이다. TCP에서는 multicasting이  불가능하다.

    clientSocket.connect((serverName, serverPort));

client가 connect 요청을 보내면 server는 ACK을 담은 segment로 응답한다. 마지막으로 client가 SYN/ACK으로 다시 응답한다. 처음 2개의 segment에는 '**payload**', 즉 apllication layer data가 없다. 세번째 segment는 payload를 포함할 수 있다. 두 host사이에서 3개의 segment가 보내지므로 이 연결설정 절차는 흔히 **'three-way handshake'** 라 부른다.

#### TCP segment 구조

<img width="457" alt="Screen Shot 2020-07-09 at 8 22 20 PM" src="https://user-images.githubusercontent.com/44011462/87033900-fe0a0400-c221-11ea-8854-d543324d21c3.png">

<details>
    <summary><span style="color:grey">클릭하여 출처보기</span></summary>
Computer Networking: A Top-Down Approach, Global Edition  <br>
Publisher: Pearson Higher Education; 7th edition (November 21, 2016)  <br>
ISBN-10: 1292153598  <br>
ISBN-13: 978-1292153599  <br>
199Page, 그림3-29 TCP 세그먼트 구조<br> 
</details>

위 그림은 segment의 구조를 보여준다. UDP와 동일하게 header는 상위 계층 application으로부터 multiplexing과 demultiplexing을 하는데 사용하는 출발지(source)와 목적지(destination) port번호를 포함한다. 또한 UDP처럼 check-sum 필드를 포함한다. UDP와 다른 내용으로 아래와 같은 field를 포함한다.

   1. 32bits **순서번호 필드(SEQuence number filed)** 와 32bits **확인응답번호 필드(ACKnowledgement number field)** 가 있다. 이는 Reliable data transfer 서비스 구현에서 TCP 송신자와 수신자에 의해서 사용된다.
   2. 16bits **수신 윈도우(receive window)** 필드는 congestion control에 사용된다. 이는 수신자가 받을 수 있는 byte의 크기를 나타낸다.
   3. 4bits 헤더 길이(header length) 필드는 TCP header의 길이를 나타낸다. 이는 가변길이이며 일반적으로 20bits로 되어있다.
   4. 옵션(option)필드는 최대 세그먼트 크기(Maxium Segment Size, MSS)를 정하거나 window 확장 요소로 이용된다.
   5. **플래그(flag)** 필드는 6bits를 포함한다. **ACK 비트**는 확인응답 필드에 있는 값이 유효한지 확인하는데 사용한다. 

#### 흐름 제어 (Flow Control)
**흐름 제어(Flow Control)** 는 수신자(Receiver)와 송신자(Sender) 사이에 생기는 전송과 수신의 **속도차이**에 의해 발생한다. 이는 application process가 데이터를 받는 시점과 사용하는 시점이 항상 같은것이 아니기 떄문에 발생하는 문제이다. application process가 다른작업으로 인해 바쁘다면 전송받은 data를 buffer에 담아두고 필요한 시점에 사용한다. 이때 수신자의 data 처리속도가 송신자보다 느리다면 **buffer가 overflow**되어 정소를 소실한다. 이와 같은 문제를 해결하기 위해서 application에 **흐름 제어(Flow Control)** 를 제공한다. 

TCP는 송신자가 **수신 윈도우 (Receive Window)** 를 두어 흐름제어를 제공한다. 수신 윈도우는 수신측에서 가용한 크기가 얼마인지 송신자에게 알려주는데 사용한다. TCP는 전이중(Full-duplex)이기 때문에 수신자와 통신하는 각각의 송신자는 **수신 윈도우rwnd**를 두어 흐름을 통제한다. 

- LastByteRead: RcvBuffer로 부터 읽힌 data stream의 마지막 byte의 수
- LastByteRcvd: RcvBuffer에 저장된 data stream의 마지막 byte의 수 

    LastByteRcvd - LastByteRcvd >= RcvBuffer
    cwnd = RcvBuffer - (LastByteRcvd - LastByteRead)

위의 내용을 정리하면 아래와 같은 표를 얻을 수 있다.

<img width="391" alt="Screen Shot 2020-07-10 at 2 40 33 PM" src="https://user-images.githubusercontent.com/44011462/87120429-580ad800-c2bb-11ea-9a5e-1f4087fca583.png">


<details>
    <summary> <span style="color:grey">클릭하여 출처보기</span></summary>
Computer Networking: A Top-Down Approach, Global Edition  <br>
Publisher: Pearson Higher Education; 7th edition (November 21, 2016)  <br>
ISBN-10: 1292153598  <br>
ISBN-13: 978-1292153599  <br>
232Page, 그림3-38 수신 윈도우(rwnd)와 수신 버퍼(RcvBuffer)<br> 
</details>

송신자는 LastByteSent, LastByteAcked라는 변수를 두어 흐름 제어에 활용한다. 두 변수의 차이인 LastByteSent - LastByteAcked는 아직 ACK을 받지 못한 data의 양을 뜻한다. 이 값이 rwnd보다 작다면 수신자측의 RcvBuffer가 overflow되지 않음이 보장된다.

    LastByteSent - LastByteAcked <= rwnd

이 방식은 수신버퍼가 rwnd = 0으로서 가득 찼다고 가정하면 송신자는 data를 추가로 보내지 못하여 ACK을 받을수가 없게 된다. 이 경우에는 수신자 측에서 RcvBuffer를 비우게 되더라도 송신자측이 알 수 있는 방법이 없다. 따라서 rwnd = 0인 상황이 되면 송신자는 segment의 크기가 1byte인 작은 data를 지속적으로 보내어 ACK을 확인한다. 결과적으로 버퍼는 비워지며 문제를 해결한다.

#### TCP 연결 관리

TCP는 UDP와 달리 연결 설정을 하며, 연결 상태를 유지하도록 되어있다. **세방향 핸드쉐이크 (three-way handshake)** 를 통해 TCP의 연결을 설정하고 상태를 유지한다.


<img width="362" alt="Screen Shot 2020-07-10 at 3 16 45 PM" src="https://user-images.githubusercontent.com/44011462/87122848-9e166a80-c2c0-11ea-80b5-4b12c4f826a3.png">

<details>
    <summary> <span style="color:grey">클릭하여 출처보기</span></summary>
Computer Networking: A Top-Down Approach, Global Edition  <br>
Publisher: Pearson Higher Education; 7th edition (November 21, 2016)  <br>
ISBN-10: 1292153598  <br>
ISBN-13: 978-1292153599  <br>
234Page, 그림3-39 TCP 'three-way' handshake: 세그먼트 교환<br> 
</details>


1. client측 TCP는 application layer data를 포함하지 않고 SYN bit라는 flag bit를 포함하는 특별한 segment를 만든다. 이 segment를 **SYN segment**라고 부른다. 이 segment는 IP datagram안에 캡슐화되어 server측으로 보내진다.
2. TCP SYN segment를 포함하는 IP datagram이 server에 도착하면 server는 IP datagram으로부터 TCP SYN segment를 뽑아낸다. 이 정보를 가지고 TCP buffer와 변수를 할당한다. 이 과정후에 SYN에 대한 응답으로 **SYN-ACK segment**를 보낸다. SYN-ACK segment에도 SYN segment와 마찬가지로 application layer data를 포함하지 않는다. 앞서 받은 SYN segment의 header field 중에서 확인응답 필드(acknowledgement number field)의 값을 1만큼 증가시키고, 순서번호 필드 (sequence number field)에 server에 할당된 정보를 설정하여 보낸다. 
3. SYN-ACK segment를 수신하면 client는 확인응답 필드의 값을 1만큼 증가시키고, 앞선 과정을 통해 이미 연결되었기 때문에 SYN은 0으로 설정된다. 이후 application layer data를 payload로 넣어 segment를 보내며 통신한다.

이 과정을 three-way handshake라고 부른다. 이 방식은 암벽을 오르는 사람들이 줄을 이용하여 소통하는 방식과 매우 유사하다.
연결을 끝맺음할때는 segment header field에서 FIN flag를 1로 설정하여 끝을 알린다. 


<img width="404" alt="Screen Shot 2020-07-10 at 3 32 23 PM" src="https://user-images.githubusercontent.com/44011462/87123872-91931180-c2c2-11ea-9052-9644f7220a78.png">

<details>
    <summary> <span style="color:grey">클릭하여 출처보기</span></summary>
Computer Networking: A Top-Down Approach, Global Edition  <br>
Publisher: Pearson Higher Education; 7th edition (November 21, 2016)  <br>
ISBN-10: 1292153598  <br>
ISBN-13: 978-1292153599  <br>
236Page, 그림3-41 클라이언트 TCP에서 TCP 상태 전이의 일반적인 순서<br> 
</details>


#### 혼잡 제어 (Congestion Control)




# COMPUTER ARCHITECTURE


