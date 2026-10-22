---
type: template
tags: ["Graphs"]
category: connectivity-ordering
categories: [connectivity-ordering, flow-matching]
---

# Bipartite Graph

```java
static List<Integer>[] graph;
static int[] color;

static boolean isBipartite(int n) {
    // restart: 끊긴 부분마다 새 색칠 시작
    for (int start = 0; start < n; start++) {
        if (color[start] != 0) continue;
        Queue<Integer> queue = new ArrayDeque<>();
        color[start] = 1;
        queue.offer(start);
        while (!queue.isEmpty()) {
            int cur = queue.poll();
            for (int next : graph[cur]) {
                // conflict: 간선 양 끝이 같은 색이면 실패
                if (color[next] == color[cur]) return false;
                if (color[next] != 0) continue;
                // paint: 반대 색을 정한 뒤 큐에 등록
                color[next] = -color[cur];
                queue.offer(next);
            }
        }
    }
    return true;
}
```

무방향 `List<Integer>[] graph`, `color=new int[n]`(0=미방문, ±1=그룹); 끊긴 그래프도 판정. O(V+E).

**분류** [[00 Maps/Connectivity and Ordering|Connectivity and Ordering]]

**사용 원리** [[30 Search/BFS|BFS]]
