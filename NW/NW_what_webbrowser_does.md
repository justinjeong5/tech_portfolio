## what webbrowser does?

<img src="https://user-images.githubusercontent.com/44011462/93552900-b8e61900-f9ac-11ea-8e77-c13a6b6334de.png" width=80px><img src="https://user-images.githubusercontent.com/44011462/93552970-dd41f580-f9ac-11ea-958b-30c8603731a4.png" width=80px> <img src="https://user-images.githubusercontent.com/44011462/93552948-cf8c7000-f9ac-11ea-8a9e-556ae421272b.png" width=80px>

chrome, IE, safari등의 web browser에서 www.google.com을 검색하면 어떤일이 일어날까?


<img src="https://user-images.githubusercontent.com/44011462/93555188-3bbca300-f9b0-11ea-8dcc-f50913c1c611.png" width=400px>
<details>
    <summary><span style="color:grey">클릭하여 출처보기</span></summary>
Computer Networking: A Top-Down Approach, Global Edition  <br>
Publisher: Pearson Higher Education; 7th edition (November 21, 2016)  <br>
ISBN-10: 1292153598  <br>
ISBN-13: 978-1292153599  <br>
460Page, 그림6-32 웹 페이지 요총 과정: 네트워크 설정 및 동작<br> 
</details>  
  
**laptop에서 www.google.com을 띄우는 과정**
1. DHCP를 이용하여 주소 정보를 얻는 과정
    1. laptop은 자신의 IP, First_hop router의 IP, DNS서버의 IP를 알아야한다.
    2. 주소 정보를 알기 위해 DHCP요청 메세지를 UDP segment에 넣는다.
    3. 이떄 DHCP서버인 포트번호 67을 목적지로, DHCP클라인트인 포트번호 68을 출발지로 한다.
    4. UDP segment는 출발지는 00.00.00.00, 목적지는 all F(255.255.255.255)로 하여 broadcast한다. 
    5. router에서 작동중인 DHCP서버는 해당 UDP segment를 역다중화(Demultiplexing)하여 DHCP요청 메세지를 얻는다.
    6. DHCP서버는 laptop의 IP, First_hop router의 IP, DNS 서버 IP를 DHCP 응답 메세지에 담아 보낸다.
    7. 이따 router는 unicast를 통해 switch로 보내고 switch는 자가 학습을 통해 이미 알고있는 interface 출력 버퍼에 넣는다.
    8. laptop은 이제 자신의 IP, First_hop router의 IP, DNS서버의 IP를 알게 된다.  

2. 구글 서버에 패킷을 보내는 과정
    1. laptop은 구글의 IP정보를 알아야 한다. 
    2. 주소 정보를 알기 위해 DNS 서버에 query를 보내야한다.
    3. 랩탑은 router IP와 ARP, query를 포함한 정보를 all F(255.255.255.255)로 하여 broadcast한다.
    4. 자신의 UP주소인 것을 확인한 router는 자신의 MAC주소를 담은 ARP ACK을 laptop에 보낸다.
    5. laptop은 router의 MAC 주소를 알게 된다.
    6. router는 DNS서버 IP가 담긴 DNS query를 forwarding table과 비교하여 목적지가 외부 AS에 있다는 것을 알게 된다.
    7. DNS query가 목적지까지 가는동안 AS내부에서는 RIP, OSPF를 이용하고 AS간에는 BGP를 이용하여 경로를 이동한다.
    8. DNS서버는 DNS query를 받고 www.google.com의 IP를 알아내어 DNS 응답 메세지를 laptop에게 보낸다.
    9. laptop은 www.google.com의 IP정보를 알게 된다.

3. 구글 서버로부터 랩탑으로의 통신
    1. laptop은 google 서버와 통신하기 위해 TCP socket을 생성하고 TCP SYN을 보낸다.
    2. google 웹 서버는 TCP SYN/ACK을 보낸다.
    3. SYN/ACK를 통해 연결이 된것을 알게된 laptop은 TCP ACK을 보낸다.
    4. 이제 연결이 되었으므로 laptop은 google 웹 서버에 http 요청 메세지를 보낸다.
    5. google 웹 서버는 http 요청 메세지를 받고 해당 content를 http의 body에 넣어 http 응답 메세지를 보낸다.
    6. laptop에 google 화면이 나타난다.


