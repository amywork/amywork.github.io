---
layout: post
title: 용어 정리 01. 프로그래밍 기초
---

> 프로그래밍, 컴퓨터의 구조, 통신 방식 등 컴퓨터 공학 기초 용어들을 찾아보고 정리하는 노트입니다.

## ✔️ Programming
- 사용자의 입력에 반응하도록 구현된 명령어의 집합

### Programming VS Processing
- Program 을 만들면 → 그것이 메모리에서 실행이 되는게 Process
- 하드디스크 등의 매체에 바이너리(010101) 형식의 파일로 저장되어 있다가 사용자가 실행시키면 메모리로 적재되어 실행된다.
- 빌드란, 고급언어로 작성된 프로그램을 바이너리 형태로 하드디스크에 저장하는 것을 말한다.

### 프로그래밍 언어: 저급언어와 고급언어
- 저급언어 : 운영체제 필요 없이, 하드웨어 직접 제어 가능
  - 기계어 : 0과 1로 된 이진수 형태의 언어, 컴퓨터가 직접 이해할 수 있는 유일한 언어이다.
  - 어셈블리어 : 기호로 나타낸 언어. 어셈블러라는 번역기를 통해 기계어로 번역된다. 기계어보다 쉽게 작성할수 있다.

- 고급언어 : 컴파일러를 통해 기계어로 변환
  - IDE = 해당 언어를 기계어로 변환(컴파일)
  - Xcode가 컴파일 가능한 언어 : C, C++, Objective C, Swift
  - Objective-C는 컴파일이 Xcode로만 가능하며, Swift는 오픈소스이기 때문에 여러가지로 컴파일 가능
  - 컴파일 언어: 프로그램 → 컴파일러가 해석 → OS 타고 → 하드웨어로 바이너리 형태로 보냄 → 메모리에서 실행(프로세싱)
  - 스크립트 언어: 응용프로그램에서 실행 → 브라우저가 분석, 페이지 단위로 interpreter가 해석


<br>



## ✔️ Computer

- 컴퓨터의 정의: 입력, 연산, 출력, 기억을 통한 데이터 처리
- 하드웨어의 기준 (5가지)
- 중앙처리장치(central processing unit, CPU) = 연산
  - 입력장치 및 기억장치로부터 데이터를 받아 분석/처리
- 주기억장치(main memory unit)
  - 중앙처리장치가 처리할 데이터를 보관
  - 한 번 데이터를 기록하면 전원이 꺼져도 데이터가 남아 있는 롬(ROM)
  - 자유롭게 데이터를 쓰거나 지울 수 있지만 전원이 꺼지면 모든 데이터가 사라지는 램(RAM) = 저장
  - 특수한 목적의 컴퓨터가 아니라면 대부분 RAM을 사용
- 보조기억장치(secondary memory unit)
  - 처리 속도가 빠르지만, 전원이 꺼지면 데이터가 지워지는데다 용량도 적은 RAM을 보완하는 역할
  - 속도가 느린 편이지만, 전원이 꺼져도 데이터를 안전하게 보관할 수 있으며 용량이 큼
  - 하드 디스크나 CD-ROM, USB 메모리 등
- 입력장치(input device)
  - 컴퓨터를 활용하기 위해 각종 자료나 명령어를 입력할 때 쓰는 장치
  - 키보드나 마우스, 태블릿, 조이스틱, 디지털 카메라, 스캐너, 마이크, 터치스크린 등
- 출력장치(output device)
  - CPU에서 처리한 정보를 실체화하여 사용자에게 전달하는 역할
  - 모니터, 프린터, 스피커
  
<br>


## ✔️ HTTP
HTTP와 DNS는 HTML, 미디어 파일과 웹상에 존재하는 것들의 송수신을 관리합니다. 그리고 이것을 가능하게 하는 핵심적인 기술은 TCP/IP와 라우터 네트워크라는 작은 단위로 정보를 쪼개서 전달하는 것입니다. 이런 단위들은 0과 1의 이진수 배열로 이루어져있고, 전선이나 광섬유 또는 무선 네트워크(radio waves)를 통해 전달됩니다.

