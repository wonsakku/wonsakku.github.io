---
title: "Abstract Factory 패턴"

categories:
    - design-pattern

tags:
     - [design-pattern, Abstract Factory]

date: 2021-07-24
last_modified_at: 2021-07-24
---

# 추상 팩토리 패턴이란?

추상 팩토리 패턴은 서로 연관되어 있는 객체들을 일정한 방식으로 생성하기 위해 사용하는 패턴인 것 같다. 추상 팩토리 패턴에서는 객체들을 만드는 Factory를 추상화하고(Abstract Factory), 이를 구체화하는 Factory들(Concrete Factory)을 통해 서로 관련있는 객체들을 생성한다. 이 때, Abstract Factory에서 정의한 추상 메소드를 Concrete Factory에서 오버라이딩함으로써 객체들을 일관된 방식으로 생성할 수 있다. 


> Head First Design Patterns - 한빛미디어

추상 팩토리 패턴에서는 인터페이스를 이용하여 서로 연관된, 또는 의존하는 객체를 구상 클래스를 지정하지 않고도 생성할 수 있다. 추상 팩토리 패턴을 사용하면 클라이언트에서 추상 인터페이스를 통해서 일련의 제품들을 공급받을 수 있습니다. 이 때, 실제로 어떤 제품이 생산되는지 전혀 알 필요도 없습니다. 따라서 클라이언트와 팩토리에서 생산되는 제품을 분리시킬 수 있습니다.   

추상 팩토리 패턴에서 메소드가 팩토리 메소드로 구현되는 경우도 종종 있습니다. 추상 팩토리가 원래 일련의 제품들을 생성하는데 쓰일 인터페이스를 정의하기 위해 만들어졌기 때문입니다. 그 인터페이스에 있는 각 메소드는 구상 제품을 생산하는 일을 맡고 있고, 추상 팩토리의 서브클래스를 만들어서 각 메소드를 구현을 제공합니다. 따라서 추상 팩토리 패턴에서 제품을 생산하기 위한 메소드를 구현하는 데 있어서 팩토리 메소드를 사용하는 것은 너무나도 자연스러운 일이라고 할 수 있습니다.

<br>

> GoF의 디자인 패턴 – 프로텍 미디어

- 의도

상세화된 서브클래스를 정의하지 않고도 서로 관련성이 있거나 독립적인 여러 객체의 군을 생성하기 위한 인터페이스를 제공합니다.

- 활용성

1. 객체가 생성되거나 구성ㆍ표현되는 방식과 무관하게 시스템을 독립적으로 만들고자 할 때
2. 여러 제품군 중 하나를 선택해서 시스템을 설정해야 하고 한번 구성한 제품을 다른 것으로 대체할 수 있을 때
3. 관련된 제품 객체들이 함께 사용되도록 설계되었고, 이 부분에 대한 제약이 외부에도 지켜지도록 하고 싶을 때
4. 제품에 대한 클래스 라이브러리를 제공하고, 그들의 구현이 아닌 인터페이스를 노출시키고 싶을 때

- 이익과 부담

1. 구체적인 클래스를 분리합니다.
추상 팩토리 패턴을 쓰면 응용프로그램이 생성할 객체의 클래스를 제어할 수 있습니다. 팩토리는 제품 객체를 생성하는 과정과 책임을 캡슐화한 것이기 때문에, 구체적인 구현 클래스가 사용자에게서 분리됩니다. 일반 프로그램은 추상 인터페이스를 통해서만 인스턴스를 조작합니다. 제품 클래스 이름이 구체 팩토리의 구현에서 분리되므로, 사용자 코드에서는 나타나지 않는 것입니다.

2. 제품군을 쉽게 대체할 수 있도록 합니다.
구체 팩토리의 클래스는 응용프로그램에서 한 번만 나타나기 때문에 응용프로그램이 사용할 구체 팩토리를 변경하기는 쉽습니다. 또한, 구체 팩토리를 변경함으로써 응용프로그램은 서로 다른 제품을 사용할 수 있게 변경됩니다. 추상 팩토리는 필요한 모든 것을 생성하기 때문에 전체 제품군은 한 번에 변경이 가능합니다.

3. 제품 사이의 일관성을 증진시킵니다.
하나의 군 안에 속한 제품 객체들이 함께 동작하도록 설계되어 있을 때, 응용프로그램은 한 번에 오직 한 군에서 만든 객체를 사용하도록 함으로써 프로그램의 일관성을 갖도록 해야 합니다. 추상 팩토리를 쓰면 이 점을 아주 쉽게 보장할 수 있습니다.

4. 새로운 종류의 제품을 제공하기 어렵습니다.
새로운 종류의 제품을 만들기 위해 기존 추상 팩토리를 확장하기가 쉽지 않습니다. 생성되는 제품은 추상 팩토리가 생성할 수 있는 제품 집합에만 고정되어 있기 때문입니다. 만약 새로운 종류의 제품이 등장하면 팩토리의 구현을 변경해야 합니다. 이는 추상 팩토리와 모든 서브클래스의 변경을 가져옵니다. 즉, 인터페이스가 변경되는 새로운 제품을 생성하는 연산이 추가되거나, 기존 연산의 반환 객체 타입이 변경되었으므로, 이를 상속받는 서브클래스 모두 변경되어야 합니다.


> JAVA 객체지향 디자인 패턴 - 한빛미디어

