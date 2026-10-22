---
type: template
tags: ["Ranges"]
aliases: ["Sliding_Window", "Sliding Window"]
---

# Sliding Window - Fixed Size

```java
// 길이 k인 모든 연속 구간의 최대 합. 1<=k<=arr.length, 음수도 허용.
int left = 0;
// 전부 음수일 수 있으므로, 아직 고르지 않은 답을 0으로 두지 않는다.
long sum = 0, max = Long.MIN_VALUE;
for (int right = 0; right < arr.length; right++) {
    sum += arr[right];
    // 하나씩 추가하므로 초과도 한 칸뿐이다. if 한 번이면 길이 k가 된다.
    if (right - left + 1 > k) sum -= arr[left++];
    // 아직 k개가 안 찬 구간은 후보로 세지 않는다.
    if (right - left + 1 == k) max = Math.max(max, sum);
}
return max;
```

**예시**

```text
arr  10  20  30  40      k=3
     [10  20  30]        합60
         [20  30  40]    합90 = 60 + 40 − 10
오른쪽 넣기 → k 넘으면 왼쪽 빼기 → 길이 k면 기록
```

**주의** 1≤k≤n. 시간 O(n), 공간 O(1).

[[Sliding Window - Variable Size]] · [[Two Pointers - Pair Sum]] · [[Prefix Sum]] · [[Monotonic Deque]]
