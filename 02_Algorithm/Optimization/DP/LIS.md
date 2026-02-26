---
type: algorithm
tags:
  - lis
  - dp
language: java
status: in-review
source: []
---

# LIS (Longest Increasing Subsequence, 최장 증가 부분 수열)

> **한 줄 요약:** 수열에서 순서를 유지하며 뽑은 부분 수열 중 strictly 증가인 것의 최대 길이

## Core Concept

- **When to use:** 최대 증가 부분 수열 길이, Dilworth 등
- **Key Point:**
  - **O(N²) DP**: dp[i] = a[i]를 끝으로 하는 LIS 길이 → j < i, a[j] < a[i]인 dp[j] 최대 + 1
  - **O(N log N)**: “꼬리” 배열을 유지하며 이분 탐색으로 위치 갱신 (길이만 구함)
  - 같은 길이의 LIS가 여러 개일 수 있음

## API & State Change

| 방법 | 상태 | 전이/유지 |
| :--- | :--- | :--- |
| O(N²) | dp[i] = a[i] 끝 LIS 길이 | dp[i] = max(dp[j]+1) for j<i, a[j]<a[i] |
| O(N log N) | tail[len] = 길이 len인 증가 수열의 끝 최소값 | 이분 탐색으로 갱신 |

## Patterns

### Pattern A: O(N²) DP

```java
int[] dp = new int[n];
Arrays.fill(dp, 1);
for (int i = 0; i < n; i++) {
    for (int j = 0; j < i; j++) {
        if (a[j] < a[i])
            dp[i] = Math.max(dp[i], dp[j] + 1);
    }
}
int ans = Arrays.stream(dp).max().orElse(0);
```

### Pattern B: O(N log N) — 길이만 (이분 탐색)

```java
List<Integer> tail = new ArrayList<>();
for (int x : a) {
    int pos = Collections.binarySearch(tail, x);
    if (pos < 0) pos = -(pos + 1);
    if (pos == tail.size()) tail.add(x);
    else tail.set(pos, x);
}
int len = tail.size();  // LIS 길이
// 실제 수열 복원하려면 인덱스 기록 필요
```

## Pitfalls & Tips

- strictly increasing: a[j] < a[i]. non-decreasing이면 a[j] <= a[i]
- O(N log N)에서 tail은 “길이 k인 증가 수열의 끝 원소 최소값”만 저장 — 실제 LIS와는 다를 수 있음
- “최대 감소”는 부호 뒤집거나 비교 반대로
