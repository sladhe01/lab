큐는 먼저 집어넣은 데이터가 먼저 나오는 성질(FIFO, First In First Out)을 가진 자료 구조이다. 삽입 및 삭제에 O(1), 탐색에 O(n)이 걸린다.
CPU 작업을 기다리는 프로세스, 스레드 행렬 또는 네트워크 접속을 기다리는 행렬, 너비 우선 탐색, [[Cache|캐시]] 등에 사용된다.

```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
	queue <int> q; // --- (1)
	q.push(1); // --- (2)
	cout << q.front() << "\n"; // --- (3)
	q.pop(); // --- (4)
	cout << q.size() << "\n"; // --- (5)
	return 0;
}
/*
1
0
*/
```

>(1) - int 타입의 요소를 가지는 큐 선언
>(2) - 큐에 1 삽입
>(3) - 큐의 가장 앞에 있는 요소 출력
>(4) - 큐의 맨 앞에 있는 요소 제거
>(5) - 큐의 크기 출력