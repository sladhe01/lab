Tree는 그래프 중 하나로 정점과 간선으로 이루어져 있고, 트리 구조로 배열된 계층적 데이터 집합이다.
루트 노드, 내부 노드, 리프 노드 등으로 구성된다.
그리고 트리로 이루어진 집합을 숲이라고 부른다.

<img alt="pasted image 0.png" src="https://github.com/sladhe01/lab/blob/main/cs/Data%20Structure/Non-Linear%20Data%20Structure/pasted%20image%200.png?raw=true" data-hpc="true" class="Box-sc-g0xbh4-0 kzRgrI">
## 트리의 특징
1.  부모, 자식 계층 구조를 가진다.
2.  간선 수는 노드 수 - 1 이다. (V - 1 = E)
3.  임의의 두 노드 사이의 경로는 항상 존재하고 단 하나뿐이다.

## 트리의 구성
### 루트 노드(Root Node)
최상위에 위치하는 부모가 존재하지 않는 노드를 말한다.

### 내부 노드(internal node, inner node)
자식 노드를 하나 이상 가진 노드를 말한다.

### 리프 노드(leaf node)
자식 노드가 없는 노드를 말한다.

#### 트리의 높이와 레벨
- 깊이 - 루트 노드부터 특정 노드까지 최단 거리로 갔을 때의 거리를 말한다.
- 높이 - 루트 노드부터 리프 토드까지 가장 긴 거리를 말한다. 위의 예시에서는 3이다.
- 레벨 - 보통 깊이와 비슷한 의미를 가지지만 기준이 되는 루트 노드의 레벨을 어떻게 정하는 지에 따라서 달라진다. 예를 들어 루트 노드의 레벨이 0이라면 4번 노드는 레벨2이고, 루트 노드의 레벨이 1이라면 4번 노드의 레벨은 3이다.
- 서브 트리 - 트리 내부의 하위 집합을 말한다.

## 트리의 유형

### 이진 트리(binary tree)

<img alt="64eb4e97c2b21c77ccb25d05_Screenshot 2023-08-27 at 9.24.32 AM.png" src="https://github.com/sladhe01/lab/blob/main/cs/Data%20Structure/Non-Linear%20Data%20Structure/64eb4e97c2b21c77ccb25d05_Screenshot%202023-08-27%20at%209.24.32%20AM.png?raw=true" data-hpc="true" class="Box-sc-g0xbh4-0 kzRgrI">
자식 노드의 개수가 2개 이하인 트리를 의미한다. 다음과 같은 유형이 존재한다.

- 정이진 트리(full binary tree) - 자식 노드가 0 또는 2개인 이진트리를 말한다.
- 완전 이진 트리(complete binary tree) - 왼쪽부터 채워져 있는 이진 트리를 말한다. 마지막 레벨을 제외하고 모든 레벨이 완전히 채워져 있고 마지막 레벨의 경우 왼쪽부터 채워져 있다.(포화 이진 트리도 완전 이진트리의 일종이다.)
- 변질 이진 트리(degenerate binary tree) - 자식 노드가 하나밖에 없는 이진 트리를 말한다.
- 포화 이진 트리(perfect binary tree) - 모든 노드가 꽉 차있는 이진 트리를 의미한다.
- 균형 이진 트리(balanced binary tree) - 왼쪽과 오른쪽 노드의 높이 차가 1 이하인 이진 트리를 말한다. map, set을 구성하는 레드 블랙 트리는 균형 이진 트리의 일종이다.

### 이진 탐색 트리(binary search tree, BST)

<img alt="images.png" src="https://github.com/sladhe01/lab/blob/main/cs/Data%20Structure/Non-Linear%20Data%20Structure/images.png?raw=true" data-hpc="true" class="Box-sc-g0xbh4-0 kzRgrI">

노드의 오른쪽 하위 트리에는 노드보다 큰 값이 있는 노드만 포함되고, 노드의 왼쪽 하위 트리에는 노드 보다 작은 값이 있는 노드만 포함되는 트리를 말한다. 이런 구조를 가지면 검색을 하기에 용이하다. 탐색시 평균 시간복잡도는 O(logn)이고 최악의 경우 O(n)의 시간복잡도를 가진다. 이 때는 선형적인 트리구조를 이룬다.

### AVL 트리(Adelson-Velsky and Landis tree)

이진 탐색트리가 선형적인 트리가 되는 것을 방지하고 스스로 균형을 잡는 이진 탐색 트리를 말한다. 
두 자식 서브트리의 높이는 항상 최대 1만큼만 차이난다.
트리의 높이가 n일 때, 탐색, 삽입, 삭제 모두 시간 복잡도가 O(logn)이며 삽입, 삭제를 할 때마다 균형을 맞추기 위해 트리 일부를 왼쪽 또는 오른쪽 방향으로 회전시킨다.


### 레드 블랙 트리

균형 이진 탐색 트리의 일종으로 탐색, 삽입, 삭제 모두 시간 복잡도가 O(logn)이다. 각 노드는 빨간색 혹은 검은색의 색상을 나타내는 추가 비트를 저장하며 삽입 또는 삭제 중에 트리가 균형을 유지하도록 하는 데 사용된다.