---
type: template
cssclasses: [algorithm-reference]
tags: [그래프]
---

# 0-1 BFS

> **핵심:** 가중치 0으로 개선한 정점은 Deque 앞에, 가중치 1로 개선한 정점은 뒤에 넣어 작은 거리부터 처리한다. 방문 표시가 아니라 거리 완화가 재삽입의 기준이다.

## 개념

0-1 BFS는 모든 간선의 가중치가 0 또는 1인 그래프의 최단 경로 알고리즘이다. 현재 거리와 같거나 1 큰 후보만 생긴다는 성질을 이용해, 우선순위 큐 대신 Deque로 탐색 순서를 유지한다. 비용이 같은 곳은 즉시 처리하고 비용이 하나 늘어나는 곳은 뒤로 보내므로 선형 시간에 최단 거리를 구한다.

## 적용 조건

- 이동 비용이 정확히 0 또는 1일 때만 사용한다. 모두 1이면 [[BFS]], 일반적인 0 이상 가중치면 [[다익스트라]]를 사용한다.
- 정점 번호는 `0..V-1`이고 인접 리스트를 사용한다. 아래 `addEdge`는 유향 간선 하나를 넣으며 무향 간선은 반대 방향도 추가한다.
- 시간 `O(V + E)`, 인접 리스트를 포함한 공간 `O(V + E)`다. 도달 불가 거리는 `INF`다.

## Java 템플릿

```java
import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Deque;
import java.util.List;

public final class ZeroOneBFS {
    public static final int INF = Integer.MAX_VALUE;

    public static final class Edge {
        public final int to;
        public final int weight;

        public Edge(int to, int weight) {
            if (weight != 0 && weight != 1) {
                throw new IllegalArgumentException("weight must be 0 or 1");
            }
            this.to = to;
            this.weight = weight;
        }
    }

    private static final class State {
        final int vertex;
        final int distance;

        State(int vertex, int distance) {
            this.vertex = vertex;
            this.distance = distance;
        }
    }

    @SuppressWarnings("unchecked")
    public static List<Edge>[] newGraph(int vertexCount) {
        List<Edge>[] graph = (List<Edge>[]) new List<?>[vertexCount];
        for (int v = 0; v < vertexCount; v++) {
            graph[v] = new ArrayList<>();
        }
        return graph;
    }

    public static void addEdge(List<Edge>[] graph,
                               int from, int to, int weight) {
        graph[from].add(new Edge(to, weight));
    }

    public static int[] shortestPaths(List<Edge>[] graph, int source) {
        int[] distance = new int[graph.length];
        Arrays.fill(distance, INF);
        Deque<State> deque = new ArrayDeque<>();
        distance[source] = 0;
        deque.addFirst(new State(source, 0));

        while (!deque.isEmpty()) {
            State current = deque.removeFirst();
            if (current.distance != distance[current.vertex]) {
                continue;
            }
            for (Edge edge : graph[current.vertex]) {
                int candidate = current.distance + edge.weight;
                if (candidate >= distance[edge.to]) {
                    continue;
                }
                distance[edge.to] = candidate;
                State next = new State(edge.to, candidate);
                if (edge.weight == 0) {
                    deque.addFirst(next);
                } else {
                    deque.addLast(next);
                }
            }
        }
        return distance;
    }
}
```

> [!info]- 원리와 코드 설명
>
> 출발점이 0이고 다음 간선을 나열한 순서대로 추가했다고 하자.
>
> `0 → 1 (1)`, `0 → 2 (0)`, `2 → 1 (0)`, `2 → 3 (1)`, `1 → 3 (1)`
>
> Deque의 항목을 `(정점, 넣을 당시 거리)`로 쓰면 다음과 같다.
>
> | 단계 | 거리 `[d0, d1, d2, d3]` | Deque 앞 → 뒤 |
> |---|---|---|
> | 시작 | `[0, INF, INF, INF]` | `(0, 0)` |
> | 0 처리 | `[0, 1, 0, INF]` | `(2, 0), (1, 1)` |
> | 2 처리 | `[0, 0, 0, 1]` | `(1, 0), (1, 1), (3, 1)` |
> | `(1, 0)` 처리 | 변화 없음 | `(1, 1), (3, 1)` |
> | `(1, 1)` 꺼냄 | 현재 `d1 = 0`이므로 건너뜀 | `(3, 1)` |
>
> 정점 1은 처음에 비용 1로 발견되지만 나중에 `0 → 2 → 1`의 비용 0 경로로 개선된다. 처음 넣을 때 `visited[1] = true`로 막으면 이 개선을 놓친다. 따라서 `candidate < distance[to]`일 때 다시 넣고, 예전 거리로 남은 항목은 꺼낼 때 건너뛴다.
>
> 현재 최솟값이 `d`이면 새 후보는 `d`와 `d + 1`뿐이다. 0 간선을 앞에, 1 간선을 뒤에 넣으면 작은 거리의 유효한 항목부터 처리된다. 같은 거리로는 다시 넣지 않아 0 비용 사이클도 끝없이 반복되지 않는다.
>
> ### 주의점
>
> - 0이 아닌 모든 간선을 뒤에 넣는 식으로 일반 가중치에 적용할 수 없다. 가중치 2가 하나라도 있으면 이 템플릿의 정렬 성질이 깨진다.
> - `candidate <= distance[to]`로 바꾸면 같은 거리의 0 비용 순환을 계속 삽입할 수 있다. **엄격히 개선될 때만** 삽입한다.
> - 상태는 칸 번호뿐 아니라 방향·열쇠 보유 등 문제에 필요한 정보까지 포함해야 한다. 상태를 정수로 인코딩하면 같은 템플릿을 적용할 수 있다.
> - Deque의 앞뒤 연산은 [[스택과 큐]], 일반 BFS와의 차이는 [[BFS]]를 참고한다.
