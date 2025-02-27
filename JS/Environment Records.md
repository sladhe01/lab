Environment Record는 ECMAScript 코드의 [[Lexical|lexical]] netsing 구조에 기반한(코드의 실행 시점이 아닌 코드의 작성 위치에 기반한다는 뜻) 변수, 함수 등의 식별자와 값의 연결을 설명하는 데 이용되는 명세용 타입이다.
코드가 평가될 때마다 새로운 Environment Record가 생성되어 해당 코데에서 생성된 식별자 바인딩(binding)을 기록한다.

모든 Environment Record는 \[\[OuterEnv]]필드를 가지고 있고 이 필드는 **null** 또는 **내부 Environment Record를 감싸고 있는 외부 Environment Record에 대한 참조(reference)**를 값으로 가진다.