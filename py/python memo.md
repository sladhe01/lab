
#### 함수 선언

***def*** 키워드와 ***:*** 사용
중괄호 대신 공백사용
기본값-> `parameter = default_value`
```Py
def hello(name="doe"):
	print("hello", name)
```

#### 문자열 리터럴

***f*** 키워드(format이라는 의미)와 중괄호 사용
```py
my_name = "jeong"
f"hello my name is {my_name}"
```

#### 조건문

js의 ***else if*** 대신 ***elif*** 라는 키워드 사용
```py
if(a<0):
	return f"{a} is less than 0"
elif(a==0):
	return f("{a} is equal to 0")
else:
	return f("{a} is greater tahn 0")
```

#### and, or

and, or을 나타내는 논리연산자를 ***&&*** 또는 ***||*** 로 표현하지 않고 문자 그대로 ***and*** 또는 ***or*** 사용

#### module import

예를 들어 Python Standard Lbrarry의 radonm 모듈의 randint 함수를 import 해보자
```py
from random import randint

radom_num = random.randint(1,10)
```

#### 주석

\#을 앞에 써서 주석을 사용한다.
여러줄의 주석을 사용할 떄는 시작과 끝에 """을 붙인다.

#### Lists

js의 array와 유사한 개념 마찬가지로 \[\]로 element를 기입하고 인덱스를 통해 접근 가능