# 01. 협력하는 객체들의 공동체

> 객체지향의 목표는 실세계를 모방하는 것이 아니다. 오히려 새로운 세계를 창조하는 것이다.



## 협력하는 사람들

- 역할
    - **손님 : 커피 주문 (손님 역할)**
    - **캐시어 : 주문 접수 (캐시어 역할)**
    - **바리스타 : 커피 제조 (바리스타 역할)**
- 책임
    - **손님 : 커피 주문 책임**
    - **캐시어 : 주문 접수 책임**
    - **바리스타 : 커피 제조 책임**
- 협력 : **커피 주문**을 위해 모든 사람들이 맡은 역할과 책임을 다하며 **협력**

### **협력**

- 요청
    - **요청은 연쇄적으로 발생한다.**
- 응답
    - **요청의 방향과 반대 방향으로 연쇄적으로 전달된다.**

```java
public class Customer {

    private final Cashier cashier = new Cashier();

    public Coffee order(CoffeeMenu menu) {
        return cashier.takeOrder(menu);
    }
}

public class Cashier {

    private final Barista barista = new Barista();

    public Coffee takeOrder(CoffeeMenu menu) {
        return barista.makeCoffee(menu);
    }
}

public class Barista {

    public Coffee makeCoffee(CoffeeMenu menu) {
        return new Coffee(menu.toString());
    }

}

요청 : Customer -> Cashier -> Barista 
응답 : Barista -> Cashier -> Customer 
```

### **역할과 책임**

- 역할이 있으면 책임도 주어진다.
    - **선생님 역할은 학생을 가르칠 책임이 있다.**
    
- 여러 사람이 동일 역할을 수행할 수 있다.
  
    ```java
    public interface Customer {
        Coffee order(CoffeeMenu menu);
    }
    
    public class Aramand implements Customer {
        ...
    }
    
    public class Spancer implements Customer {
        ...
    }
    
    Customer customer = new Aramand();
    Customer customer = new Spancer();
    ```
    
- 역할은 대처 가능성을 의미
  
    ```java
    public interface Cashier {
        Coffee takeOrder(CoffeeMenu menu);
    }
    
    public class Aramand implements Cashier {
        ...
    }
    
    public class Spancer implements Cashier {
        ...
    }
    
    public class Customer {
    
        private final Cashier cashier = new Spancer();
        private final Cashier cashier = new Aramand();
    
        public Coffee order(CoffeeMenu menu) {
            return cashier.takeOrder(menu);
        }
    }
    ```
    
- 책임을 수행하는 방법은 자율적으로 선택할 수 있다.
    - **동일 요청에 대해 서로 다른 방식으로 응답**
    - **다형성**
    
    ```java
    public interface Barista {
        Coffee makeCoffee(CoffeeMenu menu);
    }
    
    public class Subin implements Barista {
        @Override
        public Coffee makeCoffee(CoffeeMenu menu) {
            return new Coffee(menu.toString());
        }
    }
    
    public class Spancer implements Barista {
        @Override
        public Coffee makeCoffee(CoffeeMenu menu) {
            Coffee coffee = new Coffee(menu.toString());
            coffee.addSuger();
            return coffee;
        }
    }
    ```
    
- 한 사람이 동시에 여러 역할을 수행할 수 있다.
  
    ```java
    public class WooJin implements Cashier, Barista {
    
        @Override
        public Coffee takeOrder(CoffeeMenu menu) {
            return makeCoffee(menu);
        }
    
        @Override
        public Coffee makeCoffee(CoffeeMenu menu) {
            return new Coffee(menu.toString());
        }
    }
    ```
    



<br>

## **역할, 책임, 협력**

- 사람 -> 객체
- 요청 -> 메시지
- 요청을 처리하는 방법 -> 메서드

### **객체의 역할**

