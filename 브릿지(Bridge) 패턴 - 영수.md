# 브릿지 패턴(Bridge Pattern)



* 브릿지 (Bridge) 의 사전적 의미 :  다리 구조물, 두 물체를 연결해주는 장치

* 다른 이름 : 핸들 / 구현부(Handle / Body)

    
  


브릿지 패턴은 추상적인 것과 구체적인 것을 분리하여 연결하는(Bridge) 패턴이다.

* 구현(implementation) 으로 부터 추상(Abstraction) 레이어를 분리하여 이 둘이 서로 독립적으로 변화할 수 있도록 하기 위함.
  * 추상적인 것 : Abstraction - 기능을 처리(구성)하는 클래스. implementation을 이용하여 기능을 구성한다.
  * 구체적인 것 : implementation - 실제 기능을 구현하는 클래스 - Abstraction에 의해 사용되어 진다. 

* 기능을 처리(구성) 하는 클래스(Abstraction) 와 구현을 담당하는 클래스(Implementation)로 구별한다. 
  * Abstraction클래스를 호출하여 기능을 처리하는데, 이 기능을 Abstraction클래스의 메서드 내에서 Implementation클래스를 이용하여 구현한다.

  


> **기능 클래스 계층(Abstraction)     ||=========||    구현클래스 계층(implementation)**
>
> 
>
> * **기능 클래스 계층과 구현 클래스 계층 사이에 다리를 놓는다**
> * 하나의 계층 구조일 때 보다 각기 나누었을 때 독립적인 계층 구조로 발전 시킬 수 있다.

  




브릿지  패턴이 해결하려고 하는 문제는 다음과 같다

  


> 하나의 추상적 개념이 여러가지 구현으로 구체화 될 수 있을 때 대부분은 상속을 통해서 이 문제를 한다.
>
> 추상 클래스로 추상적 개념에 대한 인터페이스를 정의하고 구체적인 서브클래스들에서 서로 다른 방식으로 이들 인터페이스를 구현한다.
>
> 그러나 상속은 구현과 추상적 개념을 영구적으로 종속시키기 때문에 추상적 개념과 구현을 분리해서 사용하거나 수정 확장하기가 쉽지 않다. 상속이 가진 단점을 그대로 가져가기 때문이다. 
>
> 브릿지 패턴은 추상적 개념에 해당하는 클래스 계층과 구현에 해당하는 클래스 계층을 분리함으로써 문제를 해결한다. 
>
> 즉, 
>
> "구현부에서 추상층을 분리하여 각자 독립적으로 변형이 가능하고 확장이 가능하도록 한다. 
>
>  `브리지 패턴은 상속에서 객체 합성으로 전환하여 이 문제를 해결하려고 시도하는것`

  


###  브리지 패턴은 언제 사용하면 좋을까?

- 부모 추상 클래스가 기본 규칙 세트를 정의하고 구체적인 클래스가 추가 규칙을 추가하고 싶은 경우
- 객체에 대한 참조가 있는 추상 클래스가 있고 각 구체적인 클래스에서 정의될 추상 메서드가 있는 경우

  


## 다이어그램

<img src="https://blog.kakaocdn.net/dn/c3TABr/btrSOxcG28Z/rLlxBbg92GkP1UMknEEUj0/img.png" width = 80% height=300>



- **Abstraction** : 추상적인 부분. 
  - 기능 계층의 최상위 클래스
  - 추상화 개념의 상위 클래스이고 객체 구현자(Implementation)에 대한 참조를 갖고있는다.
  - 구현 부분에 해당하는 클래스의 인스턴스를(Implementation의 구현체) 가지고 구현된 메서드를 호출한다




- **RefinedAbstraction** : 추상적인 부분인 Abstraction의 확장된 기능을 정의
  - 기능 계층에서 새로운 부분을 확장한 클래스이다.
  - Abstraction에 의해 정의된 인터페이스를 확장한다. (extends)




- **Implementation** :구체적인 부분
  -  Abstraction의 기능을 구현하기 위한 인터페이스를 정의한다.
  - Abstraction이 이 인터페이스의 참조를 가지고 메소드를 호출하여 기능을 만든다. 
    - 즉, Abstraction 클래스 매서드 내에서 요청을 Implementor 객체에 전달

  -  하위 클래스가 구현해야 하는 기능들을  공통적인 연산의 시그니처만을 정의한다.




