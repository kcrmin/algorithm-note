---
type: template
tags: ["Graphs"]
aliases: ["Rooted Tree"]
---

# Tree and Parent Array

```java
// 연결된 무방향 Tree의 양방향 List<Integer>[] graph. parent=-1, depth=0으로 준비.
static void findParents(int root) {
    Queue<Integer> queue = new ArrayDeque<>();
    parent[root] = root; // 루트도 먼저 방문 처리해야 자식이 루트를 다시 넣지 않음
    queue.offer(root);
    while (!queue.isEmpty()) {
        int cur = queue.poll();
        for (int next : graph[cur]) {
            // 무방향 간선에서 부모 쪽으로 되돌아가지 않음
            if (parent[next] != -1) continue;
            parent[next] = cur;
            // 부모와 자식은 한 간선 차이; 깊이는 루트에서의 간선 수
            depth[next] = depth[cur] + 1;
            queue.offer(next);
        }
    }
}
```

**예시**

```text
0        parent [0,0,0,2]
   / \       depth  [0,1,1,2]
  1   2
       \3     root의 부모는 자기 자신으로 둠
```

**주의** 연결된 Tree 입력을 전제로 한다. 루트 부모의 자기 자신은 편의를 위한 표시. O(V) 시간·공간.

[[BFS]] · [[LCA]] · [[DFS and Graphs]]