> 역할이란 관련성 높은 책임의 집합이다.
> 
- 여러 객체가 동일한 역할을 수행할 수 있다.
- 역할은 대처 가능성을 의미한다.
- 각 객체는 책임을 수행하는 방법을 자율적으로 선택할 수 있다.
- 하나의 객체가 동시에 여러 역할을 수행할 수 있다.



<br>

## **협력 속에 사는 객체**

> 협력에 참여하는 주체는 **객체**이다.
> 
1. 객체는 충분히 **협력적**이어야 한다.
    - 객체는 다른 객체의 명령에 복종하는 것이 아니라 요청에 응답할 뿐이다.
    - 어떤 방식으로 응답할지는 객체 스스로 판단하고 결정하며 요청에 응할지 여부도 스스로 결정할 수 있다.
2. 객체가 충분히 **자율적**이어야 한다.
    - 스스로의 결정과 판단에 따라 행동하는 자율적인 존재이다.

### 상태와 행동을 함께 지닌 자율적인 객체

- 객체의 자율성은 **객체의 내부와 외부를 명확하게 구분하는 것**으로부터 나온다. → **캡슐화**
- 객체는 상태와 행위를 하나의 단위로 묶는 **자율적인 존재**다.
- 자율적인 객체로 구성된다면 유지보수가 쉽고 재사용이 용이해진다.
- 객체의 자율성
    - 데이터 중심 (절차 지향)
      
        ![스크린샷 2022-05-24 오후 5.51.48.png](01%20%E1%84%92%E1%85%A7%E1%86%B8%E1%84%85%E1%85%A7%E1%86%A8%E1%84%92%E1%85%A1%E1%84%82%E1%85%B3%E1%86%AB%20%E1%84%80%E1%85%A2%E1%86%A8%E1%84%8E%E1%85%A6%E1%84%83%E1%85%B3%E1%86%AF%E1%84%8B%E1%85%B4%20%E1%84%80%E1%85%A9%E1%86%BC%E1%84%83%E1%85%A9%E1%86%BC%E1%84%8E%E1%85%A6%20e08ed019d9f3452b8c60fa60c09be59d/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-24_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.51.48.png)
        
    - 객체 중심 (객체 지향)
      
        ![스크린샷 2022-05-24 오후 5.51.55.png](01%20%E1%84%92%E1%85%A7%E1%86%B8%E1%84%85%E1%85%A7%E1%86%A8%E1%84%92%E1%85%A1%E1%84%82%E1%85%B3%E1%86%AB%20%E1%84%80%E1%85%A2%E1%86%A8%E1%84%8E%E1%85%A6%E1%84%83%E1%85%B3%E1%86%AF%E1%84%8B%E1%85%B4%20%E1%84%80%E1%85%A9%E1%86%BC%E1%84%83%E1%85%A9%E1%86%BC%E1%84%8E%E1%85%A6%20e08ed019d9f3452b8c60fa60c09be59d/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-24_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.51.55.png)
        
    
    ### 절차 지향
    
    ```java
    public class Member {
    		... 
        private LocalDate expiryDate;
        private boolean male;
    		...
    
        public LocalDate getExpiryDate() {
            return expiryDate;
        }
    
        public boolean isMale() {
            return male;
        }
    }
    ```
    
    - 만료 여부를 확인 해야 한다면?
      
        ![스크린샷 2022-05-24 오후 6.09.41.png](01%20%E1%84%92%E1%85%A7%E1%86%B8%E1%84%85%E1%85%A7%E1%86%A8%E1%84%92%E1%85%A1%E1%84%82%E1%85%B3%E1%86%AB%20%E1%84%80%E1%85%A2%E1%86%A8%E1%84%8E%E1%85%A6%E1%84%83%E1%85%B3%E1%86%AF%E1%84%8B%E1%85%B4%20%E1%84%80%E1%85%A9%E1%86%BC%E1%84%83%E1%85%A9%E1%86%BC%E1%84%8E%E1%85%A6%20e08ed019d9f3452b8c60fa60c09be59d/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-24_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.09.41.png)
        
    - 여성 회원인 경우 만료 기간이 지났어도 30일 간은 서비스를 사용할 수 있도록 변경되었다면?
      
        ![스크린샷 2022-05-24 오후 6.20.28.png](01%20%E1%84%92%E1%85%A7%E1%86%B8%E1%84%85%E1%85%A7%E1%86%A8%E1%84%92%E1%85%A1%E1%84%82%E1%85%B3%E1%86%AB%20%E1%84%80%E1%85%A2%E1%86%A8%E1%84%8E%E1%85%A6%E1%84%83%E1%85%B3%E1%86%AF%E1%84%8B%E1%85%B4%20%E1%84%80%E1%85%A9%E1%86%BC%E1%84%83%E1%85%A9%E1%86%BC%E1%84%8E%E1%85%A6%20e08ed019d9f3452b8c60fa60c09be59d/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-24_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_6.20.28.png)
        
    - **만료 여부 확인 코드를 사용하는 곳을 모두 수정해야 한다.**
    
    ### 객체 지향
    
    ```java
    public class Member {
    
        private LocalDate expiryDate;
        private boolean male;
    
        public boolean isExpired() {
            return expiryDate != null && expiryDate.isBefore(LocalDate.now());
        }
        
    }
    
    if(member.isExpired()) {
    	// 만료 되었을 때 처리
    }
    ```
    
    - 여성 회원인 경우 만료 기간이 지났어도 30일 간은 서비스를 사용할 수 있도록 변경되었다면?
      
        ```java
        public boolean isExpired() {
            if (male) {
                return expiryDate != null && expiryDate.isBefore(LocalDate.now());
            }
            return expiryDate != null 
                && expiryDate.isBefore(LocalDate.now().minusDays(30L));
        }
        
        if(member.isExpired()) {
        	// 만료 되었을 때 처리
        }
        ```
        
        - **사용하는 곳에서의 수정이 없다.**

 

