# 클래스
- 클래스 : 비슷한 성질을 가진것(들)
- 객체 : 클래스가 구체화 된 것  

#### 클래스
클래스의 선언은 곧 **"클래스 안의 로직과 데이터는 함께 사용되며 클래스 내부의 데이터는 관련 로직에 의해 변화하며 클래스 내부에서 사용하는 데이터의 변화율은 비슷하다."** 라는것을 의미한다.


#### 클래스의 이름  
- 적합한 이름은 코드의 가독성을 높여준다.
- 간결성 vs 표현성 trade off
- 정확한 의미를 위해 여러단어로 클래스명을 만들어야할 경우, **메타포(은유)** 를 사용 해 한 단어로 된 클래스명을 지을수 있는지 고민해본다.
- 하위클래스 이름은 상위클래스와 유사점과 차이점을 나타내야한다. 따라서 어느정도의 간결성보다는 표현성을 택하는것이 좋다 보통 상위클래스 이름에 한두개의 수식어를 붙여서 하위클래스 이름을 정하지만, 하위클래스가 그 자체로 프로그램의 중요한 개념을 의미하는 경우라면 하위클래스에 단순한 이름을 사용해야 한다.


## 추상 인터페이스
소프트웨어는 유연해야하지만 유연성에는 비용이 들고, 어떤 부분에서 유연성이 필요할지 예측하기란 쉽지 않다. 따라서 <u>실제 필요해지는 경우에만</u> 시스템에 유연성을 부여하는것이 좋다. 

### 인터페이스
필요한 기능을 명시한 후 이후의 그 기능의 구현에는 관여하지 않을때 사용한다.  
인터페이스를 사용하면 구현을 바꾸기는 용이하지만, 인터페이스 자체를 바꾸기는 쉽지 않다. 인터페이스를 구현한 모든 구현체를 수정해야하기 때문이다.  
### 인터페이스 naming
- 일반적인 상위클래스/하위클래스 네이밍 방법을 따를 수 있다.
- 상황에 따라 인터페이스 사용을 드러내는 이름("I"를 붙이는등)이나 상속이나 구현등을 표현하지 않는 이름을 사용하지 않거나! 

### 추상 클래스
인터페이스가 변경에 취약하다면, 추상클래스는 기본 구현을 사용할 수 있다면, 기존 설계를 망가뜨리지 않고 새로운 연산을 얼마든지 추가할 수 있다는 장점이 있다.
하지만 각 클래스는 단 하나의 상위클래스만 지정할 수 있다.
최상위에 위치한 클래스를 인스턴스화 해서 사용할 가능성이 조금이라도 있다면 그렇게 해야한다. 추상클래스를 무분별하게 많이 만들게 되면 코드 복잡도가 높아진다.


### 버전인터페이스
인터페이스를 바꾸고 싶은데 그렇게 하지 못할경우, 새로운 인터페이스를 선언해서 기존 인터페이스를 확장한 후 새로운 연산을 추가할 수 있다.  
새로운 기능을 사용하기를 원하는 사용자는 확장된 인터페이스를 사용하고,  
기존 사용자는 새로운 인터페이스에 대해 알 필요가 없다.  
> 새로운 연산 사용할때 반드시 객체의 타입을 확인한 수 새로운 타입으로 **다운캐스트** 해서 사용해야 한다!

```java
interface  Command {
 void run();
}
```
```java
interface ReversibledCommand extends Command {
  void undo();
}
```

```java
Command recent = ...;
if(recent instanceof ReversibleCommand) {
  ReversibleCommand downcasted = (ReversibleCommand) recent;
  downcasted.undo();
}
```
보통 instanceof는 코드가 특정 클래스에 제한되므로 유연성이 떨어져 권장하지 않지만, 이 경우에는 인터페이스의 개선을 위한 정당한 사용이 된다.

### 값 객체
- 값 스타일 객체 : 변화하는 상태를 지닌 객체가 아닌 객체, 값 스타일의 객체에서는 생서자에서만 모든 상태를 설정할 뿐, 다른 경로를 통해 필드값을 변경하면 안된다.  
일시적이더라도 고정적인 상황에서는 함수형 스타일이 적절하고,  
상황이 변하는 경우라면 상태를 사용하는 것이 좋다.  
값 스타일의 객체를 다루는 연산은 언제나 새로운 객체를 반환하고, 연산을 요청한 쪽에 저장한다. 

가능하면 변화하는 상태를 가진 객체와 값 스타일의 객체를 적절히 섞어서 사용하는 것이 좋다.

