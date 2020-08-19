## backtracking

그래프에서 모든 정점(*Vertices*)을 방문하는 일을 그래프 순회(*Graph Traversal*)이라고 합니다. 그 중에서 원하는 특정 경로를 찾는 일을 그래프 탐색(*Graph Search*)라고 합니다. 생활에서 마주하는 문제를 그래프를 이용할 수 있도록 모델링 할 수 있기 때문에 그래프 탐색은 매우 유용한 알고리즘입니다.

1. 미로 찾기
<img src="https://user-images.githubusercontent.com/44011462/90377612-2d2e5380-e0b3-11ea-8821-0f4762a6b3ac.png" width="300px"><img src="https://user-images.githubusercontent.com/44011462/90377631-34edf800-e0b3-11ea-86f7-8d05dba48312.png" width="300px">

2. 최단 경로 찾기
<img src="https://user-images.githubusercontent.com/44011462/90378009-cb221e00-e0b3-11ea-8ad1-59e0dcd54e5f.png" width="300px">

3. 네비게이션
<img src="https://user-images.githubusercontent.com/44011462/90378371-3cfa6780-e0b4-11ea-87cd-878aa20882a5.png" width="300px">

4. TicTacToe
<img src="https://user-images.githubusercontent.com/44011462/90378495-66b38e80-e0b4-11ea-9966-b269c3d58588.png" width="300px">

5. 바둑(go)
<img src="https://user-images.githubusercontent.com/44011462/90378547-7337e700-e0b4-11ea-8f58-3a681d5beec0.png" width="300px">

위와 같은 그래프 탐색에는 대표적으로 1. **Depth First Search**, 2. **Breadth First Search**가 사용된다. DFS와 BFS는 각각 깊이와 너비를 우선적으로 탐색하는 방식을 의미한다. 위 두가지 방식은 아래의 GIF를 보면 직관적으로 이해할 수 있다. 

<img src="https://user-images.githubusercontent.com/44011462/61773130-d9f5bd00-ae2e-11e9-8bfc-41d13a269061.gif" width="600px">

<details>
    <summary><span style="color:grey">클릭하여 출처보기</span></summary>  
    https://www.uniquesoftwaredev.com/tag/dfs/
</details>

DFS와 BFS는 탐색을 우선시 하는 방식에 차이가 있고 그에 따른 특징이 다르게 나타납니다.
### Depth First Search(DFS)
1. 스택(Stack)을 이용 - 재귀(Recursion)
2. 더이상 확인할 길이 없을때까지 들어갔다가 나오며 경로를 순회한다.
3. 그래프에 싸이클이 있거나 깊이가 무한히 깊으면 적절하지 못하다.
4. Memory의 Heap 영역에 메모리를 적게 사용한다.

***N*** * ***N*** 크기의 2차원 배열에서 최단 경로를 찾는다고 할때,
시간복잡도 : ***O(2^N)***
공간복잡도 : ***O(N)***

```c++
그래프의 깊이 우선 탐색(DFS))
vector<vector<int>>adj;    // 그래프의 인접 리스트표현 
vector<bool> visited;   // 각 정점을 방문했는지 여부를나타낸다. 
 
void dfs(int here){     // 깊이 우선 탐색을 구현한다.
    cout << "DFS visits " << here << endl; 
    visited [here] = true; 
    for(int i = 0; i < adj[here].size(); ++i) {     // 모든 인접 정점을 순회하면서 
        int there = adj[here][i];   // 아직 방문한적 없다면 방문한다. 
        if( !visited[there]) 
            dfs(there); 
    } 
    // 더 이상 방문할 정점이 없으니, 재귀 호출을 종료하고 이전 정점으로 돌아간다. 
} 

void dfsAll() {     // 모든 정점을 방문한다. 
    visited = vector<bool>(adj.size(), false);   // visited를 모두 false로 초기화한다. 
    for(int i = 0; i < adj.size(); ++i) {    // 모든 정점을 순회하면서, 아직 방문한적 없으면 방문한다.
        if (!visited [i]) 
            dfs(i); 
    }
}
```

### Breadth First Search(BFS)
1. 큐(Queue)를 이용
2. 현재 위치에서 갈 수 있는 루트를 우선적으로 순회한다.
3. 최단 경로를 찾을 수 있는 방식이다.
4. Memory에 공간이 많이 필요하다.

***N*** * ***N*** 크기의 2차원 배열에서 최단 경로를 찾는다고 할때,
시간복잡도 : ***O(2^N)***
공간복잡도 : ***O(2^N)***

```c++
그래프의 너비 우선 탐색(BFS)
vector <vector<int>> adj; 	// 그래프의 인접 리스트표현 
vector<int> bfs(int start) { 	//start에서 시작해 그래프를 너비 우선 탐색하고 각 정점의 방문 순서를 반환한다. 
	vector<bool> discovered(adj.size(), false);		//각 정점의 방문 여부 
	queue<int> q; 	 	// 방문할정점 목록올 유지하는 큐 
	vector<int> order;		//정점의 방문순서 
	discovered[start] = true; 
	q.push(start);
	while(!q.empty()) {
		int here = q. front(); 
		q.pop();
		order.push_back(here);		//here를 방문한다.
		for(int i = 0; i < adj[here].size(); i++) {	// 모든 인접한 정점을 검사한다.
			int there = adj[here][i];
			if(!discovered[there]) { //처음 보는 정점이면 방문 목록에 집어 넣는다. 
				q.push(there); 
				discovered[there] = true; 
			}
		}
	}
	return order;
}
```

