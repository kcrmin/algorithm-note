---
type: technique
tags:
  - prefix-sum
language: java
status: in-review
source: []
---

# Prefix Sum
> **한 줄 요약:** query의 크기가 주어질 때, 위치는 for-loop로 순회하고 계산은 prefix sum으로 O(1)

## Core Concept
* **When to use:**
  - 1D에서 길이 K 구간 합을 모든 위치에 대해 구할 때
  - 2D에서 크기 (H x W) 직사각형 합을 모든 위치에 대해 구할 때
* **Key Point:**
  - for-loop는 위치만 이동
  - prefix sum은 계산만 담당
  - query 크기는 항상 고정값(K, H, W)

## Patterns
### Pattern A: 1D 고정 길이 K 구간 합
- 구간 길이 K가 주어짐
- 시작점 l만 for-loop로 이동
```java
// a: length n, window size K
long[] ps = new long[n + 1];

// Build
for (int i = 1; i <= n; i++) {
    ps[i] = ps[i - 1] + a[i - 1];
}

// Query: r은 K부터 시작 (ps에서 r은 "구간 끝 다음 칸")
// 실제 구간은 [r-K, r-1] (길이 K)
for (int r = K; r <= n; r++) {
    long sum = ps[r] - ps[r - K];
    // sum 사용
}
```

### Pattern B: 2D 고정 크기 (H x W) 직사각형 합
- 크기: 세로 H, 가로 W
- 좌상단 (x, y)을 for-loop로 순회
```java
// grid: n x m
long[][] ps = new long[n + 1][m + 1];

// Build
for (int x = 1; x <= n; x++) {
    for (int y = 1; y <= m; y++) {
        ps[x][y] =
            ps[x - 1][y]
          + ps[x][y - 1]
          - ps[x - 1][y - 1]
          + grid[x - 1][y - 1];
    }
}

// Query: (x, y)를 HxW 직사각형의 우하단(ps 좌표)로 본다
for (int x = H; x <= n; x++) {
    for (int y = W; y <= m; y++) {
        long rect = ps[x][y]
                  - ps[x - H][y]
                  - ps[x][y - W]
                  + ps[x - H][y - W];
    }
}
```

## Pitfalls & Tips
- Query에서는 새로 더하지 않는다
- prefix 배열은 항상 한 칸 크게 생성
- 합이 커질 수 있으면 long 사용