### 메서드와 자율성

> 메시지와 메서드의 분리는 객체의 협력에 참여하는 객체들 간의 자율성을 증진시킨다.
> 
- 외부의 요청이 무엇인지를 표현하는 메시지와 요청을 처리하기 위한 구체적인 방법인 메서드를 분리하는 것은 객체의 자율성을 높이는 핵심 메커니즘이다.
- **캡슐화**라는 개념과도 깊이 관련돼 있다.

```java
public interface Barista {
    // 메시지 
    Coffee makeCoffee(CoffeeMenu menu);
}

public class Subin implements Barista {

    // 메서드 
    @Override
    public Coffee makeCoffee(CoffeeMenu menu) {
        return new Coffee(menu.toString());
    }
}
```



<br>

## 객체지향의 본질

- 객체지향이란 시스템을 상호작용하는 자율적인 객체들의 공동체로 바라보고 객체를 이용해 시스템을 분할하는 방법이다.
- 자율적인 객체란 상태와 행위를 함께 지니며 스스로 자기 자신을 책임지는 객체를 의미한다.
- 객체는 시스템의 행위를 구현하기 위해 다른 객체와 협력한다. 각 객체는 협력 내에서 정해진 역할을 수행하며 역할은 관련된 책임의 집합이다.
- 객체는 다른 객체와 협력하기 위해 메시지를 전송하고, 메시지를 수신한 객체는 메시지를 처리하는 데 적합한 메서드를 자율적으로 선택한다.

### 객체를 지향하라

> 클래스의 구조와 메서드가 아니라 객체의 역할, 책임, 협력에 집중하라.
> 
- 객체지향의 핵심은 클래스가 아니다.
    - 핵심은 적절한 책임을 수행하는 역할 간의 유연하고 견고한 협력 관계를 구축하는 것이다.
- 객체지향의 중심에는 클래스가 아닌 객체가 위치해야 하며 중요한 것은 클래스들의 정적인 관계가 아니라 메시지를 주고받는 객체들의 동적인 관계이다.