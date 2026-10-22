---
type: template
tags: ["Ranges"]
---

# Two Pointers - Subarray Sum

```java
// 합이 정확히 target인 비어 있지 않은 연속 구간의 존재를 찾는다.
// 원소와 target은 모두 양수. 그래야 넣으면 합 증가, 빼면 합 감소.
int left = 0;
long sum = 0;
for (int right = 0; right < arr.length; right++) {
    sum += arr[right];
    // 같은 합은 답이므로 초과할 때만 줄인다. 여러 개를 뺄 수 있어 while.
    while (left <= right && sum > target) sum -= arr[left++];
    // 모두 빼서 left>right가 된 빈 구간은 정답에서 제외한다.
    if (left <= right && sum == target) return true;
}
return false;
```

**예시**

```text
arr=[2,1,3,2], target=5
[2,1,3] 합6 → 초과: 2 제거 → [1,3] 합4
  [1,3,2] 합6 → 초과: 1 제거 → [3,2] 합5 ✓
넣기 → 초과하는 동안 빼기 → 같은지 확인
```

**주의** 원소>0, target>0. 음수는 Subarray Sum 카드로. 시간 O(n), 공간 O(1).

[[Two Pointers - Pair Sum]] · [[Sliding Window - Variable Size]] · [[Subarray Sum - Existence]]
