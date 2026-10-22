---
type: template
tags: ["Ranges"]
---

# Sliding Window - Variable Size

**언제** 양수 배열에서 합이 target 이상인 가장 짧은 연속 구간.

```text
arr=[2,1,3,2], target=5
[2,1,3] 합6 → 길이3 기록 → 2 빼면 합4
  [1,3,2] 합6 → 길이3 → 1 빼면 합5
    [3,2] 합5 → 길이2 기록 ✓ → 다시 줄이기
```

**코드** `arr`, `target`을 받는 int 메서드 내부.

```java
int left = 0, minLength = Integer.MAX_VALUE;
long sum = 0;
for (int right = 0; right < arr.length; right++) {
    sum += arr[right];
    while (sum >= target) {
        minLength = Math.min(minLength, right - left + 1);  // ①
        sum -= arr[left++];  // ②
    }
}
return minLength == Integer.MAX_VALUE ? 0 : minLength;
```

| 암기 포인트 | 이유 |
|---|---|
| ① 만족하면 먼저 기록 | 빼는 순간 유효한 후보가 사라질 수 있다. |
| ② `while`로 계속 줄임 | 하나를 빼도 여전히 만족하면 더 짧은 답이 있다. |
| 양수 조건 | 오른쪽 추가는 합을 키우고, 왼쪽 제거는 합을 줄여야 한다. |

**주의** 원소>0, target>0. 없으면 0. 시간 O(n), 공간 O(1).

[[Sliding Window - Fixed Size]] · [[Two Pointers - Subarray Sum]] · [[Subarray Sum - Existence]]
