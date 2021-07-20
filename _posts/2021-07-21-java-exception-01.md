---
title: "예외처리 - try-catch와 throws Exception의 차이"
export: "예외처리를 잘하자..."

categories:
    - java

tags:
     - [java]

date: 2021-07-21
last_modified_at: 2021-07-21
---

## **try-catch**

try-catch는 try문에서 작성한 코드에서 예외가 발생하게 되면, 예외 발생 지점에서 try 블록의 코드의 진행은 중단되고 catch 블록에서 이후의 작업을 진행하게 된다. 

```java
@Data
public class Person {

  private String name;
  private int age;
}
```
```java
public class Processor {
  public static void process(){
    Person person = null;
    try {
    	person.getAge();
		System.out.println("Exception이 발생해서 try 블록은 여기부터 진행되지 않습니다.");
    }catch (NullPointerException e) {
      System.out.println("person이 null이라서 NullPointerException이 발생했습니다.");
    }catch (Exception e) {
      e.printStackTrace();
    }
    System.out.println("****************************");
    System.out.println("Processor.process() is Done!");
    System.out.println("****************************");
  }
}
```
```java
public class Main {
  public static void main(String[] args) {
	  Processor.process();
	  System.out.println("Main.main() 종료");
  }
}
```
  
위의 코드를 실행하면 콘솔창에 다음과 같이 나온다.

<br>

```
person이 null이라서 NullPointerException이 발생했습니다.
****************************
Processor.process() is Done!
****************************
Main.main() 종료
```


<br>


위의 그림을 보면 Process.process()의 try 블록에서 Exception이 발생했을 때 그 이후의 코드 흐름은 catch 구문으로 넘어가고 process() 작업을 완전히 끝낸 것을 확인할 수 있다.

___

<br>

## **throws Exception**

a()라는 메소드에서 b()라는 메소드를 호출하고, b()에서 Exception이 발생했다고 해보자. 만약 위와 같이 b()에서 try~catch구문 안에서 Exception이 발생했다면 그 Exception에 대한 처리는 b()에서 하게 된다. 하지만, b()에서 try~catch 대신 throws Exception을 사용했을 경우에는 b()에서 발생한 Exception의 처리를 a()에게 위임하게 된다. 


<br>

```java
@Data
public class Person {
  private String name;
  private int age;
}
```
```java
public class Processor {
  public static void process() throws Exception{
    Person person = null;
    person.getAge();
    
    System.out.println("Exception이 발생해서 Processor.process()는 여기부터 진행되지 않습니다.");
    System.out.println("****************************");
    System.out.println("Processor.process() is Done!");
    System.out.println("****************************");
  }
}
```
```java
public class Main {
	public static void main(String[] args) {
		
		try {
			Processor.process();
		} catch (NullPointerException e) {
			System.out.println("Processor.process()에서 발생한 Exception을 Main에서 처리했습니다.");
		} catch (Exception e) {
			e.printStackTrace();
		}
		System.out.println("Main.main() 종료");
	}
}
```
위의 코드를 실행하면 콘솔창에 다음과 같이 나온다.

<br>

```
Processor.process()에서 발생한 Exception을 Main에서 처리했습니다.
Main.main() 종료
```

<br>


위의 그림을 보면 Processor.process()에서 throws Excetion을 적용했다. 그 결과 Processor.process()에서 발생한 Exception은 Processor.process()를 호출한 Main.main()에서 처리하게 되었다. 

___

<br>

## **왜 Exception을 정리해보았는가?**    

내 기억 상으론 학원에서 코딩을 배울 때 예외 처리로 try-catch 구문은 배웠지만 throws Exception은 배우지 않았다(아마도...). 그런데 간간이 인강이나 블로그에서 본 코드들 중에 메소드 옆에 throws Exception이 적혀져 있는 것을 볼 수 있었다. 얼마 전까지만 하더라도 try-catch와 throws Exception은 그냥 예외처리하는 방법이었을 뿐이고, 어느 것을 적용하든 콘솔창에 빨간 글씨로 e.printStackTrace()가 올라오는건 마찬가지였었다. 그런데 프로젝트를 진행하면서 예외처리를 제대로 하지 못해 엄청 고생하게 되었고, 예외처리가 얼마나 중요한지 깨닫게 되었기에 이번 주제를 Exception 처리로 하였다.  

다음 포스팅에는 예외처리를 어떻게 적용해면 되는지에 대해서 적어보려고 한다.

