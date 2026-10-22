---
type: template
tags: ["Graphs"]
category: connectivity-ordering
categories: [connectivity-ordering]
---

# Topological Sort

```text
A ──→ C ──→ D
B ────┘

남은 선행 작업 수: A=0, B=0, C=2, D=1
A 처리 → C:2→1
B 처리 → C:1→0 → C 처리 → D:1→0

가능한 순서: A, B, C, D
```

| Step | Pseudocode |
|---|---|
| Ready | // 선행 작업이 없는 정점을 큐에 넣는다. |
| Take | // 준비된 정점 하나를 꺼내 결과에 기록한다. |
| Unlock | // 후행 정점의 남은 선행 작업 수를 줄이고 0이 되면 큐에 넣는다. |
| Check | // 모든 정점을 처리했으면 순서를, 남은 정점이 있으면 실패를 반환한다. |

## Java

```java
static List<Integer>[] graph;
static int[] indegree;
static int[] order(int n) {
    Queue<Integer> queue = new ArrayDeque<>();

    for (int v = 0; v < n; v++) {
        if (indegree[v] == 0) {
            queue.offer(v);
        }
    }

    int[] result = new int[n];
    int count = 0;

    while (!queue.isEmpty()) {
        int cur = queue.poll();
        result[count++] = cur;

        for (int next : graph[cur]) {
            // 지금 끝난 선행 작업을 빼고 준비 여부를 확인.
            if (--indegree[next] == 0) {
                queue.offer(next);
            }
        }
    }

    return count == n ? result : null;
}
```

## Watch points

| 선택 | 이유 / 실수 |
|---|---|
| 간선 방향 | 선행→후행으로 넣는다. 반대로 넣으면 작업 순서도 반대가 된다. |
| `--indegree[next] == 0` | 현재 작업이 끝났으므로 먼저 하나 줄이고 준비 완료인지 검사한다. |
| `count < n` | 선행 조건이 서로 순환하면 어느 정점도 0이 되지 않는다. 남은 정점은 사이클 자체 또는 그 뒤에 있다. |
| Queue / Min Heap | 아무 유효 순서면 큐. 가능한 순서 중 사전순 최소는 최소 힙을 쓴다. |

`indegree`는 원래 진입 차수로 초기화하며 호출 중 감소한다. 반환: 순서 배열, 사이클이 있으면 null. **O(V+E)**.

**분류** [[00 Maps/Connectivity and Ordering|Connectivity and Ordering]]