- **ConcreteImplementor** : Implementation에 선언된 기능을 구현한다.
  - 실제 기능을 구현한다. 
  - 여러가지 방식이 나올 수 있다.



## 구현 방법 - Code

#### Abstraction

```java
interface Abstraction {
	String operation() ;
}
```

####  RefinedAbstraction

```java
class RefinedAbstraction implements Abstraction {
	private Implementation imple;
	
	public String operation() {
		return "RefinedAbstraction opertation. call function Implementation " + imple.operationImpl();
	}
	
	public RefinedAbstraction(Implementation imple) {
		this.imple = imple;
	}
}
```

#### Implementor

```java
interface Implementation {
	String operationImpl();
}
```

#### ConcreteImplementor

```java
class ConcreteImplementation implements Implementation {
	
  public String operationImpl() {
		return "나는 그냥 일반적인 기능 1이야";
	}
  
}

class OtherImplementation implements Implementation {
	
  public String operationImpl() {
		return "나는 다른기능이야. ";
	}
  
}

```

#### Main

```java
public class Main{
	public static void main(String[] args) {
		Abstraction abstraction = new RefinedAbstraction(new ConcreteImplementation());
		System.out.println(abstraction.operation());
    
    // 같은 기능을 호출하지만 내부적으론 다른 기능을 사용하고 싶다면
    Abstraction abstraction = new RefinedAbstraction(new OtherImplementation()); 
    System.out.println(abstraction.operation());
	}
}
```



1. 클라이언트는 Abstraction의 구현체를 이용해서 기능을 호출한다.
2.  Abstraction는 자기가 가진 기능과, Implementation가 가진 기능을 이용하여 새 기능을 구성한다
3. Implementation는 특정한 기능을 가지고 있으며,  이 기능은 Implementation의 서브 클래스들에 의해 다양한 기능들이 구성될 수 있다. Implementation는 기능을 구현한다.
4.  Abstraction는 Implementation의 참조를 가지고 있으며, 다양한 Implementation의 구현체들을 이용하여 기능을 구성하고 수행한다.

5. 추상적인 기능을 제공하는 클래스는 수정하지 않되, 다른 기능을 사용하고 싶다면 Implementation의 구현체만 갈아 끼우면 된다.







## 브릿지 패턴(Bridge Pattern) 사용 예시(적용)

- 추상적 개념과 이에 대한 구현 사이의 지속적인 종속 관계를 피하고 싶을 때 사용
  - 같은 기능을 호출하지만, 내부에서 처리되는 방법을 바꾸고 싶은경우.  
  - 이를테면, 런타임에 구현 방법을 선택하거나 구현 내용을 변경하고 싶을 때가 여기에 해당한다.
  - 추상화 클래스(Abstraction) 내부의 구현 객체를 바꿀 수 있으며, 그렇게 하려면 Implementation에 다른 구현체를 할당하기만 하면 된다. 



- 추상적 개념(Abstraction)과 구현(Implementation) 모두가 독립적으로 서브클래싱을 통해 확장되어야 할 때. 
  - 브릿지 패턴은 개발자가 구현을 또 다른 추상적 개념과 연결할 수 있게 할 뿐 아니라,  Abstraction과 Implementation 각각을 독립적으로 확장 가능하게 한다.
- 개발자가 어떤 기능의 여러 변형을 가진 클래스를 나누고 설계할 때 사용
  -  (예: 클래스가 다양한 데이터베이스 서버들과 작동할 수 있는 경우 - JDBC API DriverManger와 Driver ).
- 추상적 개념에 대한 구현 내용을 변경하는 것이 다른 관련 프로그램에 아무런 영향을 주지 않아야 할 때
  - Implementation만 교체하면 된다. 



## 장점 / 단점



### 장점 

* 플랫폼에 독립적인 클래스들과 앱들을 만들 수 있다.
* 클라이언트 코드는 상위 수준의 추상화를 통해 작동하며, 플랫폼의 종류와는 상관 없다.
* 추상적인 코드를 구체적인 코드 변경 없이도 독립적으로 확장할 수 있다. - 개방 폐쇄 원칙 (OCP)
* 추상적인 코드와 구체적인 코드를 분리했기 때문에 제각각 본인이 할 일만 관리하면 된다.
  * 즉 추상화개념의 논리와 구현의 세부 정보에 집중할 수 있다. - 단일 책임 원칙 (SRP)
