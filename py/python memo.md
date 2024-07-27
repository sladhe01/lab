
### 함수

함수 선언시 ***def*** 키워드와 ***:*** 사용
중괄호 대신 공백사용
기본값-> `parameter = default_value`
```python
def hello(name="doe"):
	print("hello", name)
```

함수 호출시 argument가 정해진 순서대로 넣어서 호출도 가능하고 'keyword argument'를 이용해서 호출도 가능
```python
def greet(first_name, last_name, age): 
	print(f"Hello, {first_name} {last_name}. You are {age} years old.")

greet(age=32, lastname='Jeong', first_name='DongMin')
```

### 문자열 리터럴

***f*** 키워드(format이라는 의미)와 중괄호 사용
```python
my_name = "jeong"
f"hello my name is {my_name}"
```

### 조건문

js의 ***else if*** 대신 ***elif*** 라는 키워드 사용
js에서 부정을 나타낼 때 !를 사용했지만 파이썬은 not 키워드를 사용
```python
if(a<0):
	return f"{a} is less than 0"
elif(a==0):
	return f("{a} is equal to 0")
else:
	return f("{a} is greater tahn 0")
```

### and, or

and, or을 나타내는 논리연산자를 ***&&*** 또는 ***||*** 로 표현하지 않고 문자 그대로 ***and*** 또는 ***or*** 사용

### module import

예를 들어 Python Standard Lbrarry의 radonm 모듈의 randint 함수를 import 해보자
```python
from random import randint

radom_num = random.randint(1,10)
```

### 주석

\#을 앞에 써서 주석을 사용한다.
여러줄의 주석을 사용할 떄는 시작과 끝에 """을 붙인다.

### List

js의 array와 유사한 개념 마찬가지로 \[\]로 element를 기입하고 인덱스를 통해 접근 가능
js와 다르게 음수 인덱스를 통해 뒤에서부터 차례대로 접근 가능

### Tuple

( )안에 element를 집어 넣고 리스트와 비슷한 자료구조지만 수정이 불가능하다. 마찬가지로 인덱스를 통해 접근

### Dictionary

js의 객체와 비슷한 개념 마찬가지로 { }로 속성이 아닌 key, value pair라고 부르는 쌍을 담고 있으나 key는 stirng 형태를 쓸 때 ''를 붙여줘야하고 이외 불변자료형도 사용가능하다. 접근 방식은 dicts.get('key')와 같은 메서드를 사용하거나 dicts\['key']와 같이 접근할 수 있다. 추가 혹은 수정은 대입문을 통해 가능하다.
```python
jeong = {
  'age' : 32,
  'male': True,
  'job' : 'developer'
}

print(jeong) #{'age' : 32, 'male': True, 'job' : 'developer'}
print(jeong.get('age')) #32
print(jeong['age']) # 32

jeong['score'] = 100
print(jeong) #{'age' : 32, 'male': True, 'job' : 'developer', 'score': 100}

```

### Class

js의 constrcutor와 같은 역할을 하는 \_\_init__ 메서드가 있다.
method의 첫번째 argument는 항상 클래스 그 자체(주로 self로 많이 사용)가 되어야 한다. (메서드는 항상 클래스 자기 자신에 대한 참조를 받고 있다.)
```python
class Dog:
	def __init__(self, name, age, breed):
		Print(self) 
		self.name = name
		self.age = age
		self.breed = breed

foo = Person('milk', 5, 'samoyed') 
#<__main__.Person object at 0x792c2bc659f0>
 
print(foo)
#<__main__.Person object at 0x792c2bc659f0>
```

상속시 클래스명 옆에 소괄호를 쓰고 그 안에 상속 받을 클래스를 써주면 된다.
super( )를 통해서 부모 클래스를 참조한다.
```python
class Puppy(Dog):
	def __init__(self, name, breed):
		super().__init__(name, breed, 0.1)
	def woof_woof(self):
		print('Woof Woof!')
```