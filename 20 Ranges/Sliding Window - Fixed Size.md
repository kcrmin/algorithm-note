---
type: template
tags: ["Ranges"]
aliases: ["Sliding_Window", "Sliding Window"]
---

# Sliding Window - Fixed Size

**언제** 길이 k인 모든 연속 구간 중 최대 합. 음수도 가능하다.

```text
arr  10  20  30  40      k=3
     [10  20  30]        합60
         [20  30  40]    합90 = 60 + 40 − 10
오른쪽 넣기 → k 넘으면 왼쪽 빼기 → 길이 k면 기록
```

**코드** `arr`, `k`를 받는 long 메서드 내부.

```java
int left = 0;
long sum = 0, max = Long.MIN_VALUE;
for (int right = 0; right < arr.length; right++) {
    sum += arr[right];
    if (right - left + 1 > k) sum -= arr[left++];
    if (right - left + 1 == k) max = Math.max(max, sum);
}
return max;
```

| 암기 포인트 | 이유 |
|---|---|
| `> k`면 한 개 제거 | 한 번에 하나 추가하므로 초과도 한 칸뿐이다. |
| `== k`일 때만 기록 | 아직 덜 찬 창은 후보가 아니다. |
| `Long.MIN_VALUE` | 전부 음수인 배열에서도 0을 잘못 답으로 고르지 않는다. |

**주의** 1≤k≤n. 시간 O(n), 공간 O(1).

[[Sliding Window - Variable Size]] · [[Two Pointers - Pair Sum]] · [[Prefix Sum]] · [[Monotonic Deque]]