### 특화
코드의 유사점과 차이점을 부각시키면 코드 독자가 명확히 코드를 이해하고, 이존 코드가 새로운 요구사항을 처리 할 수 있는지 판단할 수 있다. 
또 수정이 필요한 경우 기존 코드를 변형해 특화하는 것이 좋을지 새로운 코드를 짜는것이 좋을지 판단 할 수있다.

### 하위클래스
하위클래스의 선언은 "이 객체는 상위클래스와 갔다. 이 부분만 제외하면..."과 같은 의미이다.
공통된 구현을 갖는 경우에 효과적이다.

- 단점  
1.하위클래스를 사용하변 변형이 쉽지 않다.
2.하위클래스의 이해를 위해 상위클래스의 이해가 선행되어야 한다.  
3.하위클래스가 상위클래스 세부구현에 의존할수 있으므로 상위클래스긔 수정이 어려워진다.   
4.병렬클래스 계층을 이요하는 경우 : 하나의 클래스 계층에 있는 각 하위클래스에 대해 다른 클래스 계층에 대응되는 하위 클래스가 필요하다.
5.동적으로 변화하는 로직은 표현불가(하위 클래스의 객체를 생성할때 그 객체의 목적을 알아야 하며 그 이후에는 바뀔 수 없다.)

하위클래스를 잘 사용하기 위한 방법으로는 상위클래스의 로직을 여러개의 메소드로 쪼개는 방법이 있다. 
하위클래스 작성시 각 메소드별로 오버라이드가 가능해진다. 


## 구현자 
객체지향 프로그램에서는 선택을 표현하기 위해 주로 다형적 메세지를 사용하고, 이런 메세지로 표현하기 위해서 여러 종류의 다양한 객체가 메세지를 받아 처리한다. 


### 내부클래스 
어떤 연산을 표현하기위한 클래스가 필요하지만 새로운 파일을 만들고 싶지 않을 때 내부클래스를 사용한다. 
클래스 사용에 따른 비용이 안들면서 클래스의 장점을 취할수 있다. 
- 내부클래스는 내부클래스를 감싼 클래스에 대한 정보를 암묵적으로 전달받는다. 클래스간의 관계를 명시적으로 정하지 않으면서 감싼 클래스의 데이터에 접근할 수 있어 유용하다. 
- 내부 클래스는 인자없는 생성자를 가질수 없다.(명시적으로 선언해도 불가능!)
- 생성 클래스의 인스턴스와 완전히 분리된 내부클래스를 원한다면 내부클래스를 static으로 선언하면 된다.


### 조건문  
인스턴스별 행동의 형태에는 if/then과 switch문이 있다. 
- 인스턴스별 행동을 지원하면서도 모든 로직이 하나의 클래스 안에 있다. 
- 인스턴스 행동을 변경하기 위해 해당 클래스 코드를 고쳐야한다. 
- 수행경로가 다양해질 수록 버그발생률이 높아진다.  -> 프로그램의 안전성이 떨어진다. 
- 중복되는 조건부 로직이나 분기문의 결과에 따라 로직이 달라진다면, 명시적인 로직보다는 메세지를 사용하는 것이 좋다.

### 위임
공통으로 사용되는 로직은 위임클래스를 참조하는 클래스에 들어있고, 변형은 여러 객체에 각각 구현할 수 있다. 


### 플러그인 선택자   
한두개의 메소드에서만 인스턴스별 행동이 필요하고, 모든 로직이 하나의 클래스 안에 들어가도 좋을때 메소드 이름을 필드에 저장해두고 리플렉션을 통해 메소드를 호출하는것도 좋다. 
- 단일클래스만으로 여러개의 테스트 구현이 가능해진다. 
- 프로그램 어딘가에서 플러그인 선택자를 사용하기 때문에 코드를 호출하거나 지울 수 없다.

### 익명 내부클래스
한곳에서만 사용되는 클래스를 생성해서 일부 메소드를 오버라이드한 후, 지역적으로만 사용하는 클래스로, 특정 지역에서만 사용되므로 이름이 필요 없다.    
익명 내무 클래스 코드는 내부 클래스를 사용하는 클래스의 코드를 읽는데 방해가 되므로 가급적 짧아야 한다.

### 라이브러리 클래스  
인스턴스화가 불가능한, 라이브러리 메소드만을 갖고 있는 클래스로 어떤 객체에도 적합하지 않은 기능을 구현하기 위해 정적 메소드를 구현하기위해 사용한다.  
정적메소드는 객체지향 프로그래밍의 장점(객체조립을 통한 데이터 공유)을 활용할수 없어진다. 따라서 가능한 라이브러리 클래스는 객체로 변환하는 것이 좋다.
