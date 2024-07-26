
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