---
type: template
tags: [그래프]
aliases: [Strongly Connected Components, 강한 연결 요소]
---

# SCC

> 같은 SCC의 정점은 서로 도달할 수 있고, SCC를 하나의 정점으로 압축하면 사이클 없는 유향 그래프가 된다.

## 개념

SCC(강한 연결 요소)는 유향 그래프에서 서로 왕복할 수 있는 정점들을 가능한 한 크게 묶은 집합이다. 각 SCC를 하나의 정점으로 압축하면 DAG가 되므로, 순환하는 관계를 하나의 덩어리로 처리한 뒤 덩어리 사이의 선후 관계를 다룰 수 있다.

## 적용 조건

- 정점은 `0 ~ n-1`, `edges[i] = {출발, 도착}`인 유향 그래프다. 같은 두 정점 사이의 중복 간선과 자기 루프가 있어도 된다.
- `component[v]`는 정점 v의 SCC 번호, `groups[id]`는 그 SCC의 정점 목록, `count`는 SCC 개수다. 번호는 `0 ~ count-1`이며 정점 목록은 정렬되지 않는다.
- Kosaraju 알고리즘이다. 원래 그래프에서 DFS의 **종료 순서**를 구하고, 그 역순으로 역방향 그래프를 탐색한다.
- 시간 `O(V+E)`, 그래프·종료 순서·명시적 스택을 포함한 공간 `O(V+E)`다. 두 탐색 모두 반복문이므로 재귀 호출 깊이에 의존하지 않는다.
- `n = 0`이면 SCC 개수는 0이고 결과 배열과 목록도 비어 있다.

## Java 템플릿

```java
import java.util.*;

class SCC {
    static class Result {
        final int count;
        final int[] component;
        final List<List<Integer>> groups;

        Result(int[] component, List<List<Integer>> groups) {
            this.count = groups.size();
            this.component = component;
            this.groups = groups;
        }
    }

    static Result decompose(int n, int[][] edges) {
        List<List<Integer>> graph = new ArrayList<>();
        List<List<Integer>> reverse = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            graph.add(new ArrayList<>());
            reverse.add(new ArrayList<>());
        }
        for (int[] edge : edges) {
            graph.get(edge[0]).add(edge[1]);
            reverse.get(edge[1]).add(edge[0]);
        }

        boolean[] visited = new boolean[n];
        int[] nextEdge = new int[n];
        List<Integer> finishOrder = new ArrayList<>();
        Deque<Integer> stack = new ArrayDeque<>();

        for (int start = 0; start < n; start++) {
            if (visited[start]) continue;
            visited[start] = true;
            stack.push(start);
            while (!stack.isEmpty()) {
                int current = stack.peek();
                if (nextEdge[current] == graph.get(current).size()) {
                    stack.pop();
                    finishOrder.add(current); // 모든 자식 처리가 끝난 시점
                } else {
                    int next = graph.get(current).get(nextEdge[current]++);
                    if (!visited[next]) {
                        visited[next] = true;
                        stack.push(next);
                    }
                }
            }
        }

        int[] component = new int[n];
        Arrays.fill(component, -1);
        List<List<Integer>> groups = new ArrayList<>();

        for (int i = finishOrder.size() - 1; i >= 0; i--) {
            int start = finishOrder.get(i);
            if (component[start] != -1) continue;
            int id = groups.size();
            List<Integer> group = new ArrayList<>();
            groups.add(group);
            component[start] = id;
            stack.push(start);
            while (!stack.isEmpty()) {
                int current = stack.pop();
                group.add(current);
                for (int next : reverse.get(current)) {
                    if (component[next] != -1) continue;
                    component[next] = id;
                    stack.push(next);
                }
            }
        }
        return new Result(component, groups);
    }
}
```

