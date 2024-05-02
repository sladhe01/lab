complexity(복잡도)는 알고리즘의 효율성을 나타내는 척도로 time complexity(시간 복잡도)와 space complexity(공간 복잡도)로 나뉜다.

다른 설명을 하기 전 먼저 C++에 대해 조금 알아보자.
아래는 입력받은 문자열을 출력하는 코드다.
```C++
#include <bits/stdc++.h> // --- (1)
using namespace std; // --- (2)
string a; // --- (3)
int main()
{
	cin >> a; // --- (4)
	cout << a << "\n"; // --- (5)
	return a; // ---(6)
}
```
C++은 main 함수를 중심으로 돌아가기 때문에 main 함수를 무조건 하나 만들어야 합니다.
### Time Complexity
시간 복잡도를 알아보기 전에 