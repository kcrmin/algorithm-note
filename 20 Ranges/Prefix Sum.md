---
type: template
tags: ["Ranges"]
aliases: ["Prefix_Sum"]
category: prefix-sums
categories: [prefix-sums]
---

# Prefix Sum

```java
int n = arr.length;
long[] prefix = new long[n + 1]; // prefix[i]=앞 i개 합. 0개 합도 한 칸 둔다.
for (int i = 0; i < n; i++) {
    prefix[i + 1] = prefix[i] + arr[i];
}
return prefix[right + 1] - prefix[left]; // 끝까지의 합에서 시작 직전까지를 뺀다.
```

**조건** 고정 배열, 양 끝 포함 [left,right] 합. 전처리 O(n), 조회 O(1).

**분류** [[00 Maps/Prefix Sums|Prefix Sums]]

**비교** [[60 Range Data Structures/Fenwick Tree|Fenwick Tree]]
