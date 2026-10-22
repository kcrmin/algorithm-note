---
type: template
tags: ["Ranges"]
category: two-pointers-windows
categories: [two-pointers-windows]
---

# Two Pointers - Subarray Sum

```text
target = 7
[2  3] 4       sum=5 → 오른쪽 추가
[2  3  4]      sum=9 → 왼쪽 제거
 2 [3  4]      sum=7 → found

sum 작음 → Expand     sum 큼 → Shrink     같음 → Answer
```

## Steps

| Step | Pseudocode |
| --- | --- |
| **Expand** | // 오른쪽 원소 하나를 더한다. |
| **Shrink** | // 목표를 초과하는 동안 왼쪽을 뺀다. |
| **Answer** | // 남은 창이 비어 있지 않고 합이 목표와 같으면 찾았다. |

## Java

```java
int left = 0;
long sum = 0;

for (int right = 0; right < arr.length; right++) {
    sum += arr[right];

    while (left <= right && sum > target) {
        sum -= arr[left++];
    }

    if (left <= right && sum == target) {
        return true;
    }
}

return false;
```

합=target인 연속 구간 존재 여부 · `O(n)`

## Watch points

| 선택 | 이유 / 실수 |
| --- | --- |
| 양수 원소 · 양수 target | 확장하면 합이 늘고 축소하면 줄어야 한다. 음수가 있으면 Prefix Sum + Hashing을 쓴다. |
| 초과할 때만 Shrink | 같은 합은 답이다. `>=`로 줄이면 답을 지워 버린다. |
| `left <= right` | 최소 한 원소가 있는 구간만 확인한다. |
| 정렬 금지 | 연속 구간 문제다. 정렬하면 원래 이웃 관계가 바뀐다. |

---

**분류** [[00 Maps/Two Pointers and Windows|Two Pointers and Windows]]
