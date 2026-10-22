---
type: template
tags: ["Ranges"]
category: two-pointers-windows
categories: [two-pointers-windows]
---

# Sliding Window - Variable Size

```text
target = 7
[2  3  4] 1      sum=9, length=3 → 기록
 2 [3  4] 1     sum=7, length=2 → 기록
 2  3 [4] 1     sum=4           → 확장 필요

합 충분 → 길이 기록 → 왼쪽 제거 → 아직 충분하면 반복
```

## Steps

| Step | Pseudocode |
| --- | --- |
| **Expand** | // 오른쪽 원소를 더해 합을 키운다. |
| **Answer** | // 합이 충분하면 현재 길이를 먼저 기록한다. |
| **Shrink** | // 왼쪽을 빼며 더 짧게 만든다. 합이 충분한 동안 Answer와 Shrink를 반복한다. |

## Java

```java
int left = 0;
int minLength = Integer.MAX_VALUE;
long sum = 0;

for (int right = 0; right < arr.length; right++) {
    sum += arr[right];

    while (sum >= target) {
        minLength = Math.min(minLength, right - left + 1);
        sum -= arr[left++];
    }
}

return minLength == Integer.MAX_VALUE ? 0 : minLength;
```

합 ≥ target인 최단 연속 구간 · `O(n)`

## Watch points

| 선택 | 이유 / 실수 |
| --- | --- |
| 양수 원소 · 양수 target | 넣으면 합이 커지고 빼면 작아져야 한다. 음수에는 이 이동 근거가 깨진다. |
| Answer → Shrink | 빼고 나면 조건이 깨질 수 있다. 조건이 참인 현재 창부터 기록한다. |
| `while` | 한 번 줄인 뒤에도 합이 충분할 수 있다. 더 짧은 답을 놓치지 않도록 계속 줄인다. |
| 답 없음 → `0` | 최솟값이 한 번도 바뀌지 않았으면 조건을 만족한 구간이 없다. |

## Fixed Size와 순서 비교

| 목적 | 반복 흐름 | 왜 이 순서인가 |
| --- | --- | --- |
| 길이 k의 최대 합 | Expand → Shrink → Answer | 길이부터 k로 맞춘다. |
| 합 ≥ target의 최소 길이 | Expand → **Answer → Shrink** 반복 | 조건이 깨지기 전에 유효한 길이를 기록한다. |

---

**분류** [[00 Maps/Two Pointers and Windows|Two Pointers and Windows]]

**비교** [[20 Ranges/Two Pointers - Subarray Sum|Two Pointers - Subarray Sum]]
