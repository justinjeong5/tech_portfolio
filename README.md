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
  - [UPD (User Datagram Protocol), TCP (Transmission Control Protocol)](#upd-user-datagram-protocol-tcp-transmission-control-protocol)
    - [다중화(Multiplexing)와 역다중화(Demultiplexing)](#다중화multiplexing와-역다중화demultiplexing)
      - [비연결형 다중화](#비연결형-다중화)
      - [연결형 다중화](#연결형-다중화)
      - [web-server와 TCP](#web-server와-tcp)
    - [UDP](#udp)
      - [](#)
    - [TCP](#tcp)
      - [신뢰적인 데이터 전달(reliable data transfer)](#신뢰적인-데이터-전달reliable-data-transfer)
      - [congestion control](#congestion-control)
      - [](#-1)
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

#### congestion control

#### 

# COMPUTER ARCHITECTURE


