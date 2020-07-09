## UPD (User Datagram Protocol), TCP (Transmission Control Protocol)

<img width="648" alt="Screen Shot 2020-07-08 at 1 03 02 PM" src="https://user-images.githubusercontent.com/44011462/86874488-6fab5a80-c11b-11ea-86ef-186410754bff.png">
 
<details>
    <summary>클릭하여 출처보기</summary>
Computer Networking: A Top-Down Approach, Global Edition  <br>
Publisher: Pearson Higher Education; 7th edition (November 21, 2016)  <br>
ISBN-10: 1292153598  <br>
ISBN-13: 978-1292153599  <br>
173Page, 그림3-1 트랜스포트 계층은 애플리케이션간의 물리적 통신보다는 논리적 통신을 제공함 <br> 
</details>

### 다중화(Multiplexing)와 역다중화(Demultiplexing)
Transport layer는 전달받은 segment의 data를 application layer의 어떤 socket에 넘겨줄지 확인하기 위해서 segment의 field를 검사하는 것을 말한다. 그리고 이 segment를 해당 socket으로 보낸다. 이를 Multiplexing이라고 한다. 반대로 출발지 host에서 socket으로부터 data를 모으고 이에 대한 segment를 생성하기 위해서 각 데이터에 header정보로 캡슐화하고 그 segment를 network layer로 보내는 것을 Demultiplexing이라고 한다.

#### 비연결형 다중화

    clientSocket = socket(AF_INET, SOCK_DGRAM);

위와 같은 선언을 통해 비연결형(UDP) socket을 생성할 수 있다. 이 방법으로 socket을 생성할 때 transport layer는 자동으로 port번호를 socket에 할당한다. 하지만 직접 원하는 port 번호를 socket에 할당할 수 있다.

    clientSocket.bind('', 19157);

일반적으로 application의 client는 자동으로 port번호를 할당 받는 것에 반하여, application의 server측은 특정 번호를 할당한다. 이 포트정보를 가지고 segment를 생성하고 application layer에서 multiplexing, demultiplexing 과정을 거쳐 data가 보내지고 되돌아온다. 

<img width="516" alt="Screen Shot 2020-07-08 at 1 43 32 PM" src="https://user-images.githubusercontent.com/44011462/86877082-0f1f1c00-c121-11ea-8568-9f9fb3de8a54.png">
<details>
    <summary>클릭하여 출처보기</summary>
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
    <summary>클릭하여 출처보기</summary>
Computer Networking: A Top-Down Approach, Global Edition  <br>
Publisher: Pearson Higher Education; 7th edition (November 21, 2016)  <br>
ISBN-10: 1292153598  <br>
ISBN-13: 978-1292153599  <br>
183Page, 그림3-5 같은 웹 서버애플리케이션과 통신하기 위해 같은 목적지 포트 번호(80)를 이용하는 두 클라이언트  <br> 
</details>

apache tomcat과 같은 web server는 port 번호 80에서 동작한다. 위 그림은 web server가 각각의 연결에 따라서 새로운 process를 fork하는 것을 보여준다. 이 연결을 통해서 HTTP 요청을 수신하고 HTTP 응답을 전송한다. 그러나 socket과 process가 모두 일대일 대응 관계를 갖는 것은 아니다. 실제로 고성능 웹 서비스들은 하나의 process를 사용하면서 여러개의 thread를 생성하여 서비스에 응답한다. 만약 client와 server가 지속적인(persistent) HTTP를 사용한다면 해당 기간동안 동일한 socket을 이용하여 통신한다. 이에 반하여 비지속적인(non-persistent) HTTP를 사용한다면 모든 응답/요청마다 새로운 TCP연결이 생성되고 종료되며 이는 overhead에 따른 성능상의 큰 문제가 된다. 

### UDP 

UDP는 transport layer protocol이 할수 있는 최소 기능으로 동작한다. UDP는 multiplexing, demultiplexing등의 기능과 간단한 오류검사를 제외하면 IP에 아무것도 추가하지 않는다. 사실 application 개발자가 TCP대신 UDP를 채택하여 개발한다면 application은 거의 IP와 직접 통신하는 셈이다.
UPD는 segment를 송신하기 저에 transport layer사이에서 handshake를 사용하지 않는다. 이런 이유로 UPD를 비연결형(connectionless)이라고 부른다.

DNS는 일반적으로 UDP를 사용하는 application layer의 protocol이다. host에서 DNS application query를 생성할 때, DNS query 메세지를 작성하고 UDP에 넘겨준다. 목적지 종단 시스템에서 동작하는 UDP와 host는 어떠한 handshake도 수행 하지 않고 메세지에 header를 추가한 후에 segment를 network layer에 넘겨준다. 이때 query를 보낸 DNS application은 query에 대한 응답을 기다린다. 만약 host가 응답을 수신하지 못하면 query를 다른 Name server로 송신하거나 application에게 응답을 수신할 수 없다는 것을 알려준다.

#### UDP가 TCP보다 좋은점
TCP는 신뢰적인 데이터 전송 (reliable data transfer) 서비스를 제공하지만, UDP는 그렇지 않으므로 항상 TCP가 더 좋을까? 답은 '아니오'이다. 다음과 같은 이유에서 UDP를 사용하는 것이 더 적합하다.

