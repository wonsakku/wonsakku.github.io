---
title: "RestTemplate-01"

categories:
    - Spring

tags:
     - [Spring, RestTemplate, RestApi]

date: 2021-07-27
last_modified_at: 2021-07-27
---

# RestTemplate  

RestTemplate는 REST API를 호출할 때 사용할 메서드를 제공하는 클래스다. RestTemplate는 스프링 프레임워크가 제공하는 HTTP 클라이언트 기능의 진입점이 되는 클래스로서 REST API와 궁합이 잘 맞는 메서드를 많이 갖추고 있다.   
RestTemplate는 RestTmeplate 인스턴스를 생성하는 부분을 제외하면 RestTemplate 메서드를 호출하는 한 줄의 코드만으로 다음의 4가지를 해낼 수 있다.    
1. 요청 URI 조립
2. HTTP 요청 전송
3. HTTP 응답 수신
4. 응답 본문을 자바 객체로 전환


## RestTemplate 메소드

|메서드명|리턴타입|설명|
|:--:|:--:|:--|
|getForObject|T|GET 메서드를 사용해 리소스를 가져오기 위한 메서드. 응답 본문을 임의의 자바 객체로 변환해서 가져올 때 사용한다.|
|getForEntity|ResponseEntity|GET 메서드를 사용해 리소스를 가져오기 위한 메서드. 응답을 ResponseEntity로 가져올 때 사용한다.|
|postForObject|T|POST 메서드를 사용해 리소스를 만들기 위한 메서드. 응답 본문을 임의의 자바 객체로 변환해서 가져올 때 사용한다.|
|postForEntity|ResponseEntity|POST 메서드를 사용해 리소스를 만들기 위한 메서드. 응답을 ResponseEntity로 가져올 때 사용한다.|
|put|void|PUT 메서드를 사용해 리소스를 만들거나 갱신하기 위한 메서드|
|delete|void|DELETE 메서드를 사용해 리소스를 삭제하기 위한 메서드|
|exchange|ResponseEntity|임의의 HTTP 메서드를 사용해 리소스에 접근하기 위한 메서드. 요청 헤더에 임의의 헤더 값을 설정할 때 사용한다.|  

  
* 리소스를 갱신하거나 삭제한 후 응답 본문(ResponseBody)를 받고 싶은 경우에는 exchange를 이용할 수 있다.

