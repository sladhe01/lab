go 1.13이전 버전은 프로젝트 파일을 GOPATH에 작성했어야 하지만 이후 버전부터는 Go Modules가 기본적으로 활성화 되어 있기 때문에 프로젝트 파일의 루트 디렉토리에 존재하는 go.mod를 통해 다른 디렉토리에서도 작성가능하다.
```
$go mod init <module-name>
```


다른 외부 패키지를 이용할 때
```
$go get <패키지 경로>
```
위 명령어를 통해서 1. 해당 패키지를 주어진 경로에서 다운받고 2. go.mod에 의존성 정보를 추가하고 3. go.sum 파일에 패키지의 해시값과 버전을 기록한다.

func main()이 무조건 존재해야 한다. (go 프로그램의 시작점이 되는 부분)
자동적으로 컴파일러는 main package와 그 안에 있는 main function을 먼저 찾고 실행시킨다.
main package는 오로지 컴파일을 위해 필요한 것이기 때문에 공유용 라이브러리를 위한 것이라면 굳이 필요하지 않음


import 방법
```go
import "package-name"
```


node.js에서는 함수를 export하려면 export 한다고 명시해줘야 했지만 go는 함수의 첫 글자를 대문자로 만들면 이게 export하겠다는 뜻(소문자로 만들면 private function)