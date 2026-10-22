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
    for (int start = 0; start < n; start++) {
        if (color[start] != 0) continue;
        Queue<Integer> queue = new ArrayDeque<>();
        color[start] = 1;
        queue.offer(start);
        while (!queue.isEmpty()) {
            int cur = queue.poll();
            for (int next : graph[cur]) {
                if (color[next] == color[cur]) return false; // 같은 색 간선은 두 그룹 배정 모순
                if (color[next] != 0) continue;
                color[next] = -color[cur]; // 간선 양 끝은 반대 그룹
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
