Environment Record는 ECMAScript 코드의 [[Lexical|lexical]] netsing 구조에 기반한(코드의 실행 시점이 아닌 코드의 작성 위치에 기반하여 중첩되는 구조를 뜻함) 변수, 함수 등의 식별자와 값의 연결을 설명하는 데 이용되는 명세용 타입이다.
코드가 평가될 때마다 새로운 Environment Record가 생성되어 해당 코드에서 생성된 식별자 바인딩(binding)을 기록한다.

모든 Environment Record는 \[\[OuterEnv]]필드를 가지고 있고 이 필드는 **null** 또는 **내부Environment Record를 감싸고 있는 외부 Environment Record에 대한 참조(reference)** 를 값으로 가진다. 예를 들어 함수A 안에 함수B가 있다면 함수B의 Environment Record는 함수A의 Environment Record를 참조한다. 이런 식으로 참조를 통해서 Environment Record의 중첩(nesting)이 모델링 되었다.

\*Environment Record는 ECMAScript 명세서 정의된 **기능적 메커니즘**일 뿐이고 특정 엔진에서 **구체적인 구현체**를 필요로 하지는 않습니다. 즉, ECMAScript 프로그램에서 **직접적으로 Environment Record에 접근하거나 이를 조작하는 것은 불가능**하다.

#### 하위 클래스
Environment Record는 추상적인 클래스로 역할에 따라 아래와 같은 3가지 종류의 하위클래스로 분류할 수 있다.(아래 3가지의 하위클래스는 Envrionmnet Record를 이루는 구성요소가 아니라 하나의 종류들이다.)
- Declarative Environment Record : 함수선언, 변수선언(let, const 등) Catch 절 등과 관련된 식별자 바인딩(binding, 식별자와 값을 연결하는 것)을 정의하는 데 사용된다. 객체의 프로퍼티로 저장되지 않고 내부적으로 관리된다.(Object Environment Record처럼 어떤 객체의 프로퍼티로 저장되는 게 아니라 변수 자체가 내부 데이터 구조에 저장된다는 뜻이다.)
    - Function Environment Record : 함수 호출과 관련되어 함수가 호출될 때 생성된다. 함수 내부의 최상위 선언(top-level declaration)의 바인딩(binding)을 포함하고 있다. 새로운 this 바인딩을 생성할 수 있고 super 메서드 호출을 지원하기 위해 필요한 정보를 가지고 있다.
      \*top-level declaration이 아닌 경우로 함수 속의 if 블럭 속에 선언된 변수 등을 생각해 보면 된다.
    - Module Environment Record : 모듈의 최상위 선언을 위한 위한 바인딩을 포함하고 모듈에서 명시적으로  import한 것의 바인딩도 포함한다. 이 Record의 \[\[OuterEnv]]는 Global Environment Record다.
- Object Environment Record : 객체(Object)와 관련된 바인딩을 저장하는 환경레코드다. with 문에서는 with의 타겟이 되는 객체의 프로퍼티가 Object Envrionment Record에 저장된다. 글로벌 객체도 객체이기 때문에 글로벌 객체와 관련된 바인딩도 저장된다.
- Global Environment Record : 이 레코드는 전역 실행 컨텍스트에 사용되는 레코드로 스크립트의 전역 선언을 다루며 \[\[OuterEnv]]값이 null이다. 글로벌 객체(브라우저면 window node.js면 global)와 같이 미리 정의된 식별자(미리 정의된 전역객체)바인딩을 가질 수 있다. 코드가 실행될 때 전역객체에 다른 프로퍼티가 추가 될 수 도 있다.(예를 들어 글로벌 스코프에서 선언된 변수가 글로벌 객체에 추가 되는 경우)
