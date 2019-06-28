
# Topological Sort
# Directed acyclic graph

```
ios_base::sync_with_stdio(false); cin.tie(NULL);

```
https://www.acmicpc.net/problem/2252

## 책안의 구조
DFS 사용
 Cormen et al. (2001); Tarjan (1976)이 제안
```
#include<iostream>
#include<stack>
#include<algorithm>
#include<vector>
using namespace std;
constexpr int MAX = 320000;
stack<int> S; // 실제 정렬된값이 역순으로 들어가있다.

std::vector<std::vector<int>> Graph; // 인접 노드
bool visit[MAX+2] = { 0, };
```

```
void topologicalsort()
{
	for (std::size_t i = 1; i < Graph.size(); i++)
	{
		if (visit[i] != true)
		{
			DFS_TS(i);
		}
	}
}
```




```
void DFS_TS(int x)
{
	visit[x] = true;

	std::stack<int> sub_s;
	sub_s.push(x);
	//std::cout << sub_s.top() << ' '; // 순회출력

	while (!sub_s.empty())
	{
		int y = sub_s.top();
		for (std::size_t i = 0; i < Graph[y].size(); i++)
		{
			int next = Graph[y][i];
			if (visit[next] != true)
			{
				visit[next] = true;
				sub_s.push(next);
				//std::cout << sub_s.top() << ' '; // 순회출력
				i = -1;
				y = sub_s.top();
			}

		}
		S.push(sub_s.top());
		sub_s.pop();
		
	}
}
```

```
void DFS_TS(int x)
{
	visit[x] = true;
	//std::cout << x << ' '; // 순회출력

	for (std::size_t i = 0; i < Graph[x].size(); i++)
	{
		int next = Graph[x][i];
		if (visit[next] != true)
		{
			DFS_TS(next);
			
		}
	}
	S.push(x);
}
```
함수 스택이 가장 빠르다.

기존 DFS와 차이는 마지막 sub_s.pop()하기전에 
값을 S에 넣는 것이다.

```
int main()
{
	int n, m;
	std::cin >> n >> m;
	Graph.resize(n + 1);
	for (int i = 0; i < m; i++)
	{
		int u, v;
		std::cin >> u >> v;
		Graph[u].push_back(v);
	}

	for (int i = 1; i <= n; i++)  // 리스트 넘버순 정렬
		std::sort(Graph[i].begin(), Graph[i].end());
	topologicalsort();

	while (!S.empty())
	{
		std::cout << S.top() << ' ';
		S.pop();
	}
	return 0;
}
//앆
```

## Kahn's algorithm
참고 : https://blog.naver.com/PostView.nhn?blogId=ndb796&logNo=221236874984&proxyReferer=https%3A%2F%2Fwww.google.com%2F


```
#include<iostream>
#include<algorithm>
#include<vector>
#include<queue>
using namespace std;
std::vector<std::vector<int>> Graph; // 인접 노드
std::vector<int> DAG; // 위상정렬
```


```

void topologicalsort(int v)
{
	std::vector<int> indegree;
	indegree.resize(v + 1);
	std::queue<int> qu;
	for (int i = 1; i <= v; i++)
	{
		int e = Graph[i].size();
		for (int j = 0; j < e; j++)
		{
			indegree[Graph[i][j]]++;
		}
	}

	for (int i = 1; i <= v; i++)
	{
		if (indegree[i] == 0)
		{
			qu.push(i);
		}
	}
	for (int i = 1; i <= v; i++)
	{
		if (qu.empty())
		{
			return;
		}
		int x = qu.front();
		qu.pop();
		DAG.push_back(x);
		int x_size = Graph[x].size();
		for (int i = 0; i < x_size ; i++)
		{
			int y = Graph[x][i];
			indegree[y]--;
			if (indegree[y] == 0)
			{
				qu.push(y);
			}
		}
	}
}


```


```

int main()
{
	int n, m;
	std::cin >> n >> m;
	Graph.resize(n + 1);
	for (int i = 0; i < m; i++)
	{
		int u, v;
		std::cin >> u >> v;
		Graph[u].push_back(v);
	}

	for (int i = 1; i <= n; i++)  // 리스트 넘버순 정렬
		std::sort(Graph[i].begin(), Graph[i].end());
	topologicalsort(n);

	for (auto a: DAG)
	{
		std::cout << a << ' ';
	}
	
	return 0;
}
//앆
```