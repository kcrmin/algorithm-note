---
type: template
tags: ["Search"]
aliases: ["DFS", "Depth-First Search"]
category: traversal-trees
categories: [traversal-trees]
---

# DFS and Graphs

```text
0 — 1 — 2       3 — 4       5

DFS(0): 0 → 1 → 2
DFS(3): 3 → 4
DFS(5): 5

새 출발 3번 = 연결 요소 3개
```

| Step | Pseudocode |
|---|---|
| Mark | // 현재 정점을 방문했다고 먼저 표시한다. |
| Dive | // 아직 방문하지 않은 이웃으로 끝까지 들어간다. |
| Restart | // 전체 정점 중 미방문 정점을 찾으면 새 DFS를 시작한다. |
| Answer | // 새 DFS를 시작한 횟수가 연결 요소 수다. |

## Java

```java
static List<Integer>[] graph;
static boolean[] visited;

static void dfs(int cur) {
    // 이웃으로 가기 전에 방문 표시.
    visited[cur] = true;

    for (int next : graph[cur]) {
        if (!visited[next]) {
            dfs(next);
        }
    }
}

static int countComponents(int n) {
    int count = 0;

    for (int start = 0; start < n; start++) {
        if (visited[start]) {
            continue;
        }

        count++;
        dfs(start);
    }

    return count;
}
```

## Watch points

| 선택 | 이유 / 실수 |
|---|---|
| 방문 표시 → 재귀 | 무방향 간선으로 부모에게 되돌아가며 무한 재귀하는 것을 막는다. |
| 전체 정점에서 시작 검사 | 한 번의 DFS는 출발점과 연결된 부분만 방문한다. 고립 정점도 하나의 요소다. |
| 무방향 그래프 | 방향 그래프의 강한 연결 요소는 SCC로 구한다. |
| 재귀 깊이 | 긴 일자 경로에서는 Java 호출 스택이 넘칠 수 있어 명시적 스택을 쓴다. |

양방향 `graph`, `visited=new boolean[n]`. `countComponents(n)`은 방문 배열을 변경한다. **O(V+E)**.

**분류** [[00 Maps/Traversal and Trees|Traversal and Trees]]
