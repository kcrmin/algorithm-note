---
type: template
tags: ["Graphs"]
aliases: ["Strongly Connected Components", "Kosaraju"]
category: connectivity-ordering
categories: [connectivity-ordering]
---

# SCC

```text
0 ⇄ 1 ──→ 2 ⇄ 3 ──→ 4

서로 왕복 가능한 덩어리로 묶기:
{0,1} ──→ {2,3} ──→ {4}

① 원본 그래프: DFS가 끝난 순서 기록
② 간선 뒤집기 + 종료 역순: 덩어리 하나씩 분리
```

| Step | Pseudocode |
|---|---|
| Finish | // 원본 그래프를 DFS하고 이웃 탐색이 끝난 뒤 현재 정점을 기록한다. |
| Reverse | // 기록의 뒤에서부터 아직 그룹이 없는 정점을 고른다. |
| Assign | // 역방향 그래프에서 도달하는 미배정 정점들에 같은 그룹 번호를 붙인다. |
| Count | // 새 그룹을 시작할 때마다 SCC 수를 하나 늘린다. |

## Java

```java
static List<Integer>[] graph, reverse;
static boolean[] visited;
static int[] component;
static List<Integer> finishOrder;

static void finish(int cur) {
    visited[cur] = true;

    for (int next : graph[cur]) {
        if (!visited[next]) {
            finish(next);
        }
    }

    // 모든 이웃 탐색이 끝난 뒤 기록한다.
    finishOrder.add(cur);
}

static void assign(int cur, int id) {
    component[cur] = id;

    for (int next : reverse[cur]) {
        if (component[next] == -1) {
            assign(next, id);
        }
    }
}

static int decompose(int n) {
    for (int v = 0; v < n; v++) {
        if (!visited[v]) {
            finish(v);
        }
    }

    int count = 0;

    for (int i = finishOrder.size() - 1; i >= 0; i--) {
        int start = finishOrder.get(i);
        if (component[start] == -1) {
            assign(start, count++);
        }
    }

    return count;
}
```

## Watch points

| 선택 | 이유 / 실수 |
|---|---|
| 진입 순서가 아닌 종료 순서 | 다음 덩어리를 먼저 끝내고 현재 덩어리를 뒤에 기록해야 탐색 출발 순서가 맞는다. |
| 종료 역순 + 역방향 그래프 | 원본의 앞쪽 덩어리부터 처리하면 역그래프에서 아직 미배정인 다른 덩어리로 새지 않는다. |
| 두 방문 상태 | 첫 DFS는 visited, 두 번째 DFS는 component=-1 여부를 사용한다. |
| 초기화와 깊이 | visited=false, component=-1, 빈 finishOrder. 긴 경로에서는 재귀 스택을 고려한다. |

원본 graph와 간선을 뒤집은 reverse를 모두 준비. `decompose(n)`은 SCC 수, component[v]는 그룹 번호. **O(V+E)**.

**분류** [[00 Maps/Connectivity and Ordering|Connectivity and Ordering]]

**사용 원리** [[30 Search/DFS and Graphs|DFS and Graphs]]
