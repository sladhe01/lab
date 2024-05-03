Linked List(연결 리스트)는 데이터를 감싼 노드를 포인터로 연결하여 공간적인 효율성을 극대화 시킨 자료 구조이다. 삽입시 O(1), 탐색시 O(n)이 걸린다.

\**node - 프로그래밍에서  노드는 데이터 구조에서 하나의 요소를 나타낸다. 각 노드는 데이터를 저장하는 공간과 해당 데이터와 관련된 다른 노드를 가리키는 포인터를 포함한다. 맨 앞의 노드를 head라고 부른다.*
</br>

![[Pasted image 20240503163112.png]]

- Singly Linked List(싱글 연결 리스트) - next 포인터만 가진다.
- Doubly Linked List(이중 연결 리스트) - prev포인터와 next 포인터 두 가지를 가진다.
- Circular Linked List(원형 연결 리스트) - 싱글 연결 리스트와 같지만 마지막 next포인터가 head를 가리킨다.
- Circular Doubly Linked List(원형 이중 연결 리스트) - 위 그림에는 없지만 이중 연결리스트와 같지만 마지막 next 포인터가 head를 가리킨다.

C++에는 앞에서 부터 요소를 넣는 `push_front()`함수와 뒤에서부터 요소를 넣는 `push_back()`함수와 중간에 요소를 넣는 `insert()`등의 함수가 있다.
</br>

```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
	list<int> a; // 안에 정수를 담는 a라는 이름의 리스트 선언
	for (int i = 0; i < 10; i++)a.push_back(i); // 0~9까지 리스트 끝부터 추가
	for (int i = 0; i < 10; i++)a.push_front(i); // 0~9까지 리스트 앞부터 추가
	auto it = a.begin(); it++; // a 리스트의 시작점을 가리키는 반복자 it 선언
	a.insert(it, 1000);
	for (auto it : a) cout << it << " ";
	cout << '\n';
	a.pop_front();
	a.pop_back();
	for (auto it : a) cout << it << " ";
	cout << `\n`;
	return 0;
}
/*
9 1000 8 7 6 5 4 3 2 1 0 0 1 2 3 4 5 6 7 8 9
1000 8 7 6 5 4 3 2 1 0 0 1 2 3 4 5 6 7 8
*/
```