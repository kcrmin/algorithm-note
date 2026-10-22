---
type: template
tags: ["Graphs"]
aliases: ["Rooted Tree"]
category: traversal-trees
categories: [traversal-trees]
---

# Tree and Parent Array

```text
무방향 간선: 0—1, 0—2, 1—3
루트 0을 정하면:
        0              parent: [0, 0, 0, 1]
       / \             depth : [0, 1, 1, 2]
      1   2
      |
      3

아래로 한 칸: 부모 기록, 깊이 +1
```

| Step | Pseudocode |
|---|---|
| Root | // 루트의 부모는 자기 자신, 깊이는 0으로 둔다. |
| Visit | // 이미 부모가 정해진 이웃은 건너뛴다. |
| Link | // 처음 만난 이웃의 부모를 현재 정점, 깊이를 현재보다 하나 크게 기록한다. |
| Continue | // 새로 연결한 정점을 큐에 넣고 아래로 진행한다. |

## Java

```java
static List<Integer>[] graph;
static int[] parent, depth;

static void findParents(int root) {
    Queue<Integer> queue = new ArrayDeque<>();
    parent[root] = root;
    queue.offer(root);

    while (!queue.isEmpty()) {
        int cur = queue.poll();

        for (int next : graph[cur]) {
            if (parent[next] != -1) {
                continue;
            }

            parent[next] = cur;
            depth[next] = depth[cur] + 1;
            queue.offer(next);
        }
    }
}
```

## Watch points

| 선택 | 이유 / 실수 |
|---|---|
| `parent=-1` | 방문 여부를 별도 배열 없이 부모 배정 여부로 구분한다. |
| 루트의 부모=루트 | 루트도 방문 처리되고, 조상 탐색이 루트 밖으로 나가지 않는다. |
| 트리 조건 | 일반 그래프에서는 BFS 탐색 트리가 만들어진다. 원래 그래프 자체가 트리는 아니다. |
| 초기화 | 모든 parent=-1, depth=0. 루트는 유효한 정점이어야 한다. |

연결된 무방향 트리의 양방향 인접 리스트. `findParents(root)`가 parent·depth를 채운다. **O(V)**.

**분류** [[00 Maps/Traversal and Trees|Traversal and Trees]]

**사용 원리** [[30 Search/BFS|BFS]]