* 클래스를 설계하고 계층을 분리할 때 인터페이스를 사용하므로. 이를 통해 클래스에서 구현과 추상 부분 2개의 계층으로 분리할 수 있고, 분리된 2개의 추상 계층과 구현 계층은 독립적인 확장이 가능하므로 각기 다르게 개발할 수 있다.

### 단점

* 추상화를 통해 코드를 분리할 경우 계층 구조가 늘어나 코드 디자인 설계가 복잡해진다는 단점이 있다.





### 실무에서 어떻게 쓰이나?
* 자바
  * JDBC API, DriverManger와 Driver
  * SLF4J, 로깅 퍼사드와 로거

*  스프링
  *  Portable Service Abstraction



## 브릿지 패턴과 어댑터 패턴의 차이 (Bridge vs Adaptor)

두 패턴 모두 Interface의 detail을 감추고자 하며, 구조적인 차이가 없다.

* 둘 다 합성을 기반으로 하기 때문 (Composite)

   




하지만 두 패턴은 목적의 차이가 있다.

  


* **어댑터 패턴** : 일치하지 않는 인터페이스를 가져 호환성이 없는 객체들 어댑터를 중간에 두어 맞춰주는 것 
  * 어댑터는 어떤 코드에 맞게끔 기존의 코드를 쓰기 위해 (재사용도 포함) 사용된다.
* **브릿지 패턴** : 추상과 구현을 분리하는 것 (추상 클래스와 구현의 변경이 서로 영향을 주지 않도록 한다) 
  * 추상 클래스는 추상 클래스 대로, 구현은 구현 대로 변경해도 서로 영향을 주지 않도록 한다.
  * 브릿지는 확장성을 고려하여 미리 예상하여 bridge class를 구현해 코드 작성시 사용

  




## 다른 패턴과의 관계

- [브리지](https://refactoring.guru/ko/design-patterns/bridge)는 일반적으로 사전에 설계시 사용되며, 다양한 부분을 독립적으로 개발할 수 있도록 한다. 반면에 [어댑터](https://refactoring.guru/ko/design-patterns/adapter)는 일반적으로 기존 앱과 사용되어 원래 호환되지 않던 일부 클래스들이 서로 잘 작동하도록 한다.
- [브리지](https://refactoring.guru/ko/design-patterns/bridge), [상태](https://refactoring.guru/ko/design-patterns/state), [전략](https://refactoring.guru/ko/design-patterns/strategy) 패턴은 매우 유사한 구조로 되어 있으며, [어댑터](https://refactoring.guru/ko/design-patterns/adapter) 패턴도 이들과 어느 정도 유사한 구조로 되어 있다. 위 모든 패턴은 다른 객체에 작업을 위임하는 합성을 기반으로 한다(Composite). 
  - 하지만 이 패턴들은 모두 다른 문제들을 해결한다. 
  - 패턴은 특정 방식으로 코드의 구조를 짜는 레시피에 불과하다.

- [추상 팩토리](https://refactoring.guru/ko/design-patterns/abstract-factory)를 [브리지](https://refactoring.guru/ko/design-patterns/bridge)와 함께 사용할 수 있다. 
  - 이 조합은 *브리지*에 의해 정의된 어떤 추상체들이 특정 구현체들과만 작동할 수 있을 때 유용하다. 
  - 이런 경우에 *추상 팩토리*는 이러한 관계들을 캡슐화하고 클라이언트 코드에서부터 복잡성을 숨길 수 있다.

- [빌더](https://refactoring.guru/ko/design-patterns/builder)를 [브리지](https://refactoring.guru/ko/design-patterns/bridge)와 조합할 수 있다. 
  - 디렉터 클래스는 추상화의 역할을 하고 다양한 빌더들은 구현의 역할을 한다.




  


### 참조



* https://refactoring.guru/ko/design-patterns/bridge
* [백기선님 강의](https://www.inflearn.com/course/%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4)
*  https://www.crocus.co.kr/1537
* https://has3ong.github.io/programming/designpattern-bridge/
