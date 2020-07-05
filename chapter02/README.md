# CHAPTER 02. 간단한 프로토콜 HTTP

HTTP 프로토콜의 구조에 대해 설명한다.

<br>

## 2.1. HTTP는 클라이언트와 서버 간에 통신을 한다

HTTP는 클라이언트와 서버 간에 통신을 한다. 텍스트와 이미지 등과 같은 리소스를 필요하다고 요구하는 쪽이 클라이언트가 되고, 이러한 리소스를 제공하는 쪽이 서버가 된다.

![image](https://user-images.githubusercontent.com/43431081/86482060-23af8d00-bd8c-11ea-9913-497ac50c298f.png)

* HTTP를 사용하여 2대의 컴퓨터 간에 통신을 하는 경우, 한 번 통신을 할 때 마다 반드시 어느 한 쪽은 클라이언트가 되고 다른 한 쪽은 서버가 된다.
* 즉, HTTP는 클라이언트와 서버의 역햘을 명확하게 구별하고 있다.

<br>

## 2.2. 리퀘스트와 리스폰스를 교환하여 성립

![image](https://user-images.githubusercontent.com/43431081/86482302-a59fb600-bd8c-11ea-98a1-5ad2efa8035a.png)

* HTTP는 클라이언트로부터 리퀘스트(요청, Request)가 송신되며, 그 결과가 서버로부터 리스폰스(응답, Response)로 되돌아온다.
* 즉, 반드시 클라이언트측으로부터 통신이 시작되며 서버 측은 리퀘스트를 수신하지 않으면 리스폰스가 발생하는 경우가 없다.

<br>

![image](https://user-images.githubusercontent.com/43431081/86482703-72a9f200-bd8d-11ea-9859-4604b12ad7aa.png)

* **클라이언트 측 HTTP 리퀘스트 내용**

  ```http
  GET /index.html HTTP /1.1
  Host: www.naver.com
  ```

  * **"GET"** : 메소드. 서버에 요구하는 종류
  * **"/index.html"** : URI. 요구 대상인 리소스
  * **"HTTP/1.1"** : 클라이언트 기능을 식별하기 위한 HTTP 버전 번호

  > HTTP 서버 상에 있는 /index.html 리소스가 필요하다는 리퀘스트

<br>

리퀘스트 메시지는 `메소드` , `URI` , `프로토콜 버전` , `옵션 리퀘스트 헤더 필드` 와 `엔티티` 로 구성된다.

```http
POST /form/entry HTTP/1.1
Host: www.naver.com
Connection: keep-alive
Content-Type: application/x-www-form-urlencoded
Content-Length: 16

name=ueno&age=37
```

* **POST** : 메소드
* **/form/entry** : URI
* **HTTP/1.1** : 프로토콜 버전
* **Host, Connection, Content-Type, Content-Length** : 리퀘스트 헤더 필드
* **name=ueno&age=37** : 엔티티

<br>

* **서버 측 HTTP 응답** : `프로토콜 버전` , `상태 코드` , `상태 코드 설명` , `리스폰스 헤더 필드` , `바디` 로 구성된다.

  ```http
  HTTP/1.1 200 OK
  Date: Tue, 10 Jul 2012 06:50:15 GMT
  Content-Length: 362
  Content-Type: text/html
  
  <html>
  ...
  ```

  * **HTTP /1.1** : 서버의 HTTP 버전
  * **200 OK** : 리퀘스트의 처리 결과를 나타내는 상태 코드와 설명
  * **Content-Length, Content-Type** : 헤더 필드
  * **\<html> ... (빈 줄 아래의 내용들)** : 바디(body)

<br>

## 2.3. HTTP는 상태를 유지하지 않는 프로토콜

HTTP는 상태를 계속 유지하지 않는 **스테이트리스(stateless)** 프로토콜이다. 즉, HTTP 프로토콜 레벨에서는 이전에 보냈던 리퀘스트나 이미 되돌려준 리스폰스에 대해서는 전혀 기억하지 않는다.

그러므로 HTTP에서는 리퀘스트나 리스폰스가 보내질 때 마다 매번 새롭게 연결을 하고 데이터 전송이 끝나면 연결을 끊는다. 이렇게 설계된 이유는 수십만명이 웹 서비스를 사용하더라도 접속유지를 최소한으로 처리할 수 있게 되어, 많은 데이터를 매우 빠르고 확실하게 처리하는 **범위성(scalability)을** 확보할 수 있다.

그러나 웹이 진화하여 무상태 특성만으로는 처리하기 어려운 일이 생기게 되었다. 예를 들면, 쇼핑 사이트에 로그인했을 경우이다. 이때는 다른 페이지로 이동하더라도 로그인 상태를 유지할 필요가 있다. 이를 위해서는 누가 어떤 리퀘스트를 보냈는지를 파악하기 위해 상태를 유지할 필요가 있다. 

HTTP/1.1은 상태를 유지하지 않는 프로토콜이다. 그래서 상태를 계속 유지하고 싶은 요구에 부응하기 위해서 **쿠키(Cookie)라는** 기술이 도입되었다.

<br>

## 2.4. 리퀘스트 URI로 리소스를 식별

HTTP는 URI(Uniform Resource Identifiers)를 사용하여 인터넷 상의 리소스를 지정한다.