### 휴리스틱(heuristics)

백트래킹(**backtracking**)은 어느 지점에서 문제가 제약조건을 만족하지 못하는 부분을 제거해 나가면서 해답에 점차 가까워지는 문제 해결 전략이다. DFS와 유사하게 흐름이 진행되지만 **DFS와 Backtracking을 완전히 다르게** 볼 수 있는 것은 바로 **부분 문제(subproblem)을 제거**해 나간다는 **휴리스틱(heuristics)** 에 있다. backtracking에서의 휴리스틱은 **정답이 될 수 없는 것을 제외시키는 정도**로 사용된다. 

    어떤 문제가 있을 때 그 문제를 푸는 방법이 아직 없거나 현실적으로 불가능할 때, 혹은 
    문제를 풀기 위한 정보가 완전히 주어지지 않을 때, 
    확립된 절차에 따라 답을 구할 수 있을 정도로 문제가 명확하게 정의되지 않았을 때
    휴리스틱이 사용 가능하다.

<details>
    <summary><span style="color:grey">클릭하여 출처보기</span></summary>  
    위키백과 휴리스틱 이론<br>
    https://ko.wikipedia.org/wiki/%ED%9C%B4%EB%A6%AC%EC%8A%A4%ED%8B%B1_%EC%9D%B4%EB%A1%A0<br>
</details>


스도쿠(sudoku)문제를 푼다고 가정해보자. 빈공간에 숫자를 하나씩 채워나가다가 이미 정답이 될 수 없는 숫자배열이 나온다면 그 숫자를 지우고(backtrack) 다음숫자를 배치하면서 답을 찾아나간다.
모든 가능한 경우를 전부다 확인해보는 방식보다 더 빠르다. backtrack하면서 부분 문제(Subset)을 제거했기 때문이다. 


#### related problem - Baekjoon 9663. [N-Queen](https://www.acmicpc.net/problem/9663)

```c++
/*
 *   2020.8.18
 *   Baekjoon 9663. N-Queen
 */
#include <iostream>
#include <vector>
#include <utility>

using namespace std;

int input_size;
int answer = 0;
vector<vector<int>> board;
vector<vector<int>> queen;

void resize_input() {
    board.resize(input_size, vector<int>(input_size));
}

void print_answer() {
    cout << answer;
}

bool validator(int &row, int &col) {
    for (vector<int> &it : queen) {
        if (it[1] == col) return false;
        if (it[0] - it[1] == row - col) return false;
        if (it[0] + it[1] == row + col) return false;
    }
    return true;
}

void backtracking(int &depth) {
    if (depth + 1 == input_size) {
        answer++;
        return;
    }
    int next = depth + 1;
    for (int index = 0; index < input_size; ++index) {
        if (!validator(next, index)) continue;
        queen.push_back({next, index});
        backtracking(next);
        queen.pop_back();
    }
}

void solution() {
    resize_input();

    int depth = -1;
    backtracking(depth);

    print_answer();
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);

    cin >> input_size;
    solution();

    return 0;
}
```

#### related problem - baekjoon 2580. [Sudoku](https://www.acmicpc.net/problem/2580)

```c++
/*
 * 2020.8.18
 * Baekjoon 2580_스도쿠.cpp
 */
#include <iostream>
#include <vector>
#include <utility>

using namespace std;

vector<vector<int>> board;

void print_answer() {
    for (int row = 0; row < 9; ++row) {
        for (int col = 0; col < 9; ++col) {
            cout << board[row][col] << " ";
        }
        cout << "\n";
    }
}

bool check_row(int &col, int &target) {
    for (int row = 0; row < 9; ++row) {
        if (board[row][col] == target) return false;
    }
    return true;
}

bool check_col(int &row, int &target) {
    for (int col = 0; col < 9; ++col) {
        if (board[row][col] == target) return false;
    }
    return true;
}

bool check_box(int &row, int &col, int &target) {
    const int target_y = (row / 3) * 3;
    const int target_x = (col / 3) * 3;

    for (int dy = 0; dy < 3; ++dy) {
        for (int dx = 0; dx < 3; ++dx) {
            if (board[target_y + dy][target_x + dx] == target) return false;
        }
    }
    return true;
}

bool validator(int &pos, int &target) {
    int row = pos / 9;
    int col = pos % 9;
    return check_box(row, col, target) && check_row(col, target) && check_col(row, target);
}

void backtracking(int &pos) {
    if (pos == 80) {
        print_answer();
        // exit(0);
        return;
    }

    int next = pos + 1;
    int row = next / 9;
    int col = next % 9;

    if (board[row][col] != 0) {
        backtracking(next);
        return;
    }

    for (int val = 1; val < 10; ++val) {
        if (!validator(next, val)) continue;
        board[row][col] = val;
        backtracking(next);
        board[row][col] = 0;
    }
}

void solution() {
    int start = -1;
    backtracking(start);
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);

    board.resize(9, vector<int>(9));

    for (int row = 0; row < 9; ++row) {
        for (int col = 0; col < 9; ++col) {
            cin >> board[row][col];
        }
    }

    solution();

    return 0;
}

```

