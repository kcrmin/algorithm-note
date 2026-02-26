---
deprecated: true
note: "Inbox 정리용 노트 (deprecated)"
---

# 섹션 9 - Greedy

## 1. 다익스트라 (Dijkstra) 알고리즘
```java
import java.util.*;

public class Main {
    // 그래프의 간선 정보 (그래프가 이동 가능한 노드와 가중치 저장)
    static class Edge {
        int to, cost;
        Edge(int to, int cost) { this.to = to; this.cost = cost; }
    }
    // PQ에 들어갈 정보 (갈 수 있는 노드와 현재까지 이동거리를 저장)
    static class State {
        int node, dist;
        State(int node, int dist) { this.node = node; this.dist = dist; }
    }

    // 인접 리스트 그래프 (n loop로 초기화 필요)
    static List<List<Edge>> graph = new ArrayList<>();
    // 최단 거리 테이블 (Arrays.fill(dist, Integer.MAX_VALUE)로 거리 초기화)
    static int[] dist;

    static void dijkstra(int start) {
        // 누적 거리(dist)가 작은 순서대로 꺼내는 최소 힙
        PriorityQueue<State> pq = new PriorityQueue<>(Comparator.comparingInt(s -> s.dist));
        pq.offer(new State(start, 0));
        dist[start] = 0;

        while (!pq.isEmpty()) {
            State cur = pq.poll();
            if (cur.dist > dist[cur.node]) continue; // 오래된(더 긴) 후보는 스킵

            // 인접 노드 탐색
            for (Edge e : graph.get(cur.node)) {
                int nd = cur.dist + e.cost;
                // 더 짧은 거리 발견 시 갱신
                if (nd < dist[e.to]) {
                    dist[e.to] = nd;
                    pq.offer(new State(e.to, nd));
                }
            }
        }
    }
}
```


## 2. *Union-Find 알고리즘
```java
import java.util.*;

public class Main {
    public static int[] unf;

    public static int find(int v) {
        if (unf[v] == v) return v;
        return unf[v] = find(unf[v]);
    }

    public static void union(int a, int b) {
        int fa = find(a);
        int fb = find(b);
        if (fa != fb) unf[fa] = fb;
    }
}
```
> 그냥 통으로 외우기


## 3. 크루스칼 (Kruskal) 알고리즘
```java
import java.util.*;

public class Main {
    // Union-Find 메소드 구현
    
    public static int kruskal(int n, ArrayList<Edge> roads) {
        // MST 총 비용
        int result = 0;
        // 현재 선택한 간선 수
        int edges = 0;
        // MST는 항상 V-1개의 간선 (간선의 최대 수 공식)
        int maxEdges = n - 1;

        roads.sort(Comparator.comparingInt(e -> e.w));

        for (Edge road : roads) {
            if (find(road.v1) != find(road.v2)) {        // 싸이클이 생기지 않으면
                union(road.v1, road.v2);     // 두 노드 연결
                result += road.w;  // 비용 추가
                edges++;           // 엣지수 기록 (증가)
                if (edges == maxEdges) break; // MST 완성
            }
        }

        return result;
    }
}
```
> Union-Find 알고리즘에 대한 지식 필요


## 4. 프림 (Prim) 알고리즘
```java
import java.util.*;

public class Main {
    static class Edge {
        int v, w;
        Edge(int v, int w) { this.v = v; this.w = w; }
    }

    static List<List<Edge>> graph = new ArrayList<>();
    static boolean[] visited;

    public static int prim(int n, int start) {
        visited = new boolean[n + 1];
        int result = 0;
        int edges = 0;
        int maxEdges = n - 1;

        PriorityQueue<Edge> pq = new PriorityQueue<>(Comparator.comparingInt(e -> e.w));
        pq.offer(new Edge(start, 0));

        while (!pq.isEmpty()) {
            Edge cur = pq.poll();
            if (visited[cur.v]) continue;

            visited[cur.v] = true;
            result += cur.w;
            edges++;
            if (edges == maxEdges) break;

            for (Edge next : graph.get(cur.v)) {
                if (!visited[next.v]) {
                    pq.offer(next);
                }
            }
        }
        return result;
    }
}
```
> Prim 알고리즘은 무방향 가중 그래프이므로 각 간선을 양쪽 방향으로 모두 추가해야 한다.
>
> 즉,
> ```java
> graph.get(a).add(new Edge(b, c));
> graph.get(b).add(new Edge(a, c));
> ```
> 처럼 양방향으로 연결해줘야 한다.


## 3. 주의 포인트
|   코드 / 개념    | 요약                                        |
|:------------:|:------------------------------------------|
|    Greedy    | Greedy는 “항상 최선”이 보장되는 구조인지 확인 필요          |
|      정렬      | 정렬 기준이 잘못되면 오답률 100%                      |
|      정렬      | 우선순위 큐 사용 시 오름/내림 정방향 주의                  |
|      사고      | 탑다운 사고 → “지금 선택하면 전체 영향이 최선인가?”           |
| DP vs Greedy | 정렬 또는 우선순위 기준 명확해야 함, 기준이 모호하면 DP/백트래킹 필요 |


## **핵심 요약**
- 정렬 or PQ 기반으로 “가장 좋은 선택” 반복
- 전체 최적해가 현재 최적 선택에 의해 보장되는 구조인지 파악
- 회의실 배정 / 최소 비용 / 동전 문제 / 병합 비용 / 작업 스케줄링 등에서 사용

## 문제 다시 풀기
### **[Inflearn](../../src/inflearn/section09_greedy)**
| 분류  | 문제 이름                                                                                               | 포인트 메모                     |
|:---:|:----------------------------------------------------------------------------------------------------|:---------------------------|
| 감잡기 | [09-01 씨름 선수](../../src/inflearn/section09_greedy/INF_S09_P01.java)                                 | 기초                         |
| 감잡기 | [09-02 회의실 배정](../../src/inflearn/section09_greedy/INF_S09_P02.java)                                | 기초                         |
| 감잡기 | [09-03 결혼식](../../src/inflearn/section09_greedy/INF_S09_P03.java)                                   | 기초                         |
| 응용  | [09-04 최대 수입 스케쥴(PriorityQueue 응용문제)](../../src/inflearn/section09_greedy/INF_S09_P04.java)         | 문제 잘 이해하기                  |
| 기초  | [09-05 다익스트라 알고리즘](../../src/inflearn/section09_greedy/INF_S09_P05.java)                            | 다익스트라 알고리즘                 |
| 기초  | [09-06 친구인가? (Disjoint-Set : Union&Find)](../../src/inflearn/section09_greedy/INF_S09_P06.java)     | Union & Find 알고리즘          |
| 응용  | [09-07 원더랜드(최소 스패닝 트리 - 크루스칼 : Uion&Find 이용)](../../src/inflearn/section09_greedy/INF_S09_P07.java) | Union & Find 응용, 크루스칼 알고리즘 |