---
type: template
tags: ["Graphs"]
aliases: ["Rooted Tree"]
category: traversal-trees
categories: [traversal-trees]
---

# Tree and Parent Array

```java
static List<Integer>[] graph;
static int[] parent, depth;

static void findParents(int root) {
    Queue<Integer> queue = new ArrayDeque<>();
    parent[root] = root; // 루트도 방문 처리해야 자식이 다시 넣지 않음
    queue.offer(root);
    while (!queue.isEmpty()) {
        int cur = queue.poll();
        for (int next : graph[cur]) {
            if (parent[next] != -1) continue;
            parent[next] = cur;
            depth[next] = depth[cur] + 1;
            queue.offer(next);
        }
    }
}
```

연결된 무방향 트리의 양방향 `List<Integer>[] graph`; `parent=-1`, `depth=0` 준비 후 `findParents(root)`. O(V).

**분류** [[00 Maps/Traversal and Trees|Traversal and Trees]]

**사용 원리** [[30 Search/BFS|BFS]]
