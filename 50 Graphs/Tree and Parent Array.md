---
type: template
tags: ["Graphs"]
aliases: ["Rooted Tree"]
---

# Tree and Parent Array

```java
static void findParents(int root) {
    Queue<Integer> queue = new ArrayDeque<>();
    parent[root] = root; // ①
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

| 암기 포인트 | 이유 |
|---|---|
| parent=-1이면 미방문 | 양방향 간선에서 부모 쪽으로 되돌아가는 것을 막는다. |
| ① 루트도 먼저 방문 표시 | -1로 남겨 두면 자식이 루트를 새 정점으로 다시 넣는다. |

**예시**

```text
0        parent [0,0,0,2]
   / \       depth  [0,1,1,2]
  1   2
       \3     root의 부모는 자기 자신으로 둠
```

**적용** 무방향 Tree에 루트를 정해 부모와 깊이를 기록한다.

**준비** `List<Integer>[] graph`는 양방향 Tree. `parent`는 -1, `depth`는 0으로 초기화.

**주의** 연결된 Tree 입력을 전제로 한다. 루트 부모의 자기 자신은 편의를 위한 표시. O(V) 시간·공간.

[[BFS]] · [[LCA]] · [[DFS and Graphs]]
