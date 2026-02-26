---
type: concept
tags:
  - graph
  - weighted-graph
language: java
status: in-review
source: []
---

# Weighted Graph (가중치 그래프)

> **한 줄 요약:** 간선에 비용(weight)이 붙어 있는 그래프

## Core Concept

- **When to use:** 최단경로, 최소비용, MST
- **Key Point:**
  - 간선은 (u, v, w) 형태
  - 인접 리스트에 (정점, 가중치) 함께 저장
  - 음수 간선 존재 여부에 따라 알고리즘 선택이 달라짐
  - 최단경로 알고리즘에서 간선을 통해 거리 추정치를 갱신하는 것을 **relax**라 함

## API & State Change

| Operation | Code | 설명 |
| :-------- | :--- | :--- |
| Init | `List<Edge>[] graph = new ArrayList[n+1];` | 그래프 생성 |
| Add Edge | `graph[u].add(new Edge(v, w));` | 가중치 간선 추가 |
| Relax | `if(dist[v] > dist[u] + w)` | 거리 갱신 (relax) |
| Visit | `visited[v] = true;` | 방문 체크 |

## Patterns

### Pattern A: 입력 저장 (u v w)

```java
static class Edge{
    int to;
    int weight;
    Edge(int to, int weight){
        this.to = to;
        this.weight = weight;
    }
}

int n = sc.nextInt();
int m = sc.nextInt();

List<Edge>[] graph = new ArrayList[n+1];
for(int i=1;i<=n;i++) graph[i] = new ArrayList<>();

for(int i=0;i<m;i++){
    int u = sc.nextInt();
    int v = sc.nextInt();
    int w = sc.nextInt();
    graph[u].add(new Edge(v, w));      // 유향
    // graph[v].add(new Edge(u, w));   // 무방향이면 추가
}
```

## Pitfalls

- 음수 간선 존재 여부에 따라 최단경로 알고리즘 선택이 달라짐
- 거리/비용 합이 int 범위를 넘을 수 있으므로 long 사용 권장