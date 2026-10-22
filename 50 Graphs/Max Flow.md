---
type: template
tags: ["Graphs"]
aliases: ["Residual Graph", "Min Cut"]
category: flow-matching
categories: [flow-matching]
---

# Max Flow

```text
용량 5인 간선으로 3을 보냄

보내기 전:  A ──5──→ B     B ──0──→ A
보낸 뒤:    A ──2──→ B     B ──3──→ A
              더 보낼 양       취소할 수 있는 양

잔여 경로 S→…→T를 찾아 보내고, 필요하면 되돌려 재배치
```

| Step | Pseudocode |
|---|---|
| Pair | // 용량이 있는 정방향 간선과 용량 0인 역방향 간선을 짝으로 만든다. |
| Index | // 각 간선은 짝 역간선의 인접 리스트 인덱스를 기억한다. |
| Augment | // 보낸 양만큼 정방향 잔여 용량을 줄이고 역방향을 늘린다. |
| Run | // Edmonds-Karp 또는 Dinic으로 증대 경로를 반복해서 찾는다. |

## Java

```java
static List<Edge>[] graph;

static class Edge {
    int to;
    int reverse;
    long remaining;

    Edge(int t, int r, long c) {
        to = t;
        reverse = r;
        remaining = c;
    }
}

static void addEdge(int from, int to, long capacity) {
    int forwardIndex = graph[from].size();
    int reverseIndex = graph[to].size();

    // 같은 리스트에 정방향 다음으로 역간선이 들어간다.
    if (from == to) {
        reverseIndex++;
    }

    graph[from].add(new Edge(to, reverseIndex, capacity));
    graph[to].add(new Edge(from, forwardIndex, 0));
}
```

## Watch points

| 선택 | 이유 / 실수 |
|---|---|
| 역간선의 의미 | 새로 주어진 반대 방향 용량이 아니라 이전 전송을 취소할 권한이다. |
| 양방향 입력 간선 | 입력에 반대 방향 간선도 있으면 addEdge를 따로 호출한다. 각각 자기 역간선이 생긴다. |
| `reverse`는 인덱스 | 정점 번호가 아니다. graph[to] 안에서 짝 간선이 있는 위치다. |
| 자기 루프 보정 | 같은 리스트에 두 간선을 연달아 넣으므로 정방향이 가리킬 역간선 위치를 하나 뒤로 잡는다. |
| Min Cut | 최대 유량 계산 후 양의 잔여 간선으로 source에서 닿는 쪽과 나머지의 경계가 최소 컷이다. |

이 코드는 공통 간선 구조. 모든 graph[v]를 빈 리스트로 준비, capacity≥0, 총유량 long 범위. 실행 함수에서는 source≠sink. **O(V+E)** 공간.

**분류** [[00 Maps/Flow and Matching|Flow and Matching]]

**실행** [[50 Graphs/Edmonds-Karp|Edmonds-Karp]] · [[50 Graphs/Dinic|Dinic]]
