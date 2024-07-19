스택은 가장 마지막으로 들어간 데이터가 가장 먼저 나오는 성질LILO, Last In Last Out)을 가진 자료 구조다. 주로 재귀적인 함수, 알고리즘에 사용되고 웹 브라우저의 방문 기록 등에 쓰인다. 삽입 및 삭제에 O(1), 탐색에 O(n)이 걸린다.

```cpp
#include <bits/stdc++.h>
using namespace std;
stack<int> stk; // --- (1)
int main() {
	ios_base::sync_with_stdio(false); // --- (2)
	cin.tie(NULL); // --- (3)
	for (int i=0; i< 10; i++) stk.push(i); // --- (4)
	while (stk.size()) }{
		cout << stk.top() << " "; // --- (5)
		stk.pop(); // --- (6)
	}
}
/*
9
8
7
6
5
4
3
2
1
0
*/
```

>(1) - int타입의 요소를 갖는 stk라는 이름의 스택을 선언
>(2) - C(stdio)와 C++(ios_base)의 입출력 [[Stream]]이 동기화 되지 않도록 설정한다.
>****각 스트림이 독립적인 [[Buffer]]를 가지도록하여 성능의 향상을 가져온다***
>(3) - 입력 버퍼와 출력버퍼의 연결을 끊는다.
>****기본적으로 cin과 cout은 stream buffer를 공유하고 있다. 입력 또는 출력 요청을 받으면 실행 전 각각의 stream 버퍼를 확인하고 비우는 과정을 거치는데 이 코드를 통해서 입력버퍼와 출력버퍼의 연결을 끊어주면 위의 과정들을 거치지 않기 때문에 성능 향상을 기대할 수 있다.***
>(4) - 0부터 9까지 스택에 추가
>(5) - 스택의 가장 위에 있는 요소 출력
>(6) - 스택의 가장 위에 있는 요소 제거