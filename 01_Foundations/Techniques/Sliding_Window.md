---
type: algorithm
tags:
  - sliding-window
  - two-pointers
language: java
status: in-review
source: []
---

# Sliding Window
> **한 줄 요약:** window를 유지하며 한 칸씩 이동하고, 들어오는 값과 나가는 값만 갱신

## Core Concept
* **When to use:**
  - 연속 구간을 대상으로 조건/합/최댓값을 구할 때
* **Key Point:**
  - window는 항상 [l, r]
  - 고정 크기는 초기 window를 만든 뒤 r = K부터 이동
  - 가변 크기는 조건에 따라 l, r 조절

## Pattern A: 고정 크기 K
```java
// a: length n, window size K
long sum = 0;

// 1) 초기 window [0, K-1] 구성
for (int i = 0; i < K; i++) {
    sum += a[i];
}
// sum 사용: window [0, K-1]

// 2) 오른쪽으로 한 칸씩 이동
for (int r = K, l = 0; r < n; r++) {
    sum += a[r];   // 오른쪽에서 새 값 추가
    sum -= a[l++]; // 왼쪽 값 제거
    // sum 사용: window [l, r]
}
```

## Pattern B: 가변 크기 (Two Pointers, 양수 배열 전형)
```java
long sum = 0;

for (int r = 0, l = 0; r < n; r++) {
    sum += a[r]; // 오른쪽으로 확장

    while (sum >= S) {
        // 현재 window [l, r]가 조건을 만족
        // (필요한 정답 갱신은 여기서 처리)
        sum -= a[l++]; // 왼쪽에서 축소
    }
}
```

## Pitfalls & Tips
- 연속 구간 전용
- 음수 포함 시 가변 크기 패턴이 깨질 수 있음
- 고정 크기는 prefix sum으로도 가능(구현 취향)
