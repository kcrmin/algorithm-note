---
type: concept
tags:
  - graph
  - graph-basics
language: java
status: in-review
source: []
---

# Graph (그래프 기초)

> **한 줄 요약:** 정점(Vertex)과 간선(Edge)로 관계를 표현하는 자료구조

## Core Concept

- **When to use:** 그래프, 탐색, 최단경로, 사이클
- **Key Point:**
  - 방향 / 가중치 여부에 따라 유형 구분
  - 보통 인접 리스트 사용 (Sparse 그래프)

## 용어 (그림 주석)

### Vertex (정점)

```
A ●
```

- 동그라미 하나가 정점

### Edge (간선)

```
A ●────● B
      ↑
     Edge
```

- 정점과 정점을 잇는 선이 간선

### Degree (차수)

```
      B
      |
C ─── A ─── D
      |
      E
```

- A의 Degree = 4 (A에 붙은 간선 4개)

### Weight (가중치)

```
A ●──(5)──● B
```

- 간선 위 숫자가 비용/거리/시간 등

### In-degree / Out-degree (유향 그래프)

```
D → A → B → C
```

- A의 In-degree = 1 (D)
- A의 Out-degree = 1 (B)

### Path (경로)

```
A → B → C → D
```

- 간선을 따라 이동한 정점의 나열

### Cycle (사이클)

```
A → B → C → A
```

- 시작점과 끝점이 같은 경로

### Connected Component (연결 요소)

```
(A ─ B ─ C)      (D ─ E)
```

- 서로 연결된 덩어리 하나가 연결 요소
- 위 예시는 연결 요소 2개

## API & State Change

| Operation        | Code                                      | 설명        |
| :--------------- | :---------------------------------------- | :---------- |
| Init             | `List<Integer>[] graph = new ArrayList[n+1];` | 그래프 생성 |
| Add (undirected) | `graph[u].add(v); graph[v].add(u);`               | 무방향 간선 |
| Add (directed)   | `graph[u].add(v);`                            | 유향 간선   |
| Visit            | `visited[v]=true;`                        | 방문 체크   |

## Patterns

### Pattern A: 입력 저장 (가장 기본형)

```java
int n = sc.nextInt();
int m = sc.nextInt();

List<Integer>[] graph = new ArrayList[n+1];
for(int i=1;i<=n;i++) graph[i] = new ArrayList<>();

for(int i=0;i<m;i++){
    int u = sc.nextInt();
    int v = sc.nextInt();
    graph[u].add(v);
    graph[v].add(u);  // 무방향 그래프일 때만
}
```

### Pattern B: BFS 기본형

```java
boolean[] visited = new boolean[n+1];
Queue<Integer> q = new ArrayDeque<>();

visited[start] = true;
q.add(start);

while(!q.isEmpty()){
    int u = q.poll();
    for(int v : graph[u]){
        if(visited[v]) continue;
        visited[v] = true;
        q.add(v);
    }
}
```

## Pitfalls

- 정점 번호 0/1-index 확인
- 여러 컴포넌트면 전체 루프 필요
- 무향 그래프에서 간선 수는 입력의 2배로 저장됨