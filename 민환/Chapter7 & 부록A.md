# 07. 함께 모으기

> P.209 커피 예제를 보고 구현해보기 

### 도메인 모델 

![Notes_220607_195817](https://tva1.sinaimg.cn/large/e6c9d24egy1h35ke122hrj20xc0m7ace.jpg)



### 구현 코드 

#### 손님 (Customer.java)

```java
public class Customer {

    private final Barista barista;
    private final Menu menu;

    public Customer(Barista barista, Menu menu) {
        this.barista = barista;
        this.menu = menu;
    }
		
		// 결과값 확인을 위해 반환
    public Coffee order(String menuName) {
        MenuItem menuItem = menu.getMenuByName(menuName);
        return barista.makeCoffee(menuItem);
    }
}
```

#### 메뉴 (Menu.java)

```java
public class Menu {

    private final List<MenuItem> menuItems;

    public Menu(List<MenuItem> menuItems) {
        this.menuItems = menuItems;
    }

    public MenuItem getMenuByName(String menuName) {
        return menuItems.stream()
            .filter(menuItem -> menuItem.isSameName(menuName))
            .findAny()
            .orElseThrow(() -> new IllegalArgumentException(String.format("%s에 해당하는 메뉴가 없습니다.", menuName)));
    }
}
```



#### 메뉴 아이템 (MenuItem.java)

```java
public class MenuItem {

    private final String name;
    private final int price;

    public MenuItem(String name, int price) {
        this.name = name;
        this.price = price;
    }

    public boolean isSameName(String menuName) {
        return name.equals(menuName);
    }

    public String getName() {
        return name;
    }

    public int getPrice() {
        return price;
    }
}
```

#### 바리스타 (Barista.java)

```java
public class Barista {
    
    public Coffee makeCoffee(MenuItem menuItem) {
        return new Coffee(menuItem);
    }
    
}
```

#### 커피(Coffee.java)

```java
public class Coffee {

    private final String name;
    private final int price;

    public Coffee(String name, int price) {
        this.name = name;
        this.price = price;
    }

    public Coffee(MenuItem menuItem) {
        this(menuItem.getName(), menuItem.getPrice());
    }

		// 결과값 확인을 위해 사용
    @Override
    public String toString() {
        return "Coffee{" +
            "name='" + name + '\\'' +
            ", price=" + price +
            '}';
    }

}
```

#### Main.java

```java
public class CoffeeShop {

    public static void main(String[] args) {
        List<MenuItem> menuItems = List.of(
            new MenuItem("아메리카노", 1500),
            new MenuItem("카푸치노", 2000),
            new MenuItem("카라멜 마키야또", 2500),
            new MenuItem("에스프레소", 2500)
        );
        Menu menu = new Menu(menuItems);
        Barista barista = new Barista();
        Customer customer = new Customer(barista, menu);
        Coffee coffee = customer.order("아메리카노");
        System.out.println(coffee);
    }

}
```



### 생각해볼 점 

- 책에서는 각 객체를 메서드 파라미터로 넘겨주었다. 필드로 가지고 있는 것과 파라미터로 넘겨주는 것 어떤 것이 더 좋은 설계일까?
	- 검색과 생각해본 결과 객체의 비즈니스 로직을 많이 사용한다면 필드로 빼는 것이 좋을 것 같다. 즉, 동일한 매개 변수를 여러 메서드에서 사용하는 지 여부를 확인하자!

- get과 find 네이밍의 차이
	- findById ⇒ Optional
	- getRerenceById ⇒ T , throw



### Menu를 enum으로 사용한다면? 

```java
public enum Menu {
    AMERICANO("아메리카노", 1500),
    CAPPUCCINO("카푸치노", 2000),
    CARAMEL_MACCHIATO("카라멜 마키야또", 2500),
    ESPRESSO("에스프레소", 2500);

    private static final Map<String, Menu> MENU_MAP = new HashMap();

    static {
        for (Menu menu : values()) {
            MENU_MAP.put(menu.getName(), menu);
        }
    }

    private final String name;
    private final int price;

    Menu(String name, int price) {
        this.name = name;
        this.price = price;
    }

    public static Menu getMenuByName(String menuName) {
        if (!MENU_MAP.containsKey(menuName)) {
            throw new IllegalArgumentException(String.format("%s에 해당하는 메뉴가 없습니다.", menuName));
        }
        return MENU_MAP.get(menuName);
    }

    public String getName() {
        return name;
    }

    public int getPrice() {
        return price;
    }
}
```

- Map 사용 관련 
	- [Enum 조회 성능 높여보기 - HashMap을 이용해서 빠르게 조회해보자](https://pjh3749.tistory.com/279)

- 장점 

	- enum을 사용하면 MenuItem을 생성할 필요가 없으며 확장하고 싶은 경우 상수에 추가만 해주면 된다.

	- (개인 의견) enum은 정적인 경우에만 사용하는 것이 좋을 것 같다.

- 단점

	- enum이기 때문에 상태가 변화할 수 없다.
		- 아메리카노의 가격을 500원으로 변경해야한다면 변경 후 어플리케이션을 재배포해야함.
	- 상속이나 확장이 일부 제한되기 때문에 유연성이 조금 떨어진다.



### 해결 방안은? 

- 처음 예제처럼 MenuItem 도메인으로 구현 후 데이터베이스에서 처리
	- [Anti-OOP : if 를 피하고 싶어서](http://redutan.github.io/2016/03/31/anti-oop-if)



## 부록 A. 집합과 분해

### 계층적인 복잡성

- 복잡성은 **계층**의 형태를 띈다.
- 단순한 형태로부터 복잡한 형태로 진화하는 데 걸리는 시간은 그 사이에 존재하는 **안정적인 형태**의 수와 분포에 의존한다.

![Notes_220608_135631](https://tva1.sinaimg.cn/large/e6c9d24egy1h35khi414hj20xc0nkgmy.jpg)

- 안정적인 형태의 부분으로부터 전체를 구축하는 행위를 **집합**이라 한다.
- 반대로 전제를 부분으로 분할하는 행위를 **분해**라고 한다.
- 이러한 방식으로 집합을 이루어 생성된 계층 구조는 재귀적인 설계를 가능하게 한다.

> 재귀적인 설계란 무엇을 말하는 것일까?

- 집합은 전체의 내부로 불필요한 세부 사항을 감춰주기 때문에 추상화 메커니즘인 동시에 캡슐화 메커니즘이다.
- 집합은 한 번에 다뤄야 하는 요소의 수를 감소시킴으로써 인지 과부화를 방지한다.

### 합성관계

- 객체와 객체 사이의 전체-부분 관계를 구현하기 위해서는 합성관계를 사용한다.
	- 부분을 전체안에 캡슐화 하여 인지 과부화 방지
	- 주문, 주문항목, 상품이 있다면 주문 안에 주문 항목의 존재를 일시적으로 감춰 복잡성을 낮춘다.
- 연관관계 : 주문 항목과 상품 간에 관계
	- 상품은 주문 항목의 일부가 아니다.
- 합성관계는 연관관계보다 객체를 더 강하게 결합한다.
	- 합성 관계는 포함하는 객체가 제거될 때 내부에 포함된 객체도 함께 삭제
	- 연관 관계는 독립적으로 제거

### 패키지

- 관련된 클래스 집합을 하나의 논리적인 단위로 묶는 구성 요소 (모듈, 패키지)
- 패키지는 내부에 포함된 클래스들을 감춤으로써 시스템의 구조를 추상화한다.