complexity(복잡도)는 알고리즘의 효율성을 나타내는 척도로 time complexity(시간 복잡도)와 space complexity(공간 복잡도)로 나뉜다.

다른 설명을 하기 전 먼저 C++에 대해 조금 알아보자.
아래는 입력받은 문자열을 출력하는 코드다.
```cpp
#include <bits/stdc++.h> // --- (1)
using namespace std; // --- (2)
string a; // --- (3)
int main()
{
	cin >> a; // --- (4)
	cout << a << "\n"; // --- (5)
	return 0; // ---(6)
}
```

C++은 main 함수를 중심으로 돌아가기 때문에 main 함수를 무조건 하나 만들어야 한다.
이후 컴파일이 시작되면 전역 변수 초기화. 라이브러리 import 등의 작업이 일어나고, mian 함수에 얽혀 있는 함수들이 작동한다. 그러고 나서 main 함수가 0을 return하면서 전체적인 프로세스가 종료된다. 코드의 각 부분에 대해서 알아보자.

	1. 헤더파일이다. STL 라이브러리를 import하고 bits/stdc++.h는 모든 표준 라이브러리가 포함된 헤더이다.
	2. std라는 이름의 네임스페이스를 사용한다는 뜻이다. cin 이나 cout 등을 사용할 때 원래는 std::cin 처럼 네임스페이스를 달아서 호출해야 하는데, 이를 기본으로 설정한다는 뜻이다. 
	   ****네임스페이스는 같은 클래스 이름 구별, 모듈화에 쓰이는 이름을 말한다.***
	3. 문자열 선언
	   <타입> <변수명> 순서로 선언한다.
	   예를 들어 string a = "Jeong" 이라고 할 때 a를 lvalue "Jeong"을 rvalue라고 하고 lvalue는 재사용이 가능하고 rvalue는 한번 쓰고 다시 사용되지 않는 변수를 말한다.
	4. 입력
	   대표적으로 cin, scanf가 있다. '>>'는 입력 연산자다.
	5. 출력
	   대표적으로 cout, printf가 있다. '<<'는 출력 연산자다.
	6. return 0
	   프로세스가 정상적으로 마무리 됨을 의미
### [[Time Complexity]]

### [[Space Complexity]]