---
type: template
tags: ["Graphs"]
---

# Dinic

```java
static boolean buildLevels(int source, int sink) {
    Arrays.fill(level, -1);
    Queue<Integer> queue = new ArrayDeque<>();
    level[source] = 0;
    queue.offer(source);
    while (!queue.isEmpty()) {
        int cur = queue.poll();
        for (Edge edge : graph[cur]) {
            if (edge.remaining <= 0 || level[edge.to] != -1) continue;
            level[edge.to] = level[cur] + 1;
            queue.offer(edge.to);
        }
    }
    return level[sink] != -1;
}
static long send(int cur, int sink, long limit) {
    if (cur == sink) return limit;
    for (; next[cur] < graph[cur].size(); next[cur]++) {
        Edge edge = graph[cur].get(next[cur]);
        if (edge.remaining <= 0 || level[edge.to] != level[cur] + 1) continue;
        long sent = send(edge.to, sink, Math.min(limit, edge.remaining));
        if (sent == 0) continue;
        edge.remaining -= sent;
        graph[edge.to].get(edge.reverse).remaining += sent;
        return sent; // ①
    }
    return 0;
}
static long maxFlow(int source, int sink) {
    long total = 0;
    while (buildLevels(source, sink)) {
        Arrays.fill(next, 0); // ②
        long sent;
        while ((sent = send(source, sink, Long.MAX_VALUE)) > 0) total += sent;
    }
    return total;
}
```

| 암기 포인트 | 이유 |
|---|---|
| 다음 층으로만 이동 | 층을 따라가면 사이클 없이 최단 증대 경로만 찾는다. |
| ① 성공한 간선의 next는 유지 | 같은 간선의 남은 용량을 다음 DFS에서도 사용한다. |
| ② BFS마다 next 초기화 | 층이 바뀌면 이전에 실패한 간선도 다시 쓸 수 있다. |

**예시**

```text
BFS층: s(0) → a,b(1) → t(2)
DFS는 level+1 간선만 이용 → 층 안 사이클 없음
현재 층에서 더 못 보냄 → BFS 재실행
next[v] = 이번 층에서 이미 실패한 간선은 다시 안 봄
```

**적용** Max Flow에서 한 BFS 층을 만든 뒤 그 층의 경로를 한꺼번에 소진한다.

**준비** `Max Flow`의 Edge/addEdge/graph 사용. `int[] level,next`는 정점 수 길이. `send`와 BFS 모두 필수다.

**주의** 일반 O(V²E) 시간, O(V+E) 공간. 긴 경로는 DFS 호출 깊이에 주의. 잔여 용량을 변경한다.

[[Max Flow]] · [[Edmonds-Karp]] · [[BFS]]
