# CEP 8 - 절대 NULL을 반환하지 마세요

### Header

* Reference
    * 엘레강트 오브젝트 4.1, 154p
* Status: Draft

### 규약

* 대상을 찾지 못했다면 NULL을 반환하는 대신 1)예외를 던지거나, 2)컬렉션을 반환하거나, 3)널 객체를 반환한다.

### 논의

* Optional 을 사용할 지 여부

### 정리

```
public String title(){
  if(/* title 이 없다면 */){
    return null;
  }
  
  return "Elegant Objects";
}
```

* 이 설계는 객체를 장애를 가진 존재로 취급해서는 안된다는 조언에 정확하게 반대됩니다.
    * [CEP 7](cep-7.dont_allow_null_argument.md) 참조
* title()이 반환하는 객체는 **신뢰할 수 없습니다**.
* 이 객체는 장애를 안고 있습니다. 호출할 때 마다 NullPointerException 예외가 던져질지 모른다는 사실에 불안할 수 밖에 없습니다.
* 문제는 예외가 아닙니다. 예외는 단지 기술적인 불편함일 뿐이고, 더 큰 문제는 객체에 대한 **신뢰가 무너졌다는 사실**입니다.
* 반환된 값이 객체인지부터 확인해야 하기 때문에, 객체에게 작업을 요청한 후 안심하고 결과에 의지할 수 없습니다.

* 객체라는 사상에는 우리가 **신뢰하는 엔티티라는 개념**이 담겨져 있습니다.
  * 객체는 단순한 데이터 조각이 아니고 표식(token)도 아니고 데이터를 담는 봉투도 아닙니다.
  * 우리는 객체를 신뢰하기 때문에 객체와 동일한 의미를 가지는 변수 역시 신뢰합니다.
  * 신뢰란 객체가 **자신의 행동을 전적으로 책임지고 우리가 간섭하지 않는다**는 의미가 담겨 있습니다.
* 반환값을 검사하는 방식은, 애플리케이션에 대한 신뢰가 부족하다는 명백한 신호입니다.
  * 코드를 읽을 수록 NULL을 반환하는 메서드 호출이 어떤 것인지를 이해하는데 더 많은 시간이 필요하기 떄문에 
  * 결과적으로 유지보수성의 심각한 손실로 이어집니다.
  * 동료가 속임수를 써서 NULL을 반환한다면, 작업 속도는 심각하게 느려집니다.
  * 저는 소프트웨어 안에서, 그리고 제 팀에 대해 안전하다고 느끼지 못할 것입니다.

* 4.1.2 NULL 의 대안
* 두 번째 방법은 NULL반환, 예외 발생 대신 빈 컬렉션을 반환하는 것입니다.
  * 메서드의 이름을 user() 에서 users()로 변경했다는 사실에도 주목하기 바랍니다.
  * java.util.Optional 을 사용하는 방법도 있습니다.
    * Optional 은 컬렉션과 동일하지만, 오직 하나의 요소만 포함할 수 있습니다.
    * 메서드의 이름은 user()지만, 실제로 반환하는 객체는 사용자가 아니라 사용자를 포함하는 일종의 봉투입니다.
    * NULL 참조와 매우 비슷합니다. 사용하지 않기를 바랍니다.
* 세 번째 방법은 널 객체 디자인 패턴입니다. (SqlUser, NullUser)
* NULL은 java를 포함한 모든 언어에서 유해한 키워드입니다.
* NULL 대신 예외를 던지거나, 컬렉션을 반환하거나, 널 객체를 반환하세요
