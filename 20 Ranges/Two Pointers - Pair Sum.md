---
type: template
tags: ["Ranges"]
aliases: ["Two_Pointers", "Two Pointers"]
---

# Two Pointers - Pair Sum

```java
// arr는 오름차순. 서로 다른 두 원소의 합이 target인지 찾는다.
int left = 0, right = arr.length - 1;
while (left < right) { // 같은 원소를 두 번 쓰지 않도록 두 위치가 달라야 한다.
    // int끼리 먼저 더하면 넘칠 수 있어 덧셈 전 long으로 바꾼다.
    long sum = (long) arr[left] + arr[right];
    if (sum == target) return true;
    if (sum < target) left++; // 현재 left는 가장 큰 right와도 부족하다.
    else right--; // 현재 right는 가장 작은 left와도 너무 크다.
}
return false;
```

**예시**

```text
arr   -3  -1   2   4   8      target=6
       L               R      합5 → L++
           L           R      합7 → R--
           L       R          합3 → L++ → 2+4=6 ✓
```

**주의** 오름차순 필수. 덧셈 전 long 변환. 탐색 O(n), 정렬 필요 시 O(n log n) 추가.

[[Two Pointers - Subarray Sum]] · [[Sliding Window - Fixed Size]] · [[Subarray Sum - Existence]] · [[Sorting]]