### **HTTP: Hyper Text Transfer Protocol**
✏️ 어떤 컴퓨터가 다른 컴퓨터에게 문서를 요청할 때 쓰는 언어 (HTTP 요청)
인터넷을 포함하여 일반적으로 사용하고 있는 네트워크는 TCP/IP라는 프로토콜에서 움직이고 있습니다. HTTP는 그중 하나입니다. 컴퓨터와 네트워크 기기가 상호간에 통신하기 위해서는 서로 같은 방법으로 통신하지 않으면 안됩니다. 프로토콜에는 여러가지가 있습니다. 케이블 규격, IP 주소 지정 방법, 웹을 표시하기 위한 순서 등입니다. 이렇게 인터넷과 관련된 프로토콜들을 모은 것을 TCP/IP라고 부릅니다. 

참고링크: [Khan Academy](https://ko.khanacademy.org/computing/computer-science/internet-intro/internet-works-intro/v/the-internet-http-and-html)

### **HTTP "Verbs"**
- GET: retrieves information from the specified source.
- POST: sends new information to the specified source.
- PUT: updates existing information of the specified source.
- DELETE: removes existing information from the specified source.


### **HTTPS: Hyper Text Transfer Protocol Secure**
- 256비트 암호화, 공개 키와 개인 키
- HTTPS signals the browser to use an added encryption layer of SSL/TLS to protect the traffic. 
<br>


## ✔️ 계층으로 관리하는 TCP/IP
- 애플리케이션 계층: FTP, DNS, HTTP
- 트랜스포트 계층: 애플리케이션 계층에 네트워크로 접속되어 있는 2대의 컴퓨터 사이의 데이터 흐름을 제공합니다.
- 네트워크 계층 (인터넷 계층): 네트워크 상에서 패킷의 이동을 다룹니다. 어떠한 경로를 거쳐 패킷을 보낼지 정하기도 합니다. 
- 링크 계층: 네트워크에 접속하는 하드웨어적인 면을 다룹니다. 

- TCP/IP는 애플리케이션, 트렌스포트, 데이터 링크, 링크 계층, 이렇게 4개로 나뉘어져있습니다. 계층화된 이유는 인터넷이 하나의 프로토콜로 되어 있다면 어디선가 사양이 변경되었을 때 전체를 바꾸지 않으면 안되지만 계층화되어 있으면 사양이 변경된 해당 계층만 바꾸면 되기 때문입니다. 

- TCP/IP로 통신을 할 때 계층을 순서대로 거쳐 상대와 통신을 합니다. 송신하는 측은 애플리케이션 계층에서부터 내려가고 수신하는 측은 애플리케이션 계층으로 올라갑니다. HTTP를 예로 들어 설명하면 먼저 송신측 클라이언트의 애플리케이션 계층(HTTP)에서 어느 웹 페이지를 보고 싶다라는 HTTP 리퀘스트를 지시합니다. 그 다음에 있는 트랜스포트계층(TCP)에서는 애플리케이션 계층에서 받은 데이터(HTTP 메시지)를 통신하기 쉽게 조각내어 안내 번호와 포트 번호를 붙여 네트워크 계층에 전달합니다. 네트워크 층(IP)에서는 수신지 MAC주소를 추가해서 링크 계층에 전달합니다. 

- 수신측 서버에서는 링크 계층에서 데이터를 받아들여 순서대로 위의 계층에 전달하여 애플리케이션까지 도달합니다. 애플리케이션 계층에 도달하게되면 드디어 클라이언트가 발신했던 HTTP 리퀘스트 내용을 수신할 수 있습니다. 

<br>


## ✔️ IPv4, IPv6, DNS

### **IP(Internet Protocol)**
- 컴퓨터의 주소, Bunch of Numbers are organized in a hierarchy.

### **IPv4**
- 인터넷 프로토콜의 4번째 판으로, 전 세계적으로 사용된 첫 번째 인터넷 프로토콜.
- 1973년에 만들어 졌고 80년대 초반에 널리 적용 되었으며 전자기기가 인터넷에 접속하기 위한 40억개 이상의 고유한 주소를 제공할 수 있다.
- 전통적으로 IP 주소는 32비트의 길이를 가지고 주소의 각 부분은 8비트의 길이를 가진다.
- 나라 → 지역 → 하위 네트워크 → 전자 기기의 주소


### **IPv6**
- IP 주소의 확장 : IPv4의 기존 32 비트 주소공간에서 벗어나, IPv6는 128 비트 주소공간을 제공한다.
- https://ko.wikipedia.org/wiki/IPv6


### **DNS(Domain Name System)**
- www.example.com 과 같은 주 컴퓨터의 도메인 이름을 192.168.1.0과 같은 IP 주소로 변환하고 라우팅 정보를 제공하는 분산형 데이터베이스 시스템이다.
- 가장 오른쪽 레이블은 최상위 도메인을 의미한다. 예를 들어, 도메인 이름 www.example.com 은 최상위 도메인 com에 속한다. 인터넷 초창기부터 com, net, org, edu, gov, mil의 6개의 일반 최상위 도메인(gTLD)이 사용되었다. 그 뒤, 필요에 따라 도메인이 추가되었다.
- 도메인의 계층 구조는 오른쪽부터 왼쪽으로 내려간다. 왼쪽의 레이블은 오른쪽의 서브도메인이다. 예를 들어, 레이블 example은 com 도메인의 서브도메인이며, www는 example.com의 서브도메인이다. 서브도메인은 127단계까지 가능하다.
- 각 레이블은 최대 63개 문자를 사용할 수 있고, 전체 도메인 이름은 253개 문자를 초과할 수 없다. 실제로는 도메인 레지스트리에 더 짧은 제한을 가질 수 있다.
- 레이블에 사용될 수 있는 문자는 아스키 문자 집합의 부분집합과 알파벳 a부터 z, A부터 Z, 숫자 0부터 9와 하이픈(-)을 포함한다. 이 규칙은 letter(문자), digit(숫자), hyphen(하이픈)의 첫글자를 따서 LDH 규칙이라고 한다. 도메인 이름은 대소문자 구분없이 해석되고, 레이블은 하이픈으로 시작하거나 끝날 수 없다.
- DNS를 잘못 설정할 경우 인터넷 이용에 문제가 생길 수 있다. 신뢰할 수 없는 DNS는 해킹, 파싱 등에 노출되므로, 신뢰할 수 있는 DNS 서버만 이용하는 것이 권장된다.
- 기본적으로 통신사가 제공하는 DNS는 서버가 한국에 소재하여 빠른 응답속도를 보여준다.
- 구글과 시만텍의 DNS는 한국에 서버가 소재하진 않지만, 인접국인 일본에 서버가 소재하고 있어, 40ms 미만의 응답속도를 보여준다.


### **Packet, Router**
- 패킷: 데이터의 형식화된 블록 / 라우터: 패킷 경로 전향 장치
- 하나의 파일은 패킷 교환망 안에서 전송되기 위하여 작은 크기의 데이터들로 나뉜다. 개별 데이터는 발신지 주소, 목적지 주소가 추가되어 하나의 단일한 패킷이 된다. 이런 패킷들의 나열(sequence)는 차례로 목적지까지 보내지고, 목적지에서는 이런 패킷 나열을 다시 원본 파일로 재구성하는 작업이 이루어진다. 각 패킷은 개별적으로 경로가 제어(라우팅:  어떤 네트워크 안에서 통신 데이터를 보낼 경로를 선택하는 과정)된다.
- 라우터 혹은 라우팅 기능을 갖는 공유기는 패킷의 위치를 추출하여, 그 위치에 대한 최적의 경로를 지정하며, 이 경로를 따라 데이터 패킷을 다음 장치로 전향시키는 장치이다. 이때 최적의 경로는 일반적으로는 가장 빠르게 통신이 가능한 경로이므로, 이것이 최단 거리 일수도 있지만, 돌아가는 경로라도 고속의 전송로를 통하여 전달이 되는 경로가 될 수 있다.

### **Wire, Cable, WiFi**
- Internet ships binary information. Information is made of BITS(binary codes, two possible states). 
- 8 bits together make 1 byte. 
- What is the physical stuff that actually gets sent over the wireless in the airwaves? Today we physically send bits by electricity, light (광섬유 케이블, 유리로 만든 실), and radio waves.(translate the ones and zeros into radio waves of different frequency.)
- Bit Rate: The number of bits per second a system can transmit.
- Band Width: Transition capacity, measured by bit rate.
- Latency: Time it takes for a bit to travel from sender to server. 

<br>


## ✔️ Server, Client 

### Server - Response
참고링크: [나무 위키](https://namu.wiki/w/%EC%84%9C%EB%B2%84)
- 서버도 컴퓨터다. 클라이언트에게 네트워크를 통해 서비스하는 컴퓨터. 
- The internet is a service that connects hard drives or servers together. 
- The web is a subset of the internet which deals mainly with HTML, CSS and Javascript files. We can pull other assets like images, videos and audio into our sites but they live within a web browser.
- 인터넷은 수많은 서버들이 거미줄처럼 얽혀서 형성된 것이다. 
- 홈페이지를 운영하려면 서버가 반드시 필요하며, 온라인 게임이나 웹게임들도 서버를 통해서 서비스를 하고 있다. 
- 규모가 있는 기관에서는 데이터베이스, 웹 어플리케이션 서버 등등에 방화벽, 라우터등이 붙어 네트워크를 형성한다.

### Client - Request
- 사용자가 서버에 접속하기 위해 사용하는 프로그램 또는 서비스를 말한다.
- 일반적으로 클라이언트는 서버가 제공하는 서비스를 사용자의 환경에서 구현할 수 있도록 하는 프로그램과 프로그램의 실행에 필요한 확장 파일 및 서비스에 필요한 암호화된 데이터 등으로 구성된다.
- https://youtu.be/KUXO_swGPMk

### Example.
- You’re streaming the songs using the internet to the device when you need them. The songs aren’t stored on your hard drive any more, they’re stored on the internet and you get them whenever you want them. The same thing is due of web browsers. If you’re looking at Google or Facebook, you’re downloading HTML, CSS and Javascript files on demand. Essentially web browsers were the first streaming service.
- On a site like Facebook, if I want to share a photo that’s currently on my phone and put it on Facebook’s very large hard drive (or their “server”), then I use the internet between Facebook and me to upload the photo.
- So all downloading really means is a file from someone else’s hard drive to my hard drive, and all uploading really means is sending a file from my hard drive to someone else’s using the internet. When you’re in a web browser like Google Chrome and you type in a web address like “www.google.com”, all you’re telling the web browser to do is download files from Google’s server for the homepage. What files are we downloading? HTML, CSS and Javascript files to show us the content, style and actions for that page.


<br>


## ✔️ 자료 구조 
- 프로그래밍에서 데이터를 구조적으로 표현하는 방식과 이를 구현하는 데 필요한 알고리즘에 대해 논하는 기초이론
- 컴퓨터의 메모리는 1차원 구조이기 때문에 현실 세계의 다차원 데이터를 다루기 위해서는 이것을 1차원인 선 형태로 바꾸는 것이 필요하다. 2차원 배열, 이진 트리, 그래프 등의 자료구조가 2차원 데이터를 1차원으로 욱여넣는 방법을 배우는 것이다. 더 나아가 3차원 데이터를 다루고, 더 지나면 3차원 데이터 이상의 다차원 데이터를 처리하는 자료구조를 만날 수 있다. 물론 B트리나 R트리의 경우처럼 같은 2차원 데이터도 어떻게 조직화하느냐에 따라 자료구조가 달라진다.

### Linked List
- 데이터 덩어리(노드 Node)를 저장할 때 그 다음 순서의 자료가 있는 위치를 데이터에 포함시키는 방식으로 자료를 저장한다.
- 배열에 비해 데이터의 추가/삽입 및 삭제가 용이하나 순차적으로 탐색하지 않으면 특정 위치의 요소에 접근할 수 없어 일반적으로 탐색 속도가 떨어진다. 즉 탐색 또는 정렬을 자주 하면 배열을, 추가/삭제가 많으면 연결 리스트를 사용하는 것이 유리하다. 
- 배열은 자기가 안 쓰는 공간까지 전부 예약해두고 있어야 하기 때문에 공간 낭비가 생긴다.
### Stack
- 후입선출(後入先出, Last In First Out; LIFO)
- 입력은 push, 출력은 pop
### Queue
- 선입선출(先入先出, First In First Out; FIFO), 대기열
- 입력 동작은 Enqueue, 출력 동작은 Dequeue
- 어떠한 작업/데이터를 순서대로 실행/사용하기 위해 대기시킬 때 사용한다.
### Tree

