ECMAScript 명세서의 알고리즘내 데이터의 구성(aggregation)을 설명하는 데 사용하는 데이터 타입이다.(실제 JS의 데이터 타입은 아니고 명세서에서 설명을 위해서만 사용되는 타입)
Record 타입의 값은  하나 이상의 필드로 구성되며 각 필드는 **ECMAScript language value**(숫자, 문자열, 객체 등의 JS값으로 생각하면 편함) 혹은 **specification value**(undefined, null, NaN 등 특정 용도나 동작을 가진 값)을 값으로 가진다.
자바스크립트의 객체 리터럴과 비슷하다고 생각하면 된다.

#### 표기법
`{ [[Field1]]: 42, [[Field2]]: false, [[Field3]]: empty }`
객체 리터럴과 유사한 방식으로 중괄호로 Record를 표현하고 filed의 이름은 2개의 대괄호 속에 기입한다.
명세서 내에서 record 내의 filed에 접근할 때는 `R.[[Field1]]`과 같이 dot notation을 통해서 표기한다.
`PropertyDescriptor { [[Value]]: 42, [[Writable]]: false, [[Configurable]]: true }
자주 사용되어 특별한 이름이 있는 레코드의 경우 위와 같이 중괄호 앞에 그 레코드의 이름을 표기하여 특정 레코드의 구조를 설명할 수 있다.

`