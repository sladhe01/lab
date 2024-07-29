### 프로젝트 작성 디렉토리
go 1.13이전 버전은 프로젝트 파일을 GOPATH에 작성했어야 하지만 이후 버전부터는 Go Modules가 기본적으로 활성화 되어 있기 때문에 프로젝트 파일의 루트 디렉토리에 존재하는 go.mod를 통해 다른 디렉토리에서도 작성가능하다.
```
$go mod init <module-name>
```


### 다른 외부 패키지를 이용할 때
```
$go get <패키지 경로>
```
위 명령어를 통해서 1. 해당 패키지를 주어진 경로에서 다운받고 2. go.mod에 의존성 정보를 추가하고 3. go.sum 파일에 패키지의 해시값과 버전을 기록한다.

func main()이 무조건 존재해야 한다. (go 프로그램의 시작점이 되는 부분)
자동적으로 컴파일러는 main package와 그 안에 있는 main function을 먼저 찾고 실행시킨다.
main package는 오로지 컴파일을 위해 필요한 것이기 때문에 공유용 라이브러리를 위한 것이라면 굳이 필요하지 않음


### import 방법
```go
import "package-name"
```


### 함수 export
node.js에서는 함수를 export하려면 export 한다고 명시해줘야 했지만 go는 함수의 첫 글자를 대문자로 만들면 이게 export하겠다는 뜻(소문자로 만들면 private function)


### 상수, 변수 선언
```go
const name string = "go"
var country string = "Korea"
country := "Korea" //위와 같은 코든데 줄여서 쓰면 알아서 타입 추론해줌
//단, func 안에서만, 변수만 줄여쓸 수 있음
name, country := "go", "Korea"// 이렇게 복수로 사용도 가능
```

### 함수의 인자와 반환값의 타입을 알려줘야 한다.
```go
func multiply (a int, b int) int {
	return a*b
}

func multiply (a,b int) int {
	retunr a*b
} //복수의 인자의 타입이 같으면 맨 뒤에만 적어줘도 된다
```


### go의 특이한 점은 반환값이 여러 타입일 수 있다.
```go
func lenAndUpper(name string) (int, string) {
	return len(name), strings.ToUpper(name)

}//소괄호 속에 두가지 반환 타입을 입력하면 된다.

func main() {
	totalLength, upperName := lenAndUpper("jeong")
	fmt.Println(totalLength, upperName)// 5, JEONG
	totalLength2, _ := lenAndUpper("jeong")//두가지 반환값중 언더스코어로 처리된 후자는 반환하지 않는다.
	fmt.Println(totalLength2)// 5
}
```


### 갯수가 정해지지 않은 다수의 arguments를 받는 함수를 만들고자 하면 ...를 이용한다.
```go
func repaetMe(words ...stirng) {
	fmt.Println(words)
}

func main() {
	repeatMe("jeong", "kim", "lee", "park")
}//[jeong, kim, lee, park] 배열의 형태로 출력됨
```


### naked return
함수 초반에 반환할 대상과 타입을 명시해주고 함수 내부에서 반환값을 명시하지 않고 return만 함
```go
func lenAndUpper(name string) (length int, upperCase string) {
	length = len(name)//위에서 length라는 변수는 이미 초기화 되었고 여기선 값을 변경해주는 것
	upperCase = strings.ToUppper(name)
	return //위에서 반환값을 명시해줬기 때문에 여기서 return만 해줘도 정해진 반홥값들이 반환됨
}
```


### defer
힘수가 끝난 후에 다른 행위를 할 수 있도록 해줌
```go
func lenAndUpper(name string) (length int, upperCase string) {
	defer fmt.Println("I'm done")//return 후 출력됨
	length = len(name)
	upperCase = strings.ToUpper(name)
	return
}

func main() {
	totalLength, upperName := lenAndUpper("jeong")//return 하고 I'm done 출력
	fmt.Println(totalLength, upperName)//5 JEONG 출력
}//I'm done
//5 JEONG
```

### loop
오직 for만 가능 forEach, map, for in, for of 등 없음
for 다음에 인덱스, 요소 순으로 써주고 필요할 경우 인덱스만 쓰거나 \_ 로 요소만 쓸 수있다.
for i, element := rangne elements{}를 통해 인덱스 처음부터 끝까지 순회할 수도 있고
for i := range elements{}
for \_, element := range elements{}
for i := 조건문을 통해서 조건에 부합한 index만 순회할 수도 있다.(조건문은 인덱스에만 붙일 수 있음)
```go
func superAdd(elements ...int) int{
	total := 0
	for index, element := range elements {
		fmt.Println(index, element)
		total += element
	}//ranage 이용시 인덱스 처음부터 끝까지 순회
	return total
}
```

```go
func superAdd(elements ...int) int{
	total := 0
	for i := 0; i<len(elements); i++ {
		fmt.Println(elements[i])
		total += elements[i]
	}
	return total
}
```

### 조건문 