- Keypoint  

 추상 팩토리 패턴은 관련성 있는 여러 종류의 객체를 일관성 있는 방식으로 생성할 때 유용하다.
 
 추상 팩토리 패턴에서 나타나는 역할이 수행하는 작업은 다음과 같다.
- Abstract Factory : 실제 팩토리 클래스의 공통 인터페이스. 각 제품의 부품을 생성하는 기능을 추상 메서드로 정의한다.
- ConcreteFactory : 구체적인 팩토리 클래스로 AbstractFactory 클래스의 추상 메서드를 오버라이드함으로써 구체적인 제품을 생성한다.
- AbstractProduct : 제품의 공통 인터페이스
- ConcreteProduct : 구체적인 팩토리 클래스에서 생성되는 구체적인 제품


<br>

___


## Simple Code Example

> 아래의 코드는 [이야기's G - 자바 디자인 패턴 이해 8강 추상 팩토리 패턴 (Abstract Factory Pattern) - 2](https://youtu.be/soV1C6j9kkg) 에서 나오는 코드를 참고했다.


코드를 살펴보면 GuiFac에서 Button과 TextArea를 생성하는 추상 메소드가 정의되어 있다. 그리고 GuiFac을 상속받는 WinGuiFac과 MacGuiFac에서 각각 어떠한 Button과 TextArea를 생성하는지를 구현한다. 예를 들어, GuiFac에 WinGuiFac이 대입될 때는 WinButton과 WinTextArea가 각각 Button과 TextArea에 대입된다.

### 추상화한 부분


```java
public interface Button {
	public void click();
}
```
```java
public interface TextArea {
	public String getText();
}
```
```java
public interface GuiFac {
	public Button createButton();
	public TextArea createTextArea();
}
```

### 구체화한 부분


#### window


```java
public class WinButton implements Button{
	@Override
	public void click() {
		System.out.println("윈도우 GUI 버튼을 클릭한다.");
	}
}
```
```java
public class WinTextArea implements TextArea {
	@Override
	public String getText() {
		return "Window Text Area";
	}
}
```
```java
public class WinGuiFac implements GuiFac {
	@Override
	public Button createButton() {
		return new WinButton();
	}
	@Override
	public TextArea createTextArea() {
		return new WinTextArea();
	}
}
```
___

#### mac

```java
public class MacButton implements Button{
	@Override
	public void click() {
		System.out.println("Mac GUI 버튼을 클릭한다.");
	}
}
```
```java
public class MacTextArea implements TextArea {
	@Override
	public String getText() {
		return "Mac Text Area";
	}
}
```
```java
public class MacGuiFac implements GuiFac {
	@Override
	public Button createButton() {
		return new MacButton();
	}
	@Override
	public TextArea createTextArea() {
		return new MacTextArea();
	}
}
```

### 실행

```java
public class GuiFacCreator {

	public static GuiFac getInstance(String absoluteClassName) 
			throws InstantiationException, IllegalAccessException, ClassNotFoundException {
		
		return (GuiFac) Class.forName(absoluteClassName).newInstance();
	}
}
```
```java
public class Main {

	public static void main(String[] args){
		
		try {
			if(args.length < 1) {
				throw new Exception("클래스를 지정해주세요");
			}
			
			String absoluteClassName = args[0];
			GuiFac factory = GuiFacCreator.getInstance(absoluteClassName);
			Button button = factory.createButton();
			TextArea textArea = factory.createTextArea();
			button.click();
			System.out.println(textArea.getText());
		} 
		catch (InstantiationException e) {e.printStackTrace();} 
		catch (IllegalAccessException e) {e.printStackTrace();} 
		catch (ClassNotFoundException e) {e.printStackTrace();} 
		catch (Exception e) {e.printStackTrace();}
	}
}
```

___

<br>

## Test


### WinGuiFac 
![abstract-factory-01](/assets/images/2021/07/2021-07-24-abstract-factory-01.jpg)
![abstract-factory-02](/assets/images/2021/07/2021-07-24-abstract-factory-02.jpg)

#### 결과
![abstract-factory-03](/assets/images/2021/07/2021-07-24-abstract-factory-03.jpg)



### MacGuiFac 
![abstract-factory-04](/assets/images/2021/07/2021-07-24-abstract-factory-04.jpg)

#### 결과
![abstract-factory-05](/assets/images/2021/07/2021-07-24-abstract-factory-05.jpg)


## 글을 마치며...

위의 예제 코드는 '추상 팩토리 패턴이 대충 이런 느낌이구나...' 라는 걸 느끼게 해줄 정도의 코드인 것 같다. 하지만 디자인 패턴 책에 나와있는 코드나 내가 직접 짜보려고 하는 코드로 포스팅하려고 하니, 코드도 많이 길어지는 느낌이 들어 그나마 가장 간단한 예제로 선택했다. 이 예제 코드는 '관련있는 객체들을 일정한 방식으로 생성한다'라는 추상 팩토리 패턴의 취지를 잘 보여주고 있다는 점이 좋았지만, 코드가 간단하다보니 객체들을 생성할 때 싱글톤 패턴이나 팩토리 메서드 패턴을 적용해보지 못한 부분이 조금은 아쉽긴 하다.   

추상 팩토리 패턴에 관한 다른 코드를 찾아보게 되거나, 문득 머릿속에 추상 팩토리 패턴에 대한 소재가 생각나면 [내 github](https://github.com/wonsakku)에도 올려보려고 한다. 


