---
type: algorithm
tags:
  - search
  - binary-search
language: java
status: in-review
source: []
---

# Binary Search (이분 탐색)

> **한 줄 요약:** 정렬된 구간에서 조건을 만족하는 경계(최소/최대)를 $O(\log N)$에 찾는 기법

## Core Concept

- **When to use:** 정렬된 배열에서 위치 찾기, 최적화 문제를 결정 문제로 바꿀 때
- **Key Point:**
  - 구간 `[lo, hi]`를 유지하며 `mid` 검사 후 범위를 절반씩 제거

## API & State Change

| Operation   | Code                                      | 설명                             |
|:----------- |:----------------------------------------- |:-------------------------------- |
| Mid         | `int mid = lo + (hi - lo) / 2;`           | overflow 방지                    |
| Lower bound | `hi = mid`, `lo = mid + 1`                | 최소 인덱스                      |
| Upper bound | `lo = mid`, `hi = mid - 1`                | 최대 인덱스                      |
| Loop        | `while (lo < hi)` 또는 `while (lo <= hi)` | 목적에 따라 선택                 |
| Check       | `isValid(mid)`                            | 해당 값이 조건을 만족하는지 판별 | 

## Patterns

### Pattern A: 최소값 찾기 (Lower Bound)

- 조건을 만족하는 **가장 작은** 인덱스 또는 값을 찾음

```java
int lo = 0, hi = n; 
while (lo < hi) {
    int mid = lo + (hi - lo) / 2;
    if (isValid(mid)) hi = mid;     // 만족하면 왼쪽(작은 쪽) 탐색
    else              lo = mid + 1; // 만족 안 하면 오른쪽 탐색
}
return lo;
```

### Pattern B: 최대값 찾기 (Upper Bound)

- 조건을 만족하는 **가장 큰** 인덱스 또는 값을 찾음

```java
int lo = 0, hi = n;
while (lo < hi) {
    int mid = lo + (hi - lo + 1) / 2; // +1로 올림 처리 (무한 루프 방지)
    if (isValid(mid)) lo = mid;       // 만족하면 오른쪽(큰 쪽) 탐색
    else              hi = mid - 1;   // 만족 안 하면 왼쪽 탐색
}
return lo;
```

## Pitfalls & Tips

- **Overflow:** `(lo + hi) / 2` 대신 `lo + (hi - lo) / 2`를 습관화
- **무한 루프:** `lo = mid`를 사용할 때는 반드시 `mid` 계산 시 `+ 1`을 해서 상향 조정
- **단조성(Monotonicity):** `isValid(x)`가 참이다가 어느 지점부터 거짓(혹은 반대)으로 변하는 구간이 명확해야 이분 탐색이 작동
