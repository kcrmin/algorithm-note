---
cssclasses:
  - algorithm-reference-sample
---

# Index

기억 단서 → 개념·조건 → Structure → Java 순서로 읽는다. 변형은 해당 노트의 섹션에서 고른다.

> [!reference] Java 사용
> 클래스·메서드 템플릿은 `Solution` 같은 바깥 클래스 안에 넣는다.
>
> 컬렉션은 `import java.util.*;`, 입출력은 `import java.io.*;`. Graph setup과 API 사용 예시는 메서드 안에서 실행한다.

---

## Java

| Note | 찾는 내용 |
| --- | --- |
| [[Java Essentials]] | 알고리즘을 구현할 때 자주 틀리는 Java 표현만 모은다 |
| [[Fast Input and Output]] | 숫자·토큰 입력이 많을 때 바이트 버퍼 사용 |
| [[Sorting and Comparator]] | 기본형 배열 정렬과 객체의 다중 기준 정렬 |
| [[HashMap and HashSet]] | 값으로 기록을 조회하고 중복·빈도를 관리 |
| [[ArrayDeque]] | Java의 Stack·Queue·Deque를 ArrayDeque로 사용 |
| [[PriorityQueue]] | 매번 가장 작은 값 또는 가장 큰 값을 꺼낸다 |
| [[TreeMap and TreeSet]] | 값의 순서와 이웃 조회가 필요한 집합·중복 집합 |
| [[Grid Utilities]] | 격자 탐색에서 방향·범위·회전 표현을 일정하게 유지 |

---

## Search

| Note | 찾는 내용 |
| --- | --- |
| [[Binary Search]] | First / Last True · Lower / Upper Bound |
| [[Two Pointers]] | 서로 다른 두 원소의 합이 `target`인지 검사 |
| [[Sliding Window]] | 연속 구간의 상태를 유지하며 양쪽 경계를 한 방향으로 이동 |
| [[Backtracking]] | 조건에 맞는 선택을 모두 생성 |
| [[Bitmask]] | 작은 집합을 정수 하나로 표현 |
| [[Meet in the Middle]] | 합이 target인 부분집합 개수 |
| [[Next Permutation]] | 현재 배열을 바로 다음 사전순 순열로 바꾼다 |

---

## Ranges

| Note | 찾는 내용 |
| --- | --- |
| [[Prefix Sum]] | 값이 바뀌지 않는 배열의 구간 합 |
| [[Prefix Sum with Hash Map]] | 합이 `target`인 연속 부분 배열의 개수·존재 여부 |
| [[Difference Array]] | 여러 구간에 값을 더한 뒤 최종 배열을 한 번 구한다 |
| [[Coordinate Compression]] | 큰 값·음수를 순서를 유지하는 작은 인덱스로 바꾼다 |
| [[Fenwick Tree]] | 값이 바뀌는 배열의 누적합·구간 합 |
| [[Segment Tree]] | 점 갱신과 구간의 합·최솟값·최댓값 |
| [[Lazy Segment Tree]] | 구간 전체에 더하기와 구간 합 질의를 함께 처리 |
| [[Sparse Table]] | 갱신 없는 배열의 구간 최솟값을 O(1)에 질의 |

---

## Data Structures

| Note | 찾는 내용 |
| --- | --- |
| [[Monotonic Stack]] | 각 위치에서 가장 가까운 더 큰 값·작은 값의 인덱스 |
| [[Monotonic Deque]] | 길이 k인 모든 연속 구간의 최댓값·최솟값 |
| [[Heap with Lazy Deletion]] | ID별 우선순위 갱신·삭제를 지원하는 최소 힙 |

---

## Intervals

| Note | 찾는 내용 |
| --- | --- |
| [[Interval Scheduling]] | 겹치지 않는 구간을 최대 개수 선택 |
| [[Interval Merging]] | 겹치거나 맞닿는 닫힌 구간을 합친다 |
| [[Sweep Line]] | 동시에 활성인 구간의 최대 개수 |

---

## Graphs

