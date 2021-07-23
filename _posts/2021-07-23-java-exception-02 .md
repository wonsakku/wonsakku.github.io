---
title: "예외처리해보기"
export: "예외처리를 잘하자..."

categories:
    - java

tags:
     - [java]

date: 2021-07-23
last_modified_at: 2021-07-23
---

# 예외처리 해보기

프로그래밍을 하다보면 의도했든, 의도하지 않았든 Exception이 발생하기 마련이다. Exception이 발생했을 때 개발자에게나 프로그램을 사용하는 사용자에게나 Exception에 맞는 메세지를 보내주는 것이 필요할 것이다. 개발자의 경우에는 그에 맞게 코드를 수정하도록, 사용자의 경우에는 적절한 형태로 서버에 요청을 할 수 있도록 알려줘야하기 때문이다.   
아래에는 예외처리를 해보기 위한 코드를 작성했다. 코드의 로직은 다음과 같다.

 - postman으로 email과 password를 입력해서 요청을 보낸다.
 - email이나 password가 없으면 BadRequestException으로 예외처리를 한다.
 - email이 중복되면 AlreadyExistException으로 예외처리를 한다.
 - 예외가 발생하지 않으면 userInfos에 email을 key 값으로 User정보를 추가한다.


<br>

___

## 의존성

 - Spring Web
 - Lombok

___

<br>

## Code

<br>

```java
@Data @Builder
public class UserVO {
	private String email;
	private String password;
}
```
```java
public class BadRequestException extends Exception{
	public BadRequestException() {
		super("요청 형식이 잘못되었습니다.");
	}
}
```
```java
public class AlreadyExistException extends Exception{
	public AlreadyExistException() {
		super("이미 존재하는 이메일입니다.");
	}
}
```
```java
@Service
public class TestService {

	private Map<String, UserVO> userInfos = new HashMap<>();
	
	public void enroll(UserVO user) throws Exception{
		
		if(user.getEmail() == null || user.getPassword() == null) {
			throw new BadRequestException();
		}
		
		String email = user.getEmail();
		
		if(userInfos.get(email) != null) {
			throw new AlreadyExistException();
		}	

		userInfos.put(email, user); // DB insert 대신 map에 넣는 것으로 대체했다.
	}
}
```
```java
@RestController @Slf4j
public class TestController {

	@Autowired
	private TestService service;
	
	@PostMapping("/user")
	public ResponseEntity<Map<String, String>> enroll(@RequestBody UserVO user){

		Map<String, String> returnBody = new HashMap<>();
		HttpStatus status = null;
		
		try {
			service.enroll(user);
			returnBody.put("Msg", "등록되었습니다.");
			returnBody.put("StatusCode", "200");
			status = HttpStatus.OK;
		}catch (BadRequestException e) {
			log.error(e.getMessage());
			returnBody.put("Msg", "요청 형식이 잘못되었습니다.");
			returnBody.put("StatusCode", "400");
			status = HttpStatus.BAD_REQUEST;
		}catch (AlreadyExistException e) {
			log.error(e.getMessage());
			returnBody.put("Msg", "이미 존재하는 이메일입니다.");
			returnBody.put("StatusCode", "409");
			status = HttpStatus.CONFLICT;
		}catch (Exception e) {
			log.error(e.getMessage());
			returnBody.put("Msg", "등록 중 문제가 발생했습니다.");
			returnBody.put("StatusCode", "500");
			status = HttpStatus.INTERNAL_SERVER_ERROR;
		}
		
		return ResponseEntity.status(status).body(returnBody);
	}
}
```

## Test

- password 또는 email없이 요청을 보냈을 경우

![exception-image-02-01](/assets/images/2021/07/2021-07-23-java-exception-02-01.jpg)

<br>

- 등록에 성공했을 경우

![exception-image-02-02](/assets/images/2021/07/2021-07-23-java-exception-02-02.jpg)

<br>

- email이 중복되었을 경우

![exception-image-02-03](/assets/images/2021/07/2021-07-23-java-exception-02-03.jpg)

<br>

___

<br>

## 문제가 발생했었던 코드 예시

