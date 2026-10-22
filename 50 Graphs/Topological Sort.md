---
type: template
tags: ["Graphs"]
category: connectivity-ordering
categories: [connectivity-ordering]
---

# Topological Sort

```java
static List<Integer>[] graph;
static int[] indegree;

static int[] order(int n) {
    Queue<Integer> queue = new ArrayDeque<>();
    // ready: 선행 작업이 없는 정점 등록
    for (int v = 0; v < n; v++) {
        if (indegree[v] == 0) queue.offer(v);
    }
    int[] result = new int[n];
    int count = 0;
    while (!queue.isEmpty()) {
        // pop: 준비된 정점 하나 처리
        int cur = queue.poll();
        result[count++] = cur;
        for (int next : graph[cur]) {
            // unlock: 마지막 선행 작업이 끝난 정점 등록
            if (--indegree[next] == 0) queue.offer(next);
        }
    }
    // answer: 남은 정점이 있으면 사이클
    return count == n ? result : null;
}
```

선행→후행 `List<Integer>[] graph`, `indegree`=진입 차수(호출이 소모); 사이클이면 null, 사전순 최소는 최소 힙. O(V+E).

**분류** [[00 Maps/Connectivity and Ordering|Connectivity and Ordering]]
