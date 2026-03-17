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

## Pattern A: FastScanner (입력 병목 제거)

```java
static class FastScanner {
    private final InputStream in;
    private final byte[] buffer = new byte[1 << 16];
    private int ptr = 0, len = 0;

    FastScanner(InputStream is) { this.in = is; }

    private int read() throws IOException {
        if (ptr >= len) {
            len = in.read(buffer);
            ptr = 0;
            if (len <= 0) return -1;
        }
        return buffer[ptr++];
    }

    int nextInt() throws IOException {
        int c;
        do c = read(); while (c <= ' ');
        int sign = 1;
        if (c == '-') { sign = -1; c = read(); }
        int val = 0;
        while (c > ' ') {
            val = val * 10 + (c - '0');
            c = read();
        }
        return val * sign;
    }
}
```

## Pattern B: 테스트케이스 재사용 버퍼

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

## Pattern C: 객체 생성 최소화

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
