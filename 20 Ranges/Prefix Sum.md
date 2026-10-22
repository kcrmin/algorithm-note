---
type: template
tags: ["Ranges"]
aliases: ["Prefix_Sum"]
---

# Prefix Sum

```java
int n = arr.length;
long[] prefix = new long[n + 1];
for (int i = 0; i < n; i++) {
    prefix[i + 1] = prefix[i] + arr[i];
}

return prefix[right + 1] - prefix[left];
```

| 암기 포인트 | 이유 |
|---|---|
| `prefix[i]` = 앞의 i개 합 | 0칸 합을 0으로 두면 첫 원소부터인 구간도 같은 식이다. |
| `prefix[right+1] − prefix[left]` | 끝까지의 합에서 시작 직전까지의 합을 빼면 원하는 구간만 남는다. |

**예시**

```text
index    0   1   2   3
arr     -2  [5   1  -3]  ← 선택 [1..3]
prefix[1]=−2     prefix[4]=1
답 = prefix[right+1] − prefix[left]
   = prefix[4] − prefix[1] = 1−(−2) = 3
```

**적용** 값이 바뀌지 않는 배열에서 연속 구간의 합을 여러 번 묻는다.

**준비** `arr`, `left`, `right`를 받는 long 메서드 내부.

**주의** 양 끝 포함, 음수도 가능. 전처리·공간 O(n), 조회 O(1). prefix는 한 번만 만든다.

[[2D Prefix Sum]] · [[Subarray Sum - Existence]] · [[Sliding Window - Fixed Size]] · [[Fenwick Tree]]
