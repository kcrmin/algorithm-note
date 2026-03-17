---
type: technique
tags:
language: java
status: in-review
source:
  - "[[B_Type_Master_Index]]"
---

# B형 Implementation Kit

> **한 줄 요약:** Java 기준으로 느린 기본기를 제거한 실전 템플릿 모음.

## Pattern A: 테스트케이스 재사용 버퍼

```java
int[] dist = new int[MAX_N + 1];
int[] seen = new int[MAX_N + 1];
int turn = 1;

// Arrays.fill(dist, INF) 대신 turn stamp 사용
if (seen[x] != turn) {
    seen[x] = turn;
    dist[x] = INF;
}
```

## Pattern B: 객체 생성 최소화

```java
// Node 객체를 매번 new 하지 않고 primitive 배열/인덱스 기반으로 관리
int[] q = new int[MAX_Q];
int head = 0, tail = 0;
q[tail++] = start;

while (head < tail) {
    int u = q[head++];
    // ...
}
```

## 연결 노트

- PQ 최적화: Priority_Queue, Dijkstra
- 방문 최적화: Visited, DFS
- 압축/배열화: Coordinate_Compression, Bucket, Hash
