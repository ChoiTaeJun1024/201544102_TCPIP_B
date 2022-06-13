22년도 1학기 TCP/IP 기말고사 프로젝트
=====================================
***

## 프로젝트 설명
* 클라이언트와 서버가 서로 채팅할 수 있는 기능을 구현하였습니다. 더불어 둘 이상의 쓰레드가 동시 접근하는 것을 막기 위하여 **뮤텍스(Lock)** 를 사용하였습니다.

***

## 프로젝트 실행 결과
<img width="" height="" src=result1.png></img>
<img width="" height="" src=result2.png></img>

## 프로젝트 코드
### Server.py
```python
  1 import socket
  2 import threading
  3 from threading import Thread
  4
  5 # 클라이언트 연결 처리
  6 def handleConnection(s, i):
  7     # 메세지 수신
  8     reply = s.recv(1024).decode("utf-8")
  9     print("Client: " + reply)
 10
 11     # 메세지 송신
 12     message = input("Server: ")
 13     s.send(str(message).encode("utf-8"))
 14
 15 # 소켓 설정
 16 s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
 17 s.bind(("0.0.0.0", 7777))
 18 s.listen(5)
 19 i = 0
 20
 21 # 잠금 정의
 22 lock = threading.Lock()
 23
 24 # 쓰레드 실행
 25 while True:
 26     lock.acquire()
 27     i += 1
 28     cs, addr = s.accept()
 29     t = Thread(target = handleConnection, args = (cs, i))
 30     t.start()
 31     lock.release()
                                                             7,5           12%
```

### Client.py
```python
  1 import socket
  2
  3 # 채팅을 유지하기 위해 반복문 사용
  4 while True:
  5
  6     # 서버에 연결
  7     s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
  8     s.connect(("127.0.0.1", 7777))
  9
 10     # 메세지 송신
 11     msg = input("Client: ")
 12     s.send(str(msg).encode())
 13
 14     # 메세지 수신
 15     reply = s.recv(1024).decode("utf-8")
 16     print("Server: " + reply)
"client.py" 16L, 370B                                         3,1           All
```
