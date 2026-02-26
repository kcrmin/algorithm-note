---
type: concept
tags:
  - graph
  - bipartite-graph
language: java
status: in-review
source: []
---

# Bipartite Graph (이분 그래프)

> **한 줄 요약:** 정점을 두 그룹으로 나눠 같은 그룹끼리는 간선이 없게 만들 수 있는 그래프

## Core Concept

- **When to use:** 2색칠, 이분 판정, 이분 매칭
- **Key Point:**
  - 한 정점에 색(그룹) 2개 중 하나를 칠한다고 생각하면 됨
  - 인접한 두 정점이 같은 색이 되면 실패
  - 홀수 길이 사이클이 있으면 이분 그래프가 아님
  - 그래프가 여러 컴포넌트일 수 있으니 모든 정점에서 시작해야 함

## API & State Change

| Operation | Code | 설명 |
| :-------- | :--- | :--- |
| Color | `color[v] = 1/-1;` | 그룹(색) 지정 |
| Check | `if(color[v] == color[cur])` | 충돌 검사 |
| Visit | `if(color[v] != 0)` | 방문 여부 대체 |

## Patterns

### Pattern A: 입력 저장 (u v) (무방향)

```java
int n = sc.nextInt();
int m = sc.nextInt();

List<Integer>[] graph = new ArrayList[n+1];
for(int i=1;i<=n;i++) graph[i] = new ArrayList<>();

for(int i=0;i<m;i++){
    int u = sc.nextInt();
    int v = sc.nextInt();
    graph[u].add(v);
    graph[v].add(u);
}
```

### Pattern B: BFS로 이분 그래프 판정 (2-coloring)

```java
int[] color = new int[n+1]; // 0=미방문, 1/-1=색
Queue<Integer> q = new ArrayDeque<>();
boolean ok = true;

for(int i=1;i<=n;i++){
    if(color[i] != 0) continue;

    color[i] = 1;
    q.add(i);

    while(!q.isEmpty() && ok){
        int cur = q.poll();

        for(int v : graph[cur]){
            if(color[v] == 0){
                color[v] = -color[cur];
                q.add(v);
            }else if(color[v] == color[cur]){
                ok = false;
                break;
            }
        }
    }
}

System.out.println(ok ? "YES" : "NO");
```

## Pitfalls

- 그래프가 비연결일 수 있으므로 전체 정점 루프가 필요
- 셀프 루프(u==v)가 있으면 즉시 실패
