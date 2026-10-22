---
type: template
tags: ["Graphs"]
---

# Dinic

```java
// Max Flow의 Edge/addEdge/graph 사용. source≠sink.
// level[v]=BFS 거리, next[v]=다음 검사 간선. 두 배열의 길이는 V.
// remaining=잔여 용량, reverse=짝 역간선의 인덱스.
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
        // 다음 층만 따라가 최단 증대 경로를 찾음
        if (edge.remaining <= 0 || level[edge.to] != level[cur] + 1) continue;
        // 경로의 가장 작은 잔여 용량만 전송 가능
        long sent = send(edge.to, sink, Math.min(limit, edge.remaining));
        if (sent == 0) continue;
        edge.remaining -= sent;
        // 이전 전송을 취소할 수 있는 양도 함께 증가
        graph[edge.to].get(edge.reverse).remaining += sent;
        return sent; // 성공한 간선의 next는 유지해 남은 용량을 다음 DFS에서도 사용
    }
    return 0;
}
static long maxFlow(int source, int sink) {
    long total = 0;
    while (buildLevels(source, sink)) {
        // 층을 다시 만들면 이전에 실패한 간선도 다시 후보가 됨
        Arrays.fill(next, 0);
        long sent;
        while ((sent = send(source, sink, Long.MAX_VALUE)) > 0) total += sent;
    }
    return total;
}
```

**예시**

```text
BFS층: s(0) → a,b(1) → t(2)
DFS는 level+1 간선만 이용 → 층 안 사이클 없음
현재 층에서 더 못 보냄 → BFS 재실행
next[v] = 이번 층에서 이미 실패한 간선은 다시 안 봄
```

**주의** 일반 O(V²E) 시간, O(V+E) 공간. 긴 경로는 DFS 호출 깊이에 주의. 잔여 용량을 변경한다.

[[Max Flow]] · [[Edmonds-Karp]] · [[BFS]]
