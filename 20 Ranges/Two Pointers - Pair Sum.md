---
type: template
tags: ["Ranges"]
aliases: ["Two_Pointers", "Two Pointers"]
---

# Two Pointers - Pair Sum

**언제** 정렬된 배열에서 합이 target인 서로 다른 두 원소. 연속 구간을 더하는 문제가 아니다.

```text
arr   -3  -1   2   4   8      target=6
       L               R      합5 → L++
           L           R      합7 → R--
           L       R          합3 → L++ → 2+4=6 ✓
```

**코드** `arr`, `target`을 받는 boolean 메서드 내부.

```java
int left = 0, right = arr.length - 1;
while (left < right) {
    long sum = (long) arr[left] + arr[right];
    if (sum == target) return true;
    if (sum < target) left++;
    else right--;
}
return false;
```

| 암기 포인트 | 이유 |
|---|---|
| `sum < target` → left++ | 현재 왼쪽 값은 가장 큰 오른쪽 값과 더해도 부족하다. |
| `sum > target` → right-- | 현재 오른쪽 값은 가장 작은 왼쪽 값과 더해도 너무 크다. |
| `left < right` | 같은 원소를 두 번 쓰지 않는다. |

**주의** 오름차순 필수. 덧셈 전 long 변환. 탐색 O(n), 정렬 필요 시 O(n log n) 추가.

[[Two Pointers - Subarray Sum]] · [[Sliding Window - Fixed Size]] · [[Subarray Sum - Existence]] · [[Sorting]]
