---
type: algorithm
tags:
  - binary-search
language: java
status: in-review
source: []
---

# Binary Search (이분 탐색)

> **한 줄 요약:** 정렬된 구간에서 조건을 만족하는 경계(최소/최대)를 O(log N)에 찾는 기법

## Core Concept

- **When to use:** 정렬된 배열에서 위치 찾기, 파라미터(답)를 이분 탐색으로 정할 때
- **Key Point:**
  - 구간 [lo, hi]를 유지하며 mid에서 조건 확인 후 lo/hi를 좁힘
  - "최소인 x" / "최대인 x"에 따라 lo=mid+1 vs hi=mid 등 결정
  - 무한 루프 방지: mid 계산 시 overflow 주의, 종료 조건 명확히

## API & State Change

| Operation | Code | 설명 |
| :-------- | :--- | :--- |
| Mid | `int mid = lo + (hi - lo) / 2;` | overflow 방지 |
| Lower bound | 조건 만족 시 hi=mid, 아니면 lo=mid+1 | 최소 인덱스 |
| Upper bound | 조건 만족 시 lo=mid+1, 아니면 hi=mid | 최대 인덱스 |
| Loop | `while (lo < hi)` 또는 `while (lo <= hi)` | 목적에 따라 선택 |

## Patterns

### Pattern A: Lower bound (처음으로 조건 만족하는 인덱스)

- `f(mid)`가 true가 되는 **최소** 인덱스. [lo, hi] 닫힌 구간 가정.

```java
int lo = 0, hi = n - 1;
while (lo < hi) {
    int mid = lo + (hi - lo) / 2;
    if (f(mid)) hi = mid;
    else        lo = mid + 1;
}
// lo가 최소 인덱스 (f(lo)==true). 전부 false면 lo==n이거나 범위 밖
```

### Pattern B: 정렬된 배열에서 값의 첫 위치 (lower_bound)

```java
int lo = 0, hi = n;
while (lo < hi) {
    int mid = lo + (hi - lo) / 2;
    if (a[mid] < key) lo = mid + 1;
    else              hi = mid;
}
// lo: a[lo] >= key인 최소 인덱스 (없으면 n)
```

### Pattern C: 파라미터 이분 탐색 (답을 x로 두고 가능 여부로 이분 탐색)

```java
long lo = 0, hi = (long)1e18;
while (lo < hi) {
    long mid = lo + (hi - lo + 1) / 2;  // 상한 찾을 때는 +1
    if (possible(mid)) lo = mid;
    else                hi = mid - 1;
}
// lo가 가능한 최대(또는 최소는 반대로)
```

## Pitfalls & Tips

- `(lo + hi) / 2`는 lo+hi overflow 가능 → `lo + (hi - lo) / 2` 사용
- "최대 x" 찾을 때 mid 계산을 `(lo+hi+1)/2`로 하여 무한 루프 방지
- 인덱스 범위: 0~n-1 vs 0~n (끝 한 칸 past) 혼동하지 않기
- 정렬 여부 확인; 조건 f가 단조여야 이분 탐색 적용 가능
