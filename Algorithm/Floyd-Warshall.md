# Floyd-Wharshall

#### 모든 쌍 최단 경로를 찾는 Dynamic Programming 알고리즘

- 최단 경로(Shortest Path)
  - 한 정점에서 다른 한 정점으로 이동하는 cost가 가장 적은 경로
    - 가중치가 없는 그래프의 경우, 최단경로는 BFS를 통해 구한다.
  - brute-force 접근 방법
    - 한 정점에서 다른 정점으로 이동하는 모든 경로를 찾아 그 중 최단 경로를 찾는다.
    - 시간복잡도는 지수 복잡도 이상으로, 매우 비효율적. (최악의 경우 O(n!)의 시간복잡도를 가진다.)
  - **Dijkstra**의 최단 경로 알고리즘 사용
    - 인접행렬을 사용할 시, O(n<sup>3</sup> )의 시간복잡도. (n은 정점의 수)

- 모든 쌍 최단 경로
  - 각 쌍의 점 사이의 최단 경로를 찾는 문제



#### Floyd-Wharshall 알고리즘

- 모든 쌍 최단 경로를 찾는 Dynamic Programming 알고리즘
  - 먼저 **부분 문제들을 찾아야 한다**.
    - 그래프의 점의 수가 적을 때, 그래프에 3개의 점이 있는 경우
- O(n<sup>3</sup> )의 시간복잡도를 가지며, 다익스트라의 시간복잡도와 동일하다.
  - 구체적으로는 다익스트라 알고리즘을 n-1회 사용할 때와 동일. (n-1)*O(n<sup>2</sup>)=O(n<sup>3</sup>)
  - 알고리즘을 코드로 간단히 구현할 수 있어 다익스트라보다 사용이 쉽다. 
    - 삼중 for문을 사용하여 O(n<sup>3</sup> )이 된다.
- 다익스트라와 달리 **음수의 가중치에 대해서도 작동**한다.

---

#### DP  접근 방법

완전탐색으로는 시간복잡도가 너무 크므로, **부분 문제를 찾아야 한다**. (각 정점을 시작 정점으로 정하여 Dijkstra의 최단 경로 알고리즘을 수행하면 된다. )

##### 부분문제 정의

D<sub>ij</sub><sup>k</sup> = 정점 {1,2,...,k}만을 경유 가능한 정점들로 고려하여, 정점 i에서 j까지 이동하는 모든 경로 중 **가장 짧은 경로의 거리**

- **k≠i** : k는 경유를 할 수도 있는 점을 뜻하므로, “시작점≠경유점”
- **k≠j** : k는 경유를 할 수도 있는 점을 뜻하므로, “도착점≠경유점”
- **i≠j** : “시작점≠도착점
  - 보통 i=j인 경우의 weight를 0으로 두기 때문에, 이 경우 코드에서 i≠j 일때의 if문을 생략해도 결과가 같다.

- 1~k까지의 정점을 모두 고려. 
  - 1~k 까지 전부 지나가는 것 아님! 
    - 4번 정점을 지나가는 경로를 고려하고 4번 정점으로 가지 않는 경로를 택할 수 있다.
  - k만 경유하는 것 아님!

<br>

그래프의 정점의 수가 가장 적을 때부터 생각해보자. 정점의 수 n에 대하여, 

n = 2이면, 간선이 하나뿐이므로, 이 간선비용이 곧 두 정점간의 최소비용이 된다.

- D<sub>ij</sub> <sup>0</sup> 은 문제에서 입력으로 주어지는 간선 (i, j)의 가중치이다. 

n = 3인 경우, (가장 작은 부분 문제 (D<sub>ij</sub> <sup>1</sup>)) 

