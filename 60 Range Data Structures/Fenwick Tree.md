---
type: template
tags: ["DataStructures"]
aliases: ["Binary Indexed Tree"]
category: range-queries
categories: [range-queries]
---

# Fenwick Tree

```java
int n;
long[] tree;
// init: 원소 수 설정·합 초기화
void init(int length) { n = length; tree = new long[n + 1]; }
// add: index를 포함하는 상위 구간에 delta 반영
void add(int index, long delta) {
    // ascend: 최하위 1비트만큼 다음 부모로
    for (int i = index; i <= n; i += i & -i) tree[i] += delta;
}
// prefix: [1,index] 합
long prefix(int index) {
    long result = 0;
    // collect: 더한 구간 길이만큼 빼서 중복 회피
    for (int i = index; i > 0; i -= i & -i) result += tree[i];
    return result;
}
// sum: [left,right] 합
long sum(int left, int right) {
    return prefix(right) - prefix(left - 1);
}
```

init(n) 후 1-index [left,right] · add의 index≥1, 대입은 새 값−기존 값을 더한다 · 연산 O(log n), 공간 O(n).

**분류** [[00 Maps/Range Queries|Range Queries]]
