---

---
#network #posix #winsock 
___
## 소켓(Network Socket)
___
소켓은 인터넷 상에서 통신을 하기 위해 사용되는 프로그램 통신의 종착점(엔드포인트[^1])이다.
현재 통신은 인터넷을 사용해 이루어지기 때문에 프로토콜은 [[IP]], [[TCP]], [[UDP]]를 사용하고 있다.

## 소켓 프로그래밍
___
소켓 프로그래밍은 기본적으로는 버클리 소켓이 표준처럼 되어있어 비동기 소켓을 제외하고는 모두 사용법은 같다. 하지만 사용되는 라이브러는 다른데 Windows는 winsock을 사용하고 있고, Unix계열은 POSIX를 사용하고 있다. 그래서 소켓의 구현도 다르다.

POSIX에선 소켓 객체를 [[File descriptor]]로, winsock에선 파일 핸들(포인터)로 구현되어 있다.

### 헤더
---
- Windows
	- `<winsock2.h>`
	- `<ws2tcpip.h>`
- Unix 계열
	- `<netinet/in.h>`
	- `<sys/socket.h>`



[^1]: End Point. IP주소 + 포트의 조합이다.