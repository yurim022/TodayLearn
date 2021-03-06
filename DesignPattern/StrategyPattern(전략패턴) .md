# 전략 패턴(strategy pattern)

### 특정 컨텍스트에서 알고리즘을 별도록 분리하는 설계 방법

실행 중에 알고리즘을 선택할 수 있게 하는 행위 소프트웨어 디자인 패턴. 

알고리즘들을 정의하고 캡슐화하여 이 알고리즘들을 해당 계열 안에서 상호 교체가 가능하게 만든다. 

유연하고 재사용 가능한 객체지향 소프트웨어

> 전략패턴 적용1 예시
> 

한 과일매장은 상황에 따라 다른 할인정책을 적용하고 있다. 

제일 먼저온 손님에게 10%, 신선도가 떨어진 과일에 대해서 20%, 마지막 손님 20% 할인을 제공한다. 

### 전략패턴 미적용 코드

```java
public class Calculator {
    
    	public double calculate(boolean isFirstGuest, boolean isLastGuest, List<Item> items) {
    		double sum = 0;
    		for (Item item : items) {
    			if (isFirstGuest) {
    				sum += item.getPrice() * 0.9;
    			} else if (!item.isFresh()) {
    				sum += item.getPrice() * 0.8;
    			} else if (isFirstGuest) {
    				sum += item.getPrice() * 0.8;
    			} else {
    				sum += item.getPrice();
    			}
    		}
    		return sum;
    	}
    }
    
    public class Item {
    	private final String name;
    	private final int price;
    
    	public Item(String name, int price) {
    		this.name = name;
    		this.price = price;
    	}
    
    	public int getPrice() {
    		return price;
    	}
    
    	public boolean isFresh() {
    		return true;
    	}
    }
```

위 코드의 문제점

- if-else 분기처리. 하나의 메소드에 너무 많은 확인 로직이 추가된다.
- 변경에 유연하지 않다.
    
    새로운 가격정책이 추가되었을 때 기존의 코드를 복붙해야함. 두가지 문제점
    
    1. 실제 프로그램에서 복잡한 로직이 추가되면 복붙이 어렵다 
    
     2.  실수로 동일 변수명을 사용해서 오류 발생 가능하다
    
- 시간이 지날수록 코드 분석이 어려워진다.

### 전략패턴 적용 코드1

할인 알고리즘을 DiscountPolicy 라는 인터페이스를 통해 분리하여 관리한다. 

```java
public interface DiscountPolicy {
    	double calculateWithDisCountRate(Item item);
    }
    
    public class FirstCustomerDiscount implements DiscountPolicy{
    	@Override
    	public double calculateWithDisCountRate(Item item) {
    		return item.getPrice() * 0.9;
    	}
    }
    
    public class LastCustomerDiscount implements DiscountPolicy{
    	@Override
    	public double calculateWithDisCountRate(Item item) {
    		return item.getPrice() * 0.8;
    	}
    }
    
    public class UnFreshFruitDiscount implements DiscountPolicy{
    	@Override
    	public double calculateWithDisCountRate(Item item) {
    		return item.getPrice() * 0.8;
    	}
    }
```

이를 기존의 Calculator 클래스에서 생성자를 통해 필요한 하위 타입을 주입받아 사용한다. (Dependency Ingection)

```java
public class Calculator {
    
    	private final DiscountPolicy discountPolicy;
    
    	public Calculator(DiscountPolicy discountPolicy) {
    		this.discountPolicy = discountPolicy;
    	}
    
    	public double calculate(List<Item> items) {
    		double sum = 0;
    		for (Item item : items) {
    			sum += discountPolicy.calculateWithDisCountRate(item);
    		}
    		return sum;
    	}
    }
```

아래 첫번째 손님 할인정책을 구현하는 코드이다. 일반적으로 Controller는 사용자의 요청(클릭이나 입력) 등을 매핑하여 받아오기 때문에 특정 알고리즘(첫번째 손님 계산)을 눌렀다는 것을 알 수 있다. 

```java
public class FruitController {
    	public static void main(String[] args) {
    		Calculator calculator = new Calculator(new FirstCustomerDiscount());
    		calculator.calculate(Arrays.asList(
    			new Item("Apple", 3000),
    			new Item("Banana", 3000),
    			new Item("Orange", 2000),
    			new Item("Pitch", 4000)
    		));
    	}
    }
```

