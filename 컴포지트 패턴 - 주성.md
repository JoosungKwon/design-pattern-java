## Composite?

<img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/a18c6942-10e6-4a9e-baa2-a5894e13f171/SmartSelectImage_2022-11-11-01-52-49.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221112T041734Z&X-Amz-Expires=86400&X-Amz-Signature=48032e1a43c273e6b74e90b295e9d63e68f6e85a86ad9f319a4222119b30d825&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22SmartSelectImage_2022-11-11-01-52-49.png%22&x-id=GetObject">

**Composite이란 단어는 여러 개의 객체(사물,등)들로 구성 된 복합체를 의미**

## 디자인 패턴의 Composite Pattern?

<aside>
💡 Composite pattern은 객체들을 트리 구조들로 구성한 후, 이러한 구조들과 개별 객체들처럼 작업할 수 있도록 하는 구조 패턴

</aside>

**구조 패턴**

- 클래스나 객체를 조합해 더 큰 구조를 만드는 패턴
- 객체를 묶어 단일 인터페이스를 제공하거나 객체들을 서로 묶어 새로운 기능을 제공하는 패턴

**구조 패턴의 특징**

- 서로 독립적으로 개발한 클래스 라이브러리를 마치 하나인 것처럼 사용할 수 있다.
- 여러 인터페이스를 합성하여 서로 다른 인터페이스들의 통일된 추상을 제공한다.
- 인터페이스나 구현을 복합하는 것이 아니라 객체를 합성하는 방법을 제공한다.

<img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/f74af53d-96e9-4ad3-a63a-5b1a335b5735/SmartSelectImage_2022-11-11-01-44-51.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221112T041346Z&X-Amz-Expires=86400&X-Amz-Signature=024439fd4c5826c713d4aaa46dabea4e22050af04435049f800437826c86211e&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22SmartSelectImage_2022-11-11-01-44-51.png%22&x-id=GetObject">

- Component : Leaf 클래스와 전체에 해당하는 Composite 클래스에 공통 인터페이스
- Composite: 복합 객체 그룹을 표현할 클래스로 전체 클래스
- Leaf : 부분 클래스로 단일 객체를 표현

<aside>
✅ 여러 개의 객체들로 구성된 복합 객체와 단일 객체를 클라이언트에서 구별 없이 다루게 해주는 패턴

</aside>

<img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/99701f12-a81e-4237-80b2-3382e3cccc80/SmartSelectImage_2022-11-11-01-45-03.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221112T041414Z&X-Amz-Expires=86400&X-Amz-Signature=0a74cb8a291edc147495d660330109b9e8da33215623486dac3b3366515a399d&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22SmartSelectImage_2022-11-11-01-45-03.png%22&x-id=GetObject">

```java
import java.util.ArrayList;
import java.util.List;

public interface Component {
    int price();
}

class Item implements Component {

    private final String name ;
    private final int price ;

    public Item(String name, int price) {
        this.name = name;
        this.price = price;
    }

    @Override
    public int price() {
        return this.price;
    }
}

class Bag implements Component {
   public List<Component> components;

    public Bag(List<Component> components) {
        this.components = components;
    }

    @Override
    public int price() {
        return components.stream().mapToInt(Component::price).sum();
    }

}
```

```java
import java.util.ArrayList;
import java.util.List;

public class Main {
    public static void main(String[] args) {

        Component keyboard = new Item("키보드",100);
        Component mouse = new Item("마우스",10);
        List<Component> computer = new ArrayList<>();

        computer.add(keyboard);
        computer.add(mouse);

        Component computerSet = new Bag(computer);

        System.out.println(computerSet.price()); // 110 잘 나온다

    }
}

```

<img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/d62b8b59-c3f9-4be5-ae10-449905920b6d/SmartSelectImage_2022-11-11-01-41-04.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221112T041433Z&X-Amz-Expires=86400&X-Amz-Signature=2389b3ba57d6a31e5ddb7c603136843296ee78caf7d3125a1a6c11f73ef239f4&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22SmartSelectImage_2022-11-11-01-41-04.png%22&x-id=GetObject">

재귀적으로 기능을 수행하게 되어 복잡한 객체를 하나의 객체처럼 다룰 수 있는 구조를 가진 클래스를 제공하는 패턴

<img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/c56867d4-3570-453a-bfff-d59616948bac/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221112T041452Z&X-Amz-Expires=86400&X-Amz-Signature=559785dafa8b00da520ee7928d357f729c5997beea175b736b7c5fd3568199b8&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject">

- Composite 패턴은 공통 인터페이스를 공유하는 두 가지 기본 요소 유형들인 `단순 잎(Leaf)`들과 `복합 컨테이너(components)`들로 구성되어 있다.
- 컨테이너는 개별 객체 혹은 다른 컨테이너들로 구성될 수 있으며, 이를 통해 나무와 유사한 중첩된 `재귀 객체 구조`를 구성할 수 있다.
- Composite 패턴은 클라이언트 코드가 `단순 요소들과 복합 요소들을 모두 균일하게 처리`하도록 하고 싶을 때 사용한다.
- 모든 요소들은 공통 인터페이스를 공유하며, 이 인터페이스를 사용하면 `클라이언트는 작업하는 객체들의 구상 클래스에 대해 걱정할 필요`가 없다.

**장점**

- 앱에 새로운 요소 유형들을 도입하여  사용 할 수 있다 → OCP
- 다형성과 재귀를 사용해 복잡한 트리 구조들과 더 편리하게 작업할 수 있다.
- 전체-부분( 혹은 계층)의 관계(Ex. Directory-File)를 갖는 객체들 사이의 관계를 정의(표현)할 때 유용하다.
- 클라이언트가 단일 객체와 집합 객체를 구분하지 않고 동일한 형태로 사용하고자 할 때

**단점**

- 기능이 너무 다른 클래스들에는 공통 인터페이스 를 제공하기 어려울 수 있으며, 어떤 경우에는 컴포넌트 인터페이스를 과도하게 일반화해야 하여 이해하기 어렵게 만들 수 있다.

<aside>
💯 한줄 정리 : 단일 객체와 복합 객체를 같은 타입으로 취급하여  객체들 간의 관계를 트리 구조로 구성할 수 있는 패턴

</aside>

> Decorator 패턴은 Composite 패턴과 비슷하지만, 자식 컴포넌트가 하나만 있습니다. 또 다른 중요한 차이점은 decorator는 래핑된 객체에 추가 책임들을 추가하는 반면 composite은 자신의 자식들의 결과를 '요약'하기만 합니다.
> 

### 참조(Reference)

[복합체 패턴](https://refactoring.guru/ko/design-patterns/composite)

[코딩으로 학습하는 GoF의 디자인 패턴 - 인프런 | 강의](https://www.inflearn.com/course/%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4)

[디자인패턴, Composite Pattern, 콤포짓 콤포지트 패턴](https://youtu.be/XXvrHAsfTso)
