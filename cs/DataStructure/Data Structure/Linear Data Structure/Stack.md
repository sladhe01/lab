스택은 가장 마지막으로 들어간 데이터가 가장 먼저 나오는 성질LILO, Last In Last Out)을 가진 자료 구조다. 주로 재귀적인 함수, 알고리즘에 사용되고 웹 브라우저의 방문 기록 등에 쓰인다. 삽입 및 삭제에 O(1), 탐색에 O(n)이 걸린다.

```cpp
#include <bits/stdc++.h>
using namespace std;
stack<int> stk; // --- (1)
int main() {
	ios_base::sync_with_stdio(false); // --- (2)
	cin.tie(NULL); // --- (3)
	for (int i=0; i< 10; i++) stk.push(i);
	while (stk.size()) }{
		cout << stk.top() << " ";
		stk.pop(); 
	}
}
/*
9 8 7 6 5 4 3 2 1 0
*/
```