## 전략패턴의 장점

- 컨텍스트 코드의 변경 없이 새로운 전략을 추가할 수 있다.
- 때문에 OCP 의 원칙을 준수할 수 있다.

## 전략패턴의 단점

- 컨텍스트에 적용되는 알고리즘이 하나이거나 두개인 경우는 분기를 타는 것이 편한 경우도 있다.

> 전략패턴 적용2
> 

![image](https://user-images.githubusercontent.com/45115557/180704375-c0bf2ea3-69cc-4a9f-b5d6-a4bc3c1e0603.png)


위에서 Main에서 알고리즘을 선택하고 그 알고리즘이 객체를 받아 수행하는 것이였다면, 

전략패턴을 사용하는 또 다른 방법은 객체를 abstract로 선언하여 '행위'(변동 가능성 있는 알고리즘)를  상속받는 객체마다 다르게 적용하는 것이다. 

위 그림에서는 interface 형태의 Strategy 라는 전략을 인자로 가지며, abstract을 구현한 Class1, Class2 에서 setter를 통해 strategy구현 클래스를 적용한다.   

### Abstract class

Robot class

```java
public abstract class Robot {
private String name;
private AttackStrategy attackStrategy;
private MovingStrategy movingStrategy;

public Robot(String name) { this.name = name; }
public String getName() { return name; }
public void attack() { attackStrategy.attack(); }
public void move() { movingStrategy.move(); }

// 집약 관계, 전체 객체가 메모리에서 사라진다 해도 부분 객체는 사라지지 않는다.
// setter 메서드
public void setAttackStrategy(AttackStrategy attackStrategy) {
  this.attackStrategy = attackStrategy; }
public void setMovingStrategy(MovingStrategy movingStrategy) {
  this.movingStrategy = movingStrategy; }
}

```

### extends한  Robot 클래스

```java
public class TaekwonV extends Robot {
public TaekwonV(String name) { super(name); }
}
public class Atom extends Robot {
public Atom(String name) { super(name); }
}
```

### Strategy 구현

AttackStrategy

```java
// 인터페이스
interface AttackStrategy { public void attack(); }
// 구체적인 클래스
public class MissileStrategy implements AttackStrategy {
  public void attack() { System.out.println("I have Missile."); }
}
public class PunchStrategy implements AttackStrategy {
  public void attack() { System.out.println("I have strong punch."); }
}
```

```java
// 인터페이스
interface MovingStrategy { public void move(); }
// 구체적인 클래스
public class FlyingStrategy implements MovingStrategy {
  public void move() { System.out.println("I can fly."); }
}
public class WalkingStrategy implements MovingStrategy {
  public void move() { System.out.println("I can only walk."); }
}

```

### 클라이언트 적용

```java
public class Client {
public static void main(String[] args) {
  Robot taekwonV = new TaekwonV("TaekwonV");
  Robot atom = new Atom("Atom");

  /* 수정된 부분: 전략 변경 방법 */
  taekwonV.setMovingStrategy(new WalkingStrategy());
  taekwonV.setAttackStrategy(new MissileStrategy());
  atom.setMovingStrategy(new FlyingStrategy());
  atom.setAttackStrategy(new PunchStrategy());

  /* 아래부터는 동일 */
  System.out.println("My name is " + taekwonV.getName());
  taekwonV.move();
  taekwonV.attack();

  System.out.println()
  System.out.println("My name is " + atom.getName());
  atom.move();
  atom.attack();
}
}

```

> 전략패턴 적용3
> 

함수의 인자로 적용할 패턴을 받는다. 

### 주의사항

위 그림처럼 클래스 내에 strategy interface객체를 가지고 있는 경우, 해당객체는 반드시 abstract 클래스 및 abstract 함수로 구현되어야 한다. 

참고링크

[https://velog.io/@kyle/디자인-패턴-전략패턴이란](https://velog.io/@kyle/%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4-%EC%A0%84%EB%9E%B5%ED%8C%A8%ED%84%B4%EC%9D%B4%EB%9E%80)

[https://gmlwjd9405.github.io/2018/07/06/strategy-pattern.html](https://gmlwjd9405.github.io/2018/07/06/strategy-pattern.html)
