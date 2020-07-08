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

#### 


### TCP

#### 신뢰적인 데이터 전달(reliable data transfer)

#### Congestion Control

#### 