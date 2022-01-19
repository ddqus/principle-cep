# CEP 5 - 생성자 하나를 주 생성자로 만드세요

### Header

* Reference
    * 엘레강트 오브젝트 1.2, 24p
* Status: Draft

### 규약

* 주(primary) ctor이 초기화를 책임진다.
* 부(secondary) 는 주 ctor을 통해 초기화 해야한다. 부 ctor는 프로퍼티에 접근하면 안된다.
* ctor 의 순서는 주 ctor이 마지막이 되어야 한다.

#### 권장

* 2~3개의 메서드와 5~10개의 ctor 을 포함하는 것이 적당합니다.
* ctor 에 메서드 오버로딩을 적극적으로 사용하자.

### 정리

* ctor 은 constructor 의 줄임말입니다.
* 올바르게 클래스를 설계한다면, 클래스는 많은 수의 ctor과 적은 수의 메서드를 포함할 것입니다.
* 다시 강조하겠습니다. **ctor 의 수가 메서드의 수보다 더 많아집니다**.
* 2~3개의 메서드와 5~10개의 ctor 을 포함하는 것이 적당합니다.
* ctor 의 개수가 더 많을수록 클래스는 더 개선되고, 사용자 입장에서 클래스를 더 편하게 사용할 수 있습니다.
    * ctor 이 많아질수록 클래스를 더 유연하게 사용할 수 있게 됩니다.
    * 하지만 메서드가 많아질수록 클래스를 사용하기는 더 어려워집니다.
    * 메서드가 많아지면 **클래스의 초점이 흐려지고**, SRP 를 위반하기 때문입니다.
* ctor 의 주된 작업은 제공된 인자를 상요해서 캡슐화 한 프로퍼티를 초기화하는 일입니다.
    * 이런 초기화 로직을 단 하나의 ctor에만 위치시키고, 주(primary) ctor 로 부르기를 권장합니다.
    * 부(secondary) ctor 이라고 부르는 다른 ctor 이 주 ctor 을 호출해야 합니다.

```java
class Cash {
  private int dollars;

  Cash(float dlr) {
    this((int) dlr);
  }

  Cash(String dlr) {
    this(Cash.parse(dlr));
  }

  Cash(int dlr) {
    this.dollars = dlr;
  }
}
```

* 주 ctor 을 부 ctor 뒤에 위치시키는데, 주된 이유는 유지보수성 때문입니다.
    * 마지막 ctor 로 스크롤을 내렸을 때 그곳에 항상 주 ctor 이 있다면 더 편합니다.
* String 의 경우 private static 메서드 Cash.parse() 를 호출합니다.
    * java 의 ctor 에서는 this()를 호출하기 전까지는 인스턴스 메서드를 호출할 수 없기 떄문에 static 메서드를 사용해야 합니다.

* 이 원칙의 핵심은 무엇일까요? 유지보수성이 향상된다는 점입니다. 비교해보겠습니다.

    ```java
    class Cash {
      private int dollars;
    
      Cash(float dlr) {
        this.dollars = (int) dlr;
      }
    
      Cash(String dlr) {
        this.dollars = Cash.parse(dlr);
      }
    
      Cash(int dlr) {
        this.dollars = dlr;
      }
    }
    ```

    * dollars 의 값이 항상 양수여야 한다면?
    * 모든 ctor 에서 검사 로직을 작성해야 합니다.
* 메서드 오버로딩을 사용하면 메서드를 비즈니스 언어와 유사하게 만들 수 있기 때문에 **코드의 가독성을 극적으로 향상**시킬 수 있습니다.
    * content(File) 와 contentInCharset(File, Charset) 이라는 이름 대신 content(File) 와 content(File, Charset) 이라는 이름을 사용하면 코드가
      더 간결해집니다.
* 핵심은 유지보수성입니다.
