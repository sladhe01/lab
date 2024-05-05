벡터는 C++에 존재하는 동적으로 요소를 할당할 수 있는 동적 배열이다. 컴파일 시점에 개수를 모른다면 벡터를 사용해야 한다. 배열과 동일하게 요소의 중복을 허용하고 순서가 있으며 랜덤 접근이 가능하다. 맨 뒤의 요소를 삭제, 삽입하는 데 O(1)의 시간이 걸리고 맨 뒤가 아닌 요소를 삭제, 삽입하는 데 O(n)의 시간이 걸린다.
****참고로 뒤에서부터 삽입하는 push_back()의 경우 벡터의 크기가 증가하는 시간 복잡도를 합산 하게되면 O(1)보다는 시간이 더 걸리지만 상수 시간이 걸리기 때문에 상수 시간 복잡도 O(1)으로 표시한다.***

```cpp
#include <bits/stdc++.h>
using namespace std;
vector<int> v; // --- (1)
int main() {
	for (int i=1; i <= 10; i++) v.push_back(1); // --- (2)
	for (int a:v) cout << a << " "; // --- (3)
	cout << "\n";
	v.pop_back(); // --- (4)
	
	for (int a:v) cout << a << " ";
	cout << "\n";
	
	v.erase(v.begin(), v.begin() + 1); // --- (5)
	
	for (int a:v) cout << a << " ";
	cout << "\n";
	
	auto a = find(v.begin(), v.end(), 100); // --- (6)
	if (a === v.end()) cout << "not found" << "\n";
	
	fill(v.begin(), v.end(), 10); // --- (7)
	for (int a:v) cout << a << " ";
	cout << "\n";
	v.clear(); // --- (8)
	for (int a:v) cout << a << " ";
	cout << "\n";
	
	return 0;
}

```
>(1) - int 유형의 요소를 가지는 v라는 변수명의 벡터 선언
>(2) - 뒤에서 부터 1부터 10까지 요소 삽입 // 1 2 3 4 5 6 7 8 9 10
>(3) - `for (int i=0; i<v.size(); i++) cout << v[i] << '\n;`과 같은 뜻으로 JavaScript의 for of 와 비슷한 표현으로 `for (유형 변수:container)`와 같은 방식으로 쓴다.
>(4) - 뒤에서 첫번째 요소를 제거한다 // 1 2 3 4 5 6 7 8 9
>(5) - v.begin( ) 즉 첫번째 요소부터 v.begin( )+1 번째 요소 전까지 제거 // 2 3 4 5 6 7 8 9
>(6) - find( ) 함수는 주어진 컨테이너의 첫번째 인자 여기서는 v.begin( )