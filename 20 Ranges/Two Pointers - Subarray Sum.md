---
type: template
tags: ["Ranges"]
---

# Two Pointers - Subarray Sum

```java
int left = 0;
long sum = 0;
for (int right = 0; right < arr.length; right++) {
    sum += arr[right];
    while (left <= right && sum > target) sum -= arr[left++];
    if (left <= right && sum == target) return true;
}
return false;
```

| 암기 포인트 | 이유 |
|---|---|
| `sum > target`일 때만 제거 | 같은 합은 정답이므로 먼저 버리면 안 된다. |
| `while`, 그리고 `left <= right` | 여러 개를 뺄 수 있다. 빈 구간을 답으로 잡지 않는다. |
| 양수 조건 | 원소를 추가하면 합 증가, 제거하면 합 감소가 보장된다. |

**예시**

```text
arr=[2,1,3,2], target=5
[2,1,3] 합6 → 초과: 2 제거 → [1,3] 합4
  [1,3,2] 합6 → 초과: 1 제거 → [3,2] 합5 ✓
넣기 → 초과하는 동안 빼기 → 같은지 확인
```

**적용** 양수 배열에서 합이 정확히 target인 연속 구간이 있는지 찾는다.

**준비** `arr`, `target`을 받는 boolean 메서드 내부.

**주의** 원소>0, target>0. 음수는 Subarray Sum 카드로. 시간 O(n), 공간 O(1).

[[Two Pointers - Pair Sum]] · [[Sliding Window - Variable Size]] · [[Subarray Sum - Existence]]