> [!info]- 원리와 코드 설명
>
> ### 1. 연결되어 있다는 것과 서로 왕복할 수 있다는 것은 다르다
>
> 간선이 `0→1` 하나뿐이면 0에서 1로 갈 수 있지만 1에서 0으로 돌아갈 수 없다. 두 정점은 서로 다른 SCC다. `1→0`도 있으면 두 정점은 하나의 SCC가 된다.
>
> 아래 유향 그래프를 사용한다.
>
> ```text
> 0 → 1 → 2 → 3 → 4 → 5
> ↑       │   ↑   │
> └───────┘   └───┘
> ```
>
> 정확한 간선은 `0→1, 1→2, 2→0, 2→3, 3→4, 4→3, 4→5`다. SCC는 `A={0,1,2}`, `B={3,4}`, `C={5}`이고, 압축한 그래프는 `A→B→C`다.
>
> ### 2. 방문 순서 대신 종료 순서를 기록하는 이유
>
> 이 예에서 DFS는 `0,1,2,3,4,5` 순서로 들어간다. 하지만 나올 때는 `5,4,3,2,1,0` 순서다. 5는 갈 곳이 없어 바로 끝나고, 4는 5의 처리가 끝난 뒤에야 끝난다.
>
> `nextEdge[current]`는 그 정점에서 **다음에 검사할 간선 번호**다.
>
> | 상태 | 코드의 동작 |
> |---|---|
> | 아직 검사할 간선이 남음 | 간선 번호를 증가시키고, 미방문 이웃이면 스택에 넣는다 |
> | 모든 간선 검사가 끝남 | 정점을 스택에서 빼고 `finishOrder`에 기록한다 |
>
> 부모는 스택에 남아 있고 자식만 위에 쌓이므로 재귀의 호출·복귀 순서가 그대로 보존된다. 정점을 처음 스택에서 꺼낼 때 바로 종료 목록에 넣는 일반 순회는 이 종료 순서를 보장하지 않는다.
>
> SCC 사이에 `A→B`가 있으면 A 안에서 가장 늦게 끝나는 정점은 B 안의 정점들보다 늦게 끝난다. 따라서 종료 역순은 압축 그래프의 앞쪽 SCC부터 선택할 수 있게 한다.
>
> ### 3. 두 번째 탐색에서 간선을 뒤집는 이유
>
> 종료 역순은 `0,1,2,3,4,5`다. 원래 그래프에서 0을 탐색하면 A를 넘어 B와 C까지 들어가므로 SCC를 분리할 수 없다.
>
> 역방향 그래프에서는 덩어리 사이의 간선이 `C→B→A`가 된다. 0부터 시작하면 A 안에서는 왕복할 수 있지만 A에서 B로 나가는 역방향 간선은 없으므로 `{0,2,1}`만 방문한다.
>
> | 시작 정점 | 역방향 탐색에서 새로 묶는 정점 | 번호 |
> |---|---|---:|
> | 0 | 0, 2, 1 | 0 |
> | 1, 2 | 이미 번호가 있어 건너뜀 | — |
> | 3 | 3, 4 | 1 |
> | 4 | 이미 번호가 있어 건너뜀 | — |
> | 5 | 5 | 2 |
>
> 따라서 `component = [0,0,0,1,1,2]`, `count = 3`이다. 간선 입력 순서가 바뀌면 그룹 안의 정점 나열 순서는 달라질 수 있다.
>
> ### 4. SCC 압축이 DAG가 되는 이유
>
> 서로 다른 SCC A와 B 사이에서 A에서 B로 가는 경로와 B에서 A로 돌아오는 경로가 모두 있으면, 두 덩어리의 모든 정점이 서로 왕복할 수 있다. 그러면 처음부터 하나의 SCC여야 하므로 서로 다른 SCC 사이에는 사이클이 생길 수 없다.
>
> 원래 간선 `u→v`에 대해 `component[u] != component[v]`인 경우만 압축 그래프에 넣는다. 같은 SCC 사이에 여러 원래 간선이 있으면 중복 간선이 생긴다. 단순 도달성에는 문제가 없지만, 진입차수나 경로 개수를 계산할 때는 중복을 어떻게 셀지 정해야 한다.
>
> ### 경계 조건과 변형
>
> - 한 정점만 있는 SCC도 가능하다. 자기 루프가 없어도 길이 0의 경로로 자기 자신에 도달한다.
> - 그래프가 끊겨 있을 수 있으므로 첫 탐색은 모든 정점을 대상으로 시작한다.
> - 첫 탐색의 방문 표시는 두 번째 탐색의 방문 상태로 재사용하지 않는다. 코드에서는 `component = -1`을 두 번째 탐색의 미방문 표시로 사용한다.
> - [[유니온 파인드와 MST|유니온 파인드]]는 방향을 고려한 왕복 도달성을 계산하지 않으므로 SCC를 대신하지 않는다.
> - Tarjan은 DFS 발견 번호와 `low-link`, 현재 SCC 후보 스택으로 같은 분할을 구하는 다른 구현이다. 위 코드는 두 번의 반복 탐색을 사용하는 Kosaraju다.
>
> ## 연결되는 개념
>
> - [[DFS와 그래프]] — DFS가 들어가는 시점과 종료되는 시점의 차이
> - [[위상 정렬]] — SCC를 압축한 DAG의 선후 관계
> - [[유니온 파인드와 MST]] — 무방향 집합 연결과 유향 왕복 관계의 차이