**실시간성이 중요한 application**
UDP에서는 application process가 data를 UPD에게 전달하면 UDP는 data를 UDP segment로 만들어서 바로 network layer로 전송한다. 이에 반하여 TCP는 congestion control을 실시하여 목적지 host와 출발지 host 사이가 과도하게 혼잡하면 전송속도를 조절한다. 또한 TCP는 신뢰적인 전달을 중요시 하므로 응답시간의 빠르기와 상관없이 목적지 host가 segment의 수신여부를 확인할때까지 기다리거나 재송신한다. UDP를 사용하면 조금의 data loss는 발생할 수 있지만, application 개발자가 일부 data loss는 허용할 수 있으므로 TCP는 위의 조건에 맞지 않는다.

**연결 설정이 없다**
TCP는 data 전송을 시작하기전에 3-way handshake를 사용한다. 반면에 UDP는 형식적인 예비동작없이 data를 전송한다. 그러므로 UDP는 연결을 설정하기 위한 어떠한 지연도 없다. 이 이유로 DNS가 TCP가 아닌 UDP를 사용한다. DNS가 TCP를 사용한다면 많이 느려진다. 하지만 HTTP로 작성된 웹 페이지는 신뢰성이 중요하기 때문에 보통은 TCP로 동작한다. 

**연결 상태가 없다**
TCP는 종단 시스템에서 연결 상태를 유지한다. 이 연결상태는 수신버퍼, 송신버퍼, 혼잡제어의 변수, 순서번호와 응답확인번호 등등의 정보를 포함한다. 따라서 동일한 조건이라면 TCP보다 UDP가 조금더 많은 client를 보유할 수 있다.

**작은 packet header overhead**
TCP가 segment마다 20bytes의 header overhead를 갖는 반면에 UDP는 단지 8bytes의 header overhead를 갖는다.

##### UDP segment 구조

<img width="213" alt="Screen Shot 2020-07-09 at 6 05 17 PM" src="https://user-images.githubusercontent.com/44011462/87020400-c34aa080-c20e-11ea-8e21-42dccc9db59f.png">

<details>
    <summary>클릭하여 출처보기</summary>
Computer Networking: A Top-Down Approach, Global Edition  <br>
Publisher: Pearson Higher Education; 7th edition (November 21, 2016)  <br>
ISBN-10: 1292153598  <br>
ISBN-13: 978-1292153599  <br>
187Page, 그림3-7 UDP 세그먼트 구조<br> 
</details>

###### UDP check-sum

UDP check-sum은 error detection을 제공한다. segment가 출발지에서 목적지로 이동핸 후 UDP segment의 정보가 손실 또는 변경되었는지 확인하는 것이다. 매우 다양한 방식의 UPD check-sum이 존재한다. 그중 보수를 이용하여 모든 자리의 합이 1이 되는지 확인하는 방법이 있다. UDP는 TCP에 비해 가볍고 빠르며 신뢰성을 보장하는 것이 장점으로 여겨질 때가 많다. 또한 많은 링크 계층의 protocol이 error detection을 제공하는데 굳이 UDP가 check-sum을 제공하는 이유는 무엇일까. 바로 출발지와 목적지 사이에 있는 모든 링크가 error detection을 제공하는 것은 아니기 때문이다. 그러므로 segment가 정확하게 도착하였다고 하더라도 비트가 일부분 변경되어 전송되는 것이 가능하다. 이때 UDP header에 있는 check-sum을 확인하여 error를 detect한다. 하지만 이를 고치는 방법을 포함되어 있지 않아서 단순히 참고만 할수 있다. 




### 신뢰적인 데이터 전달(reliable data transfer)의 원리
**비트 오류와 손실 있는 채널 상에어싀 신뢰적 데이터 전송: rdt3.0**  

<img width="528" alt="Screen Shot 2020-07-09 at 6 19 47 PM" src="https://user-images.githubusercontent.com/44011462/87021960-ca72ae00-c210-11ea-939c-b00583924adb.png">

<details>
    <summary>클릭하여 출처보기</summary>
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

그렇다면 얼마나 기다려야 하는가?? 송신자는 적어도 송신자와 수신자 사이의 왕복시간 지연에 수신 측에서 패킷을 처리하는데 걸리는 시간을 더한 만큼 기다려야한다. 하지만 이 시간은 예측하기도 어려울 뿐 아니라, 시간을 기다리는 것은 오류 복구가 시작될 떄까지 기다려야하는 시간이 길수도 있다는 점에서 이는 좋지 못한 방법이다. 

실제 네트워크 상황에서는 송신자가 패킷 손실을 헀다는 보장은 없지만 만익 ACK이 일정 시간안에 도착하지 않는다면 재전송한다. 중복 데이터에 대한 해결은 순서번호를 이용하여 가능하다. 

### TCP

TCP는 application process가 data를 다른 process에게 보내기 전에 서로 먼저 "handshake"를 하기 때문에 연결 **지향적 (Connection-oriented)**이다. TCP연결은 회선 교환 네트워크에서와 같은 종단간의 TDM (Time Division Multiplexing)이나 FDM (Frequenct Division Multiplexing)이 아니다. 연결 상태가 두 종단 시스템에만 존재하므로 가상회선도 아니다. TCP protocol은 오직 종단 시스템에만 동작하고 중간의 네트워크 요소에서는 동작하지 않으므로, 중단의 네트워크 요소들은 TCP 연결 상태를 유지하지 않는다. 물론 중간 router들은 TCP연결을 전형 감지하지 못한다. 

TCP연결은 전이중(Full-duplex) 서비스를 제공한다. 또한 TCP 연결은 항상 점대점(point to point)이다. TCP에서는 multicasting이  불가능하다.

    clientSocket.connect((serverName, serverPort));


#### Congestion Control

#### 