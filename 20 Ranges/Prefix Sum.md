---
type: template
tags: ["Ranges"]
aliases: ["Prefix_Sum"]
---

# Prefix Sum

```java
// 값이 바뀌지 않는 배열. 양 끝을 포함한 [left,right]의 합을 반환.
int n = arr.length;
// prefix[i]=앞의 i개 합. 0개 합도 넣어 시작이 0일 때 분기를 없앤다.
long[] prefix = new long[n + 1];
for (int i = 0; i < n; i++) {
    prefix[i + 1] = prefix[i] + arr[i];
}

// 오른쪽 끝까지의 합에서 왼쪽 직전까지의 합을 빼면 원하는 구간만 남는다.
return prefix[right + 1] - prefix[left];
```

**예시**

```text
index    0   1   2   3
arr     -2  [5   1  -3]  ← 선택 [1..3]
prefix[1]=−2     prefix[4]=1
답 = prefix[right+1] − prefix[left]
   = prefix[4] − prefix[1] = 1−(−2) = 3
```

**주의** 양 끝 포함, 음수도 가능. 전처리·공간 O(n), 조회 O(1). prefix는 한 번만 만든다.

[[2D Prefix Sum]] · [[Subarray Sum - Existence]] · [[Sliding Window - Fixed Size]] · [[Fenwick Tree]]