![부분 문제 원리](https://taegyunwoo.github.io/assets/img/2021-07-23-ALGORITHM_DP_AllPairsShortestPaths/Untitled%2052.png)점 i에서 점 j로 이동하는 경로는 (i -> j : 직접 경로), (i -> 1 -> j: 간접 경로)의 두 경로가 있다. 

점 i에서 점 j로 이동하는 최단 경로는 이 중 비용이 더 적은 것을 선택하면 된다.

=> 정점 1에서 시작하여, 정점을 1개씩 늘리면서, (정점 1,2, 그 다음 정점 1,2,3, ....) 마지막 정점 1~n까지의 모든 정점을 경유 가능한 정점으로 고려하여, 모든 쌍의 최단 경로의 거리를 계산.



점 개수가 k인 경우의 해 : (i, j)의 최단 경로

- ‘점i에서 점j로 직접가는 경우’

- ‘점k을 경유하여 가는 경우’

- 총 2가지의 경우 중, 짧은 것을 선택하면 됨

  ![점의 개수가 k개인 경우의 해](https://taegyunwoo.github.io/assets/img/2021-07-23-ALGORITHM_DP_AllPairsShortestPaths/Untitled%2054.png)

즉,  ![{D_{ij}}^k=min({D_{ij}}^{k-1},&space;({D_{ik}}^{k-1}+{D_{kj}}^{k-1}))](https://latex.codecogs.com/svg.image?{D_{ij}}^k=min({D_{ij}}^{k-1},&space;({D_{ik}}^{k-1}+{D_{kj}}^{k-1}))) 이다. 

- **D<sub>ik</sub> <sup>k-1</sup> +D<sub>kj</sub> <sup>k-1</sup>  에서, D가 큰 값이 들어갈 때 오버플로우에 대한 고려가 필요할 수 있다.**



#### Pseudo Code

![1711589752867](C:\Users\SSAFY\OneDrive\SSAFY\tassel\Algorithm\1711589752867.png)



먼저 D<sub>ij</sub><sup>k</sup> 을 만든다.

D는  row i개,  column j개의 2차원 배열.

​	i=j일때 D<sub>ij</sub> = 0; 연결되지 않은 간선값은 가중치를 무한대로 초기화.

​		여기서 무한대값은 Integer.MAX_VALUE  와 같은 값이 아니라, 가능한 큰 값을 쓴다. (오버플로우가 일어나지 않는 값)

​	k에 대해 for문을 돌려 D<sub>10</sub><sup>k</sup>  의 값을 업데이트.

 

#### JAVA Code

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

/*
sample input(첫 번째 숫자는 노드의 개수, 두 번째 숫자는 간선의 개수 이다).
5
8
0 1 5
0 4 1
0 2 7
0 3 2
1 2 3
1 3 6
2 3 10
3 4 4
 */
public class 플로이드 {
	static int N, M;
	static int[][] dist;

	public static void main(String[] args) throws NumberFormatException, IOException {
		// 초기화
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		N = Integer.parseInt(br.readLine());
		M = Integer.parseInt(br.readLine());
		// 플로이드 초기 거리 테이블 초기화
		dist = new int[N][N];
		for (int i = 0; i < N; i++) {
			for (int j = 0; j < N; j++) {
				// 자기 자신으로 가는 길은 최소 비용이 0이다.
				if (i == j) {
					dist[i][j] = 0;
					continue;
				}
				// 자기 자신으로 가는 경우를 제외하고는 매우 큰 값(N개의 노드를 모두 거쳐서 가더라도 더 큰 값).
				dist[i][j] = 100_000_000;
			}
		}

		for (int i = 0; i < M; i++) {
			StringTokenizer st = new StringTokenizer(br.readLine());
			int a = Integer.parseInt(st.nextToken());
			int b = Integer.parseInt(st.nextToken());
			int cost = Integer.parseInt(st.nextToken());

			// 가는 경로가 하나가 아닐 수 있다. 따라서 그 중 최소 비용을 저장해두면 된다.
			dist[a][b] = Math.min(dist[a][b], cost);
			dist[b][a] = Math.min(dist[b][a], cost);
		}

		// 플로이드 워셜 알고리즘
		// 노드를 1개부터 N개까지 거쳐가는 경우를 모두 고려한다.
		for (int k = 0; k < N; k++) {
			// 노드 i에서 j로 가는 경우.
			for (int i = 0; i < N; i++) {
				for (int j = 0; j < N; j++) {
					// k번째 노드를 거쳐가는 비용이 기존 비용보다 더 작은 경우 갱신
					// 또는 연결이 안되어있던 경우(INF) 연결 비용 갱신.
					dist[i][j] = Math.min(dist[i][j], dist[i][k] + dist[k][j]);
				}
			}
		}

		// 출력
		for (int i = 0; i < N; i++) {
			for (int j = 0; j < N; j++) {
				// 연결이 안되어 있는 경우
				if (dist[i][j] == 100_000_000) {
					System.out.print("INF ");
				} else {
					System.out.print(dist[i][j] + " ");
				}
			}
			System.out.println();
		}
	}
}

/* 결과
 * 0 5 7 2 1
 * 5 0 3 6 6
 * 7 3 0 9 8 
 * 2 6 9 0 3
 * 1 6 8 3 0
 */

```

