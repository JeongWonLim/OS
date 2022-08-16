# Two pointer
## 방식
1. 한 포인터는 앞에서 나머지 한 포인터는 맨 뒤에서 시작하는 방식. 특정 중간 지점에서 만나는 조건이 종료조건이 된다.
2. 같은 지점에서 시작하는 방식, 앞서나가는 포인터는 fast pointer, 늦게 나가는 포인터를 slow pointer라 한다.


# range sum(구간합)
구간합이란 구간 (i, j)의 합을 의미한다.


## 누적합을 이용한 구간합, O(n)
누적합이란 (0,i) 구간의 합을 의미한다. 구간 \[i,j\]의 합을 구하기 위해서 (0,j)의 누적합에서 (0,i-1)의 누적합을 빼주면 i,j의 구간합을 구할 수 있다.

### 소스코드
``` c
#include<iostream>

using namespace std;

int prefixSum[100001];

int main(void) {
	std::ios_base::sync_with_stdio(false);
	cin.tie(nullptr);
	cout.tie(nullptr);
	int N, M;

	cin >> N >> M;

	int sum = 0;
	for (int i = 1; i <= N; i++) {
		int num;
		cin >> num;
		sum += num;
		prefixSum[i] = sum;
	}

	for (int i = 0; i < M; i++) {
		int s, e;
		cin >> s >> e;
		cout << prefixSum[e] - prefixSum[s-1] << '\n';
	}

	return 0;
}
```

## segment tree를 이용한 구간합


### 소스코드
```c
#include<iostream>
#include<vector>
#define INF 1000000000+1
using namespace std;

int arr[100000];
int n;

class segment {
private:
	vector<int> tree;

public:
	segment() {
		tree.resize(4*n);
		init(1, 0, n-1);
	}

	int init(int node, int start, int end) {
		if (start == end) return tree[node] = arr[start];

		int mid = (start + end) / 2;

		return tree[node] = min(init(node * 2, start, mid), init(node * 2 + 1, mid + 1, end));
	}

	int query(int node, int start, int end, int rangeLeft, int rangeRight) {
		if (start > rangeRight || end < rangeLeft) return INF;
		if (rangeLeft <= start && end <= rangeRight) return tree[node];

		int mid = (start + end) / 2;

		return min(query(node * 2, start, mid, rangeLeft, rangeRight), query(node * 2 + 1, mid + 1, end, rangeLeft, rangeRight));
	}

	int query(int rangeLeft,int rangeRight) {
		return query(1, 0, n - 1, rangeLeft - 1, rangeRight - 1);
	}
};

int main(void) {
	std::ios_base::sync_with_stdio(false);
	cin.tie(nullptr);
	cout.tie(nullptr);

	int m;
	cin >> n >> m;

	for (int i = 0; i < n; i++)
		cin >> arr[i];

	segment seg;
	
	for (int i = 0; i < m; i++) {
		int a, b;
		cin >> a >> b;

		cout << seg.query(a, b) << '\n';
	}
	return 0;
}
```
## Fenwick tree를 이용한 구간합 O(logN)
펜윅 트리 오른쪽 자식이 없는 트리이다. 구간합, 누적합을 구하기 최적화 되어있다. 세그먼트에 비해 공간 활용도가 좋다(O(N+1)). 구현이 간단하다.

### 소스코드
```c

```

# divide and conquer 
### 개념
> 분할정복(divide and conquer)은 큰 문제를 작은 문제로 쪼개 해결하는 방법이다.

* divide : 문제를 하위문제로 분할. 하위 문제는 주어진 문제와 유사한 형태로 분할 되어야 하며 최소 2개 이상의 하위 문제로 분할 되어야한다.
* conquer : 하위 문제를 해결한다.
* combine : 해결된 하위 문제를 조합하여 상위 문제를 해결한다.

### 소스코드
```c
// 백준 1780
#include<iostream>
#define SIZE 20000

using namespace std;

int N;
int in[SIZE][SIZE];
int ans[3];
bool check(int y, int x, int n) {
	for (int dy = y; dy < y + n; dy++) {
		for (int dx = x; dx < x + n; dx++) {
			if (in[y][x] != in[dy][dx]) {
				return false;
			}
		}
	}

	return true;
}

void solution(int y, int x, int n) {
	if (check(y, x, n)) {// conquer
		ans[in[y][x]]++; 
	}
	else {
		int s = n / 3;

		for (int i = 0; i < 3; i++) {
			for (int j = 0; j < 3; j++)
				solution(y + i * s, x + j * s, n); //divide
		}
	}
}

int main(void) {
	cin >> N;

	for (int i = 0; i < N; i++)
		for (int j = 0; j < N; j++)
		{
			cin >> in[i][j];
			in[i][j]++;
		}
	solution(0, 0, 9);
	cout << ans[0] << endl << ans[1] << endl << ans[2];
	return 0;
}
```

<br></br>
# 문자열 search 알고리즘
문자열 search 알고리즘은 사용자가 원하는 문자열을 게시글 또는 긴 문자열에서 효율적으로 찾는 알고리즘이다.

## KMP Algorithm (Knuth-Morris-Pratt Algorithm)
커누스, 모리스, 플랫이 같이 개발한 string-Searching algorithm의 대표적인 알고리즘이다.

### 원리
#### 1
![image](https://user-images.githubusercontent.com/56042451/184581448-cb5571c4-84a7-4575-b47a-812770f1a729.png)
S의 0~3번 인덱스까지 전체 일치한다. 이후 계속 비교를 위해 S가 몇 번 인덱스로 이동해야할까? S의 2~3번 인덱스를 보면 S의 0~1번 인덱와 동일하므로 0번 -> 2번, 1번 -> 3번으로 이동하면 효율적으로 비교할 수 있을 것이다. 앞의 0~1번 AB가 접두사, 2~3이 접미사가 된다. 

#### 2
![image](https://user-images.githubusercontent.com/56042451/184581659-6e00ec21-a776-485c-962d-c4a025a4105f.png)
이것도 1번과 같이 앞에 2개가 일치하였다. 다음번 이동해야할 곳은 1번과 같다.

#### 3
![image](https://user-images.githubusercontent.com/56042451/184581742-d8cd2fc5-e603-4eb5-99c3-c346b76cc729.png)

#### 4
![image](https://user-images.githubusercontent.com/56042451/184582226-021c5fa9-2295-4693-9ac0-31b4b1bb8f30.png)

#### 5
![image](https://user-images.githubusercontent.com/56042451/184582120-68a696c5-2f48-49a9-8f20-ca2e5af05d2f.png)

#### 6
![image](https://user-images.githubusercontent.com/56042451/184582137-034d3c0f-233c-4ad0-91c4-d5352e10f33b.png)


### 개념
접두사와 접미사가 같은 경우 해당 접두사의 위치를 접미사로 옮겨 효율적으로 비교 가능하다.

#### prefix, suffix
> 찾으려는 문자열이 S라고 가정할 때, KMP 알고리즘에서 접두사, 접미사 
"abababaaa" 
