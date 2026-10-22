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
void init(int length) { n = length; tree = new long[n + 1]; }
void add(int index, long delta) {
    // i의 최하위 1비트만큼 올라가면 이 원소를 포함하는 다음 구간이다.
    for (int i = index; i <= n; i += i & -i) tree[i] += delta;
}
long prefix(int index) {
    long result = 0;
    // 더한 구간 길이만큼 빼서 겹치지 않는 이전 구간으로 간다.
    for (int i = index; i > 0; i -= i & -i) result += tree[i];
    return result;
}
long sum(int left, int right) {
    return prefix(right) - prefix(left - 1);
}
```

init(n) 후 1-index [left,right] · add의 index≥1, 대입은 새 값−기존 값을 더한다 · 연산 O(log n), 공간 O(n).

**분류** [[00 Maps/Range Queries|Range Queries]]
