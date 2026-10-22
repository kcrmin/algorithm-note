---
type: template
tags: ["Graphs"]
category: connectivity-ordering
categories: [connectivity-ordering, flow-matching]
---

# Bipartite Graph

```text
같은 색끼리 간선이 없어야 함

+ — −         + — −
│   │          \ /
− — +           + ✗

짝수 고리: 가능    홀수 고리: 충돌
```

| Step | Pseudocode |
|---|---|
| Restart | // 미색칠 정점에서 새 연결 요소의 색칠을 시작한다. |
| Paint | // 이웃은 현재 정점과 반대 색이어야 한다. |
| Conflict | // 이미 같은 색인 이웃이 있으면 즉시 실패한다. |
| Answer | // 모든 연결 요소를 충돌 없이 색칠하면 성공이다. |

## Java

```java
static List<Integer>[] graph;
static int[] color;

static boolean isBipartite(int n) {
    for (int start = 0; start < n; start++) {
        if (color[start] != 0) {
            continue;
        }

        Queue<Integer> queue = new ArrayDeque<>();
        color[start] = 1;
        queue.offer(start);

        while (!queue.isEmpty()) {
            int cur = queue.poll();

            for (int next : graph[cur]) {
                if (color[next] == color[cur]) {
                    return false;
                }

                if (color[next] != 0) {
                    continue;
                }

                color[next] = -color[cur];
                queue.offer(next);
            }
        }
    }

    return true;
}
```

## Watch points

| 선택 | 이유 / 실수 |
|---|---|
| 0 / +1 / -1 | 0은 미방문, ±1은 두 그룹. 반대 색은 부호만 뒤집으면 된다. |
| 모든 연결 요소 | 한 부분이 성공해도 다른 부분에 홀수 사이클이 있을 수 있다. |
| 홀수 사이클 / 자기 루프 | 홀수 번 색을 뒤집고 원점에 돌아오면 색이 충돌한다. 자기 루프도 불가능하다. |
| 이분성 ≠ 최대 매칭 | 이 노트는 두 그룹으로 나눌 수 있는지 판정한다. 몇 쌍을 짝지을지는 별도 문제다. |

무방향 `graph`, `color=new int[n]`. 반환: 이분 그래프 여부. **O(V+E)**.

**분류** [[00 Maps/Connectivity and Ordering|Connectivity and Ordering]]

**사용 원리** [[30 Search/BFS|BFS]]
