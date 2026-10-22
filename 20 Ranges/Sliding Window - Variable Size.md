---
type: template
tags: ["Ranges"]
---

# Sliding Window - Variable Size

```java
// 원소>0, target>0. 합이 target 이상인 가장 짧은 구간, 없으면 0.
int left = 0, minLength = Integer.MAX_VALUE;
long sum = 0;
for (int right = 0; right < arr.length; right++) {
    sum += arr[right];
    // 하나를 빼도 만족할 수 있어, 만족하는 동안 더 짧은 창을 계속 확인한다.
    while (sum >= target) {
        // 먼저 빼면 지금의 유효한 후보를 잃을 수 있으므로 기록이 먼저다.
        minLength = Math.min(minLength, right - left + 1);
        sum -= arr[left++];
    }
}
return minLength == Integer.MAX_VALUE ? 0 : minLength;
```

**예시**

```text
arr=[2,1,3,2], target=5
[2,1,3] 합6 → 길이3 기록 → 2 빼면 합4
  [1,3,2] 합6 → 길이3 → 1 빼면 합5
    [3,2] 합5 → 길이2 기록 ✓ → 다시 줄이기
```

**주의** 원소>0, target>0. 없으면 0. 시간 O(n), 공간 O(1).

[[Sliding Window - Fixed Size]] · [[Two Pointers - Subarray Sum]] · [[Subarray Sum - Existence]]
