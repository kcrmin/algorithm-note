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

// findParents: 루트 기준 부모·깊이를 BFS로 채우기
static void findParents(int root) {
    Queue<Integer> queue = new ArrayDeque<>();
    // root: 루트도 방문 처리하도록 부모를 자기 자신으로
    parent[root] = root;
    queue.offer(root);
    while (!queue.isEmpty()) {
        int cur = queue.poll();
        for (int next : graph[cur]) {
            if (parent[next] != -1) continue;
            // link: 처음 만난 정점의 부모·깊이 기록
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
