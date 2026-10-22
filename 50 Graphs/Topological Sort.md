---
type: template
tags: ["Graphs"]
---

# Topological Sort

```java
// 선행→후행 방향의 List<Integer>[] graph. indegree=아직 남은 선행 작업 수이며
// 이 함수가 소모한다.
static int[] order(int n) {
    Queue<Integer> queue = new ArrayDeque<>();
    for (int v = 0; v < n; v++) {
        // 기다릴 선행 작업이 없는 정점부터 시작
        if (indegree[v] == 0) queue.offer(v);
    }
    int[] result = new int[n];
    int count = 0;
    while (!queue.isEmpty()) {
        int cur = queue.poll();
        result[count++] = cur;
        for (int next : graph[cur]) {
            // 마지막 선행 작업이 끝난 순간에만 실행 가능
            if (--indegree[next] == 0) queue.offer(next);
        }
    }
    // 남은 정점이 있으면 사이클로 서로 기다리는 상태
    return count == n ? result : null;
}
```

**예시**

```text
0 ─┐
   ├→ 2 → 3     indegree [0,0,2,1]
1 ─┘             0 처리: 2의 차수 2→1
                 1 처리: 2의 차수 1→0 → Queue에
```

**주의** 사전순 최소가 필요하면 Queue를 최소 Heap으로 바꾼다. O(V+E) 시간·공간.

[[SCC]] · [[DP]] · [[Heap]]
