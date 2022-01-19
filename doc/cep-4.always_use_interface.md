# CEP 4 - 항상 인터페이스를 사용하세요

### Header

* Reference
    * 엘레강트 오브젝트 2.3, 44p
* Status: Draft

### 규약

* 클래스의 모든 퍼블릭 메서드는 인터페이스를 구현해야 한다.
    * 즉 인터페이스를 구현하지 않는 메서드는 퍼블릭이어서는 안된다.

### 정리

* 객체들은 서로를 필요하기 때문에 **결합된다(coupled)**
* 어플리케이션이 성장하면서 객체들의 수가 수십개를 넘어가면 객체 사이의 **강한 결합도**(tight coupling)가 심각한 문제로 대두된다
* **유지보수가 가능하기 위해서는** 최선을 다해서 **객체를 분리해야(decouple)** 합니다.
* 이를 가능하게 하는 도구가 바로 인터페이스 입니다.

```java
interface Cash {
  Cash multiply(float factor);
}
```

* 인터페이스는, 다른 객체와 의사소통하기 위해 따라야 하는 **계약(contract)** 입니다.
* 다음과 같은 규칙을 제안하겠습니다. 클래스의 **모든 퍼블릭 메서드가 인터페이스를 구현**하도록 만들어야 합니다.
* 동일한 인터페이스를 구현하는 여러 클래스들이 존재합니다.
* 그리고 각각의 경쟁자는 다른 경쟁자를 쉽게 대체할 수 있어야 합니다. 이것이 느슨한 결합도(loose coupling)의 의미입니다.