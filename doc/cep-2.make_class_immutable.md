# CEP 2 - 클래스는 불변(immutable) 객체로 만드세요

### Header

* Reference
    * 엘레강트 오브젝트 2.6, p65
* Status: Draft

### 규약

* 클래스는 불변 객체로 만든다.
* Java 에서 클래스의 최대 줄 수는 250줄 이다. (주석, 공백 포함)

### 정리

* 불변 클래스는 이해하기 수월하고, 이해하기 쉬운 코드는 유지보수하기도 쉽습니다.
* 불변 객체는 자기 자신을 수정할 수 없습니다. 원하는 상태를 가지는 새 객체를 생성해서 반환해야 합니다.
* 요지는 가변 객체는 존재해서는 안된다는 점입니다. 가변 객체의 사용을 **엄격하게 금지해야 합니다.**
* 2.6.1 불변 객체에는 식별자 가변성(Identity Mutability) 문제가 없습니다.
    * (e.g. map 에 등록한 객체의 식별자를 동일하게 변경하면 map 은 어떤 객체를 반환할 것인가?)
* 2.6.2 불변 객체에는 실패 원자성(Failure Atomicity)이라는 장점이 있습니다. (2.6.1 보다 중요)
* 2.6.3 불변 객체의 장점은 시간적 결합(Temporal Coupling)을 제거할 수 있습니다. (2.6.2. 보다 중요)
    * 실수로 코드의 순서를 바꿔도(재정렬) 컴파일이 성공한다. (버그가 발생했는데)
    * 코드를 재정렬하기로 결정했다면, 수정하기 전에 우선 코드 줄 사이의 **시간적인 결합을 이해해야 합니다.**
    * 이 상황에서 컴파일러는 아무 도움이 안됩니다. 어떤 줄이 앞에 있어야 하고, 어떤 줄이 뒤에 있어야 하는 지를 기억하는 일은 전적으로 (컴파일러가 아닌) 프로그래머의 몫입니다.
    * 불변성을 활용하면 코드 전반적으로 구문 사이에 존재하는 시간적인 결합을 제거할 수 있습니다.
* 2.6.4 부수효과 제거(Side effect-free) (2.6.3 보다 중요)
    * 객체를 수정하지 않으니 부수효과가 발생할 일이 없다.
* 2.6.5 NULL 참조 없애기 (2.6.4 보다 중요)
    ```java
    class User {
      private final int id;
      private String name;
      public User(int num){
        this.id = num;
      }
      public void setName(String name){
        this.name = name;
      }
    }
    ```
    * 초기화 되지 않은 name 프로퍼티를 가지고 있는 User 객체가 필요한 이유는 무엇일까요? 언제 그리고 왜 이런 객체가 필요한 것일까요?
    * 제가 생각하는 답은, 다른 클래스가 필요하지만 새로운 클래스를 생성하는 작업이 **귀찮게** 여겨질 때 입니다. 또는 그 클래스를 어떻게 만들어야 할지 모를 수도 있습니다. 또는 OOP 에서 그 클래스가 무엇을
      의미하는 지를 제대로 이해하지 못해서일 수도 있습니다.
    * 결과는 동일합니다. 매우 큰 클래스를 만들게 된다는 것입니다.
    * 커다란 클래스를 만드는 이유는, 문제를 더 작은 부분으로 분해하기 위해 상속과 캡슐화를 어떻게 사용하는 지를 **모르기 때문입니다**. 이것이 같은 클래스를 재사용하는 이유입니다.
    * 모든 객체를 불변으로 만들면 객체안에 NULL 을 포함시키는 것이 애초에 불가능해집니다.
* 2.6.6 스레드 안전성 (2.6.5 보다 중요)
    * 어떤 스레드도 객체의 상태를 수정할 수 없기 때문에 아무리 많은 스레드가 객체에 접근해도 문제가 없습니다.
    * 명시적인 동기화(synchronized) 를 이용하면 가변 클래스 역시 스레드에 안전하게 만들 수는 있습니다.
    * 하지만 각 스레드는 객체를 **배타적으로 잠그기 때문에** 다른 스레드는 기다릴 수밖에 없습니다.
* 2.6.7 더 작고 더 단순한 객체 (가장 중요)
    * 객체가 더 단순해질 수록 응집도는 더 높아지고 유지보수는 더 쉬워집니다. 최고의 소프트웨어는 단순합니다.
    * Java 에서 허용 가능한 클래스의 최대 크기는 주석과 공백을 포함해서 **250줄 정도**라고 생각합니다.
    * 저는 이 주장이 가장 강력하다고 생각합니다. 불변성은 클래스를 더 깔끔하고 더 짧게 만듭니다. 이것이 불변 클래스의 가장 중요한 장점입니다.