---
type: template
tags: ["Ranges"]
aliases: ["Sliding_Window", "Sliding Window"]
category: two-pointers-windows
categories: [two-pointers-windows]
---

# Sliding Window - Fixed Size

```text
k = 3
[2  1  5] 1  3     sum = 8
 2 [1  5  1] 3     sum = 8 - 2 + 1 = 7
 2  1 [5  1  3]    sum = 7 - 1 + 3 = 9 ← max

             한 칸 추가 → 넘친 한 칸 제거 → 길이 k의 답
```

## Steps

| Step | Pseudocode |
| --- | --- |
| **Expand** | // 오른쪽 원소 하나를 합에 넣는다. |
| **Shrink** | // 길이가 k를 넘으면 왼쪽 원소 하나를 뺀다. |
| **Answer** | // 길이가 정확히 k일 때만 최댓값을 기록한다. |

## Java

```java
int left = 0;
long sum = 0;
long max = Long.MIN_VALUE;

for (int right = 0; right < arr.length; right++) {
    sum += arr[right];

    if (right - left + 1 > k) {
        sum -= arr[left++];
    }

    if (right - left + 1 == k) {
        max = Math.max(max, sum);
    }
}

return max;
```

길이 k인 연속 구간의 최대 합 · 음수 허용 · `1 ≤ k ≤ arr.length` · `O(n)`

## Watch points

| 선택 | 이유 / 실수 |
| --- | --- |
| 길이 `right - left + 1` | 양 끝을 모두 포함하는 창이다. |
| Shrink는 `if` | 한 번에 한 칸만 추가하므로 길이가 넘쳐도 한 칸만 넘친다. |
| Answer는 Shrink 뒤 | 넘친 원소를 빼고 k개만 남긴 합을 기록한다. |
| 최댓값 초기화 | 음수만 있어도 답이 음수여야 한다. 0으로 시작하면 틀린다. |

---

**분류** [[00 Maps/Two Pointers and Windows|Two Pointers and Windows]]

**비교** [[20 Ranges/Sliding Window - Variable Size|Sliding Window - Variable Size]]