이전 포스팅에서 예외처리를 제대로 하지 못해 고생한 적이 있다고 했다. 그 당시의 코드를 비슷하게 재현해보면 아래와 같다.
(위의 코드에서 Service와 Controller 내용만 추가했다.)


```java
@Service
public class TestService {
	
	...

	public boolean badCase(UserVO user) {
		
		boolean isComplete = false;
		
		try {
			String email = user.getEmail();
			if(userInfos.get(email) != null) {
				throw new AlreadyExistException();
			}
			
			System.out.println("이후의 로직에서 Exception이 발생한다.(DB관련 Exception이나 다른 어떠한 이유로....)");
			System.out.println(1/0); // 예외가 발생하는 부분

			userInfos.put(email, user); // DB insert 대신 map에 넣는 것으로 대체했다.
			
			isComplete = true;
		}catch (Exception e) {
			e.printStackTrace();
		}	
		return isComplete;
	}
}
```
```java
@RestController @Slf4j
public class TestController {
	
	...

	@PostMapping("/bad-case")
	public ResponseEntity<Map<String, String>> badCase(@RequestBody UserVO user){
		
		Map<String, String> returnBody = new HashMap<>();
		HttpStatus status = null;
		boolean isComplete = false;
		
		try {
			isComplete = service.badCase(user);
			
			if(isComplete) {
				returnBody.put("Msg", "등록되었습니다.");
				returnBody.put("StatusCode", "200");
				status = HttpStatus.OK;
			}else {
				returnBody.put("Msg", "이미 존재하는 이메일입니다.");
				returnBody.put("StatusCode", "409");
				status = HttpStatus.CONFLICT;
			}			
		}catch (Exception e) {
			log.error(e.getMessage());
			returnBody.put("Msg", "등록 중 문제가 발생했습니다.");
			returnBody.put("StatusCode", "500");
			status = HttpStatus.INTERNAL_SERVER_ERROR;
		}		
		return ResponseEntity.status(status).body(returnBody);
	}
}
```

![exception-image-02-04](/assets/images/2021/07/2021-07-23-java-exception-02-04.jpg)

![exception-image-02-05](/assets/images/2021/07/2021-07-23-java-exception-02-05.jpg)

<br>

위의 코드를 살펴보면 service단에서 email 중복이 아닌 다른 이유로 Exception이 발생했다. 하지만 service의 try-cath 블럭에서  Exception을 처리했기 때문에 testService.badCase()의 로직은 isComplete(false)를 return시키면서 완료가 된다. 결국 controller에서는 isComplete를 false로 받았기 때문에 클라이언트에는 email이 중복되었다는 메세지를 보내게 된다. 즉, service단에서 어떠한 Exception이 발생하더라도 클라이언트 쪽에서는 email이 중복되었다는 메세지만 받게 되는 황당한 경우를 겪게 된다.   

위의 코드를 작성할 때는 email 중복이 일어날 때 throw Exception을 날리기 때문에 email 중복 이외에는 예외가 발생하지 않을 것이라는 엄청난 확신(?)을 가졌던 것 같다. ~~'무식하면 용감하다'라는 말이 어울리는 것 같다.~~

<br>

___

## 글을 마치며

위와 같은 코드를 짜게 된건...  

1. 예외처리에 대한 개념이 잘 잡혀있지 않아서...
2. 때문에 예외처리를 제대로 해본적이 없어서...

인 듯 싶다. 

이 글의 첫 부분에 작성했던 코드도 예외처리를 제대로 했는지 확신할 수는 없지만,  끝 부분에서 작성한 코드보단 조금은 더 나아지지 않았나 라는 생각이 든다.

이 글을 작성할 때 Spring에 Validation 기능이 있는 것을 알게 됐다.(실은 스프링 처음 배울 때 인강에서 몇 번 보기는 했었는데, 안쓰다보니 까먹었다.) Validation을 사용해서 예외처리하는 것도 하나의 방식이 될 수 있는 것 같다.(Spring Restapi 인강에서 사용하던데 대충대충 넘기면서 봤기 때문에 어떻게 사용했는지는 잘 모르겠다.) 어쨌든, 다음에는 Spring의 Validation 사용법이랑 이를 이용해서 클라이언트에 어떻게 Retrun Message를 보내는지도 공부해봐야 겠다. 
