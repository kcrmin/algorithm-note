---
type: template
tags: ["Graphs"]
category: flow-matching
categories: [flow-matching]
---

# Dinic

```java
static int[] level, next;
// buildLevels: 잔여 간선 BFS로 층 만들기
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
// send: 다음 층으로 따라가 병목만큼 전송
static long send(int cur, int sink, long limit) {
    if (cur == sink) return limit;
    // cursor: 실패한 간선은 넘기고, 성공한 간선은 재사용
    for (; next[cur] < graph[cur].size(); next[cur]++) {
        Edge edge = graph[cur].get(next[cur]);
        if (edge.remaining <= 0 || level[edge.to] != level[cur] + 1) continue;
        long sent = send(edge.to, sink, Math.min(limit, edge.remaining));
        if (sent == 0) continue;
        edge.remaining -= sent;
        graph[edge.to].get(edge.reverse).remaining += sent;
        return sent;
    }
    return 0;
}
static long maxFlow(int source, int sink) { // maxFlow: 층 만들기 → 막힐 때까지 전송 반복
    long total = 0;
    while (buildLevels(source, sink)) {
        Arrays.fill(next, 0); // reset: 새 층은 처음부터 탐색
        long sent;
        while ((sent = send(source, sink, Long.MAX_VALUE)) > 0) total += sent;
    }
    return total;
}
```

[[Max Flow]]의 `Edge`·`graph` 사용; `level`·`next`는 길이 V 배열, source≠sink; 잔여 용량을 변경, 깊은 경로는 재귀 스택 주의. O(V²E).

**분류** [[00 Maps/Flow and Matching|Flow and Matching]]

**필요 코드** [[50 Graphs/Max Flow|Max Flow]]
