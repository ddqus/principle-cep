# CEP 6 - 퍼블릭 상수(Public Constant)를 사용하지 마세요

### Header

* Reference
    * 엘레강트 오브젝트 2.5, 56p
* Status: Draft

### 규약

* public 상수(public static, constant)는 사용하지 않는다.
  * private 은 사용해도 된다. 객체 외부로 노출이 안되니까.

### 논의

* enum 도 사용하면 안되는가?

### 정리

```java
class Constants {
  public static final String EOL = "\r\n";
}
```

* 어디에서나 접근할 수 있는 퍼블릭 상수를 '재사용' 할 수 있습니다. 문제를 해결한 것처럼 보이나요? 절대 아닙니다.
* 코드 중복이라는 하나의 문제를 해결하기 위해 두 개의 더 큰 문제를 추가하고 말았습니다.
    * 첫 번째 문제는 결합도(coupling)가 높아진 것이고,
    * 두 번째 문제는 응집도(cohesion)가 낮아진 것입니다.

```java
class Records {
  void write(Writer out) {
    out.write(Constants.EOL);
  }
}

class Rows {
  void print(PrintStream pnt) {
    pnt.printf("%s", Constants.EOL);
  }
}
```

* 두 클래스는 Constants 객체에 의존하고 있으며, 이 의존성은 **하드 코딩**되어 있습니다.
* Constants.EOL 의 내용을 수정하면 다른 두 클래스의 행동은 예상할 수 없는 방향으로 변경될 것입니다.
    * 왜 예상할 수 없을까요?
    * Constants.EOL 의 입장에서는 이 값이 어떻게 사용되고 있는지 알 수 없기 때문입니다.
    * HTTP 프로토콜의 콘텐츠 한 줄을 종료하기 위해 이 값을 사용할 수도 있습니다.
    * HTTP를 사용하기 위해서는 의무적으로 지켜야 하는 요구사항이기 때문에 이 규칙을 변경하는 것은 불가능합니다.
        * (사용 의도가 다르다 -> life cycle 이 다르다)

* 응집도 저하
* 낮은 응집도는 객체가 자신의 문제를 해결하는데 덜 집중한다는 것을 의미합니다.
* 객체는 아주 멍청한 상수 위에 **자신만의 의미론**을 덧붙여야 합니다.
    * Constants.EOL 은 자신에 관해 아무 것도 알 지 못하며, 자신의 존재 이유를 이해하지 못하는 하나의 텍스트 덩어리에 불과합니다.
    * 이 상수는 삶의 의미가 명확하지 않습니다.
    * (상수는 데이터일 뿐이며 의미=의도=목적이 배제되어 있다. 갖다 쓰는 쪽에서 의미를 부여하기 때문에 변경에 취약하다)
* 어떤 대안이 있을까요? 객체 사이에 데이터를 중복해서는 안됩니다. 대신 기능을 공유할 수 있도록 **새로운 클래스**를 만들어야 합니다.
    * **데이터가 아니라 기능을 공유해야 합니다.**

```java
class EOLString {
  private final String origin;

  EOLString(String src) {
    this.origin = src;
  }

  String toString() {
    return String.format("%s,\r\n", origin);
  }
}
```

* 접미사를 덧붙이는 기능을 EOLString 클래스 안으로 고립시켰습니다.
    * 접미사를 줄 마지막에 추가하는 방법에 관해서는 EOLString 이 책임집니다.
* Records 와 Rows 는 더 이상 해당 로직을 포함하지 않습니다.
* 우리는 각 줄의 끝에 접미사가 **추가되는 방법**을 알 지 못합니다. 그저 EOLString 이 그 작업을 **책임진다는 사실**만을 알고 있습니다.
* EOLString 에 대한 결합은 계약(Contract)을 통해 추가된 것이며 언제라도 분리가 가능하기 때문에 유지보수성을 저하시지키 않습니다.

* 퍼블릭 상수마다 계약의 의미를 캡슐화하는 새로운 클래스를 만들어야 할까요? -> 맞습니다.
* 수백 개의 단산한 상수 문자열 리터럴 대신 **수백 개의 마이크로 클래스**를 만들어야 할까요? -> 맞습니다.
* 중복 코드를 가진 마이크로 클래스들에 의해 코드가 더 장황해지고 오염되지는 않을까요? -> 아닙니다.
    * 클래스 사이에 중복 코드가 없다면, 클래스가 작아질수록 코드는 더 깔끔해집니다.
* 논리적이지 않을 수 있지만, **애플리케이션의 클래스의 수가 많을수록** 설계가 더 좋아지고 유지보수하기도 쉬워집니다.
* java.io.File 이 여러 코드에서 사용될 경우, TextFile, JPGFile, TempFIle 을 사용한다면 이해하기가 더 수월해집니다.

```java
String body=new HttpRequest().method(HttpMethods.POST).fetch();
```

* 이 코드는 OOP의 정신에 어긋납니다.
* 리터럴을 사용하는 대신 HTTP 메서드를 표현하는 **단순한 클래스를 많이** 만드는 편이 더 좋습니다.

```java
String body = new PostRequest(new HttpRequest()).fetch();
```

* java 의 열거형(enum) 에 대해서도 동일한 규칙이 적용됩니다.
* 열거형과 퍼블릭 상수 사이에는 아무 차이가 없기 때문에 **열거형 역시 사용해서는 안됩니다**.
