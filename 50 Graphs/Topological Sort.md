---
type: template
tags: ["Graphs"]
---

# Topological Sort

```java
static int[] order(int n) {
    Queue<Integer> queue = new ArrayDeque<>();
    for (int v = 0; v < n; v++) {
        if (indegree[v] == 0) queue.offer(v);
    }
    int[] result = new int[n];
    int count = 0;
    while (!queue.isEmpty()) {
        int cur = queue.poll();
        result[count++] = cur;
        for (int next : graph[cur]) {
            if (--indegree[next] == 0) queue.offer(next); // ①
        }
    }
    return count == n ? result : null; // ②
}
```

| 암기 포인트 | 이유 |
|---|---|
| ① 남은 선행 작업 수 | 간선을 지울 때 감소하며 0이 된 순간에만 실행 가능하다. |
| ② 처리 수가 n보다 작음 | 남은 정점들이 사이클로 서로 기다린다. |

**예시**

```text
0 ─┐
   ├→ 2 → 3     indegree [0,0,2,1]
1 ─┘             0 처리: 2의 차수 2→1
                 1 처리: 2의 차수 1→0 → Queue에
```

**적용** 선행→후행 관계가 있는 작업을 선행 작업부터 나열한다. 전체 순서는 DAG에서만 가능하다.

**준비** `List<Integer>[] graph`, `indegree`는 같은 간선으로 만든다. 아래는 indegree를 소모한다.

**주의** 사전순 최소가 필요하면 Queue를 최소 Heap으로 바꾼다. O(V+E) 시간·공간.

[[SCC]] · [[DP]] · [[Heap]]