| Note | 찾는 내용 |
| --- | --- |
| [[BFS]] | 동일한 간선 비용에서 최단 거리 |
| [[DFS]] | 깊이 우선 탐색, 연결 요소와 사이클 판정 |
| [[Union-Find]] | 집합 합치기와 같은 집합인지 확인 |
| [[Topological Sort]] | 방향 그래프에서 모든 선행 조건을 지키는 순서 |
| [[Bipartite Graph Check]] | 무방향 그래프를 두 집합으로 나눠 모든 간선이 집합 사이에만 있는지 검사 |
| [[Dijkstra]] | 한 시작점에서 모든 정점까지의 최단 거리 |
| [[Floyd-Warshall]] | 모든 정점 쌍의 최단 거리 |
| [[Kruskal]] | 무방향 그래프의 최소 신장 트리 |
| [[0-1 BFS]] | 가중치가 정확히 `0` 또는 `1`인 최단 거리 |
| [[Bellman-Ford]] | 음수 간선이 있는 방향 그래프의 최단 거리와 음수 사이클 검사 |
| [[SCC]] | 방향 그래프에서 서로 왕복 가능한 정점들을 하나로 묶는다 |
| [[2-SAT]] | 두 literal의 OR로 된 조건들을 모두 만족하는 참·거짓 배정 |
| [[Bipartite Matching]] | 왼쪽·오른쪽 정점을 각각 한 번만 사용하는 최대 매칭 |
| [[Max Flow and Min Cut]] | 용량 제한 안에서 source→sink 최대 유량과 최소 절단 |
| [[Bridges and Articulation Points]] | 무방향 그래프에서 제거하면 연결 성분 수가 늘어나는 간선·정점 |
| [[Eulerian Trail]] | 모든 간선을 정확히 한 번 사용하는 경로 |

---

## Trees

| Note | 찾는 내용 |
| --- | --- |
| [[Rooted Tree Traversal]] | 루트를 기준으로 부모·깊이·처리 순서·서브트리 크기를 만든다 |
| [[Tree Diameter]] | 트리에서 가장 긴 단순 경로의 길이 |
| [[LCA and Binary Lifting]] | 최소 공통 조상, k번째 조상, 두 정점 사이 간선 수 |
| [[Tree Flattening]] | 트리의 서브트리를 배열의 연속 구간으로 바꾼다 |

---

## DP

| Note | 찾는 내용 |
| --- | --- |
| [[Maximum Subarray - Kadane]] | 연속 부분 배열의 최대 합 |
| [[Grid DP]] | 오른쪽·아래로만 이동하는 격자의 경로 수 또는 최소 비용 |
| [[Knapsack DP]] | 용량 이하에서 얻을 수 있는 최대 가치 |
| [[Coin Change]] | 동전을 무제한 사용해 정확히 amount를 만든다 |
| [[LIS]] | 원래 순서를 유지하는 가장 긴 증가 부분 수열 |
| [[LCS]] | 두 문자열에서 순서를 유지하는 최장 공통 부분 수열 |
| [[Edit Distance]] | 삽입·삭제·교체로 a를 b로 바꾸는 최소 연산 수 |
| [[Interval DP]] | 짧은 구간의 결과로 큰 구간을 계산 |
| [[Bitmask DP]] | 0번에서 출발해 모든 정점을 한 번씩 방문하고 돌아오는 최소 비용: TSP |
| [[Tree DP]] | 서로 인접한 정점을 함께 고르지 않는 최대 가중치 독립 집합 |
| [[DAG DP]] | DAG의 시작점에서 각 정점까지 최장 거리 또는 경로 수 |

---

## Strings

| Note | 찾는 내용 |
| --- | --- |
| [[Trie]] | 문자열 저장·검색과 접두사 검사 |
| [[KMP]] | 문자열에서 패턴의 모든 시작 위치 찾기 |
| [[Rolling Hash]] | 부분 문자열의 해시를 O(1)에 구한다 |
| [[Manacher]] | 모든 중심의 최대 팰린드롬 반지름을 O(n)에 구한다 |

---

## Math

| Note | 찾는 내용 |
| --- | --- |
| [[GCD and LCM]] | 최대공약수와 최소공배수 |
| [[Prime Sieve and Factorization]] | 0..n의 소수 판정 또는 여러 수의 소인수분해를 전처리 |
| [[Modular Arithmetic]] | 큰 지수의 거듭제곱과 소수 모듈러에서 나눗셈 처리 |
| [[Binomial Coefficient]] | 순서를 구분하지 않고 n개 중 k개를 고르는 경우의 수 |
| [[Matrix Exponentiation]] | 같은 선형 전이를 매우 많이 반복할 때 전이 행렬을 거듭제곱 |

---

## Geometry

| Note | 찾는 내용 |
| --- | --- |
| [[CCW and Segment Intersection]] | 세 점의 방향과 두 선분의 교차 판정 |
| [[Convex Hull]] | 모든 점을 포함하는 가장 작은 볼록 다각형의 꼭짓점 |

