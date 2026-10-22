---
type: template
tags: ["DataStructures"]
aliases: ["Lazy Segment Tree"]
category: range-queries
categories: [range-queries]
---

# Lazy Propagation

```java
long[] tree, lazy;
void init(int n) { tree = new long[4 * n]; lazy = new long[4 * n]; }
void apply(int node, int lo, int hi, long delta) {
    tree[node] += delta * (hi - lo + 1); // 원소마다 더하므로 합은 길이 배 증가한다.
    lazy[node] += delta;
}
void push(int node, int lo, int hi) {
    if (lazy[node] == 0 || lo == hi) return;
    int mid = (lo + hi) >>> 1;
    apply(node * 2, lo, mid, lazy[node]);
    apply(node * 2 + 1, mid + 1, hi, lazy[node]);
    lazy[node] = 0; // 전달 후 지워야 자식에게 두 번 적용하지 않는다.
}
void add(int node, int lo, int hi, int left, int right, long delta) {
    if (right < lo || hi < left) return;
    if (left <= lo && hi <= right) {
        apply(node, lo, hi, delta);
        return;
    }
    push(node, lo, hi); // 부모의 미전달 갱신부터 내려야 과거 자식을 합치지 않는다.
    int mid = (lo + hi) >>> 1;
    add(node * 2, lo, mid, left, right, delta);
    add(node * 2 + 1, mid + 1, hi, left, right, delta);
    tree[node] = tree[node * 2] + tree[node * 2 + 1];
}
long sum(int node, int lo, int hi, int left, int right) {
    if (right < lo || hi < left) return 0;
    if (left <= lo && hi <= right) return tree[node];
    push(node, lo, hi);
    int mid = (lo + hi) >>> 1;
    return sum(node * 2, lo, mid, left, right)
         + sum(node * 2 + 1, mid + 1, hi, left, right);
}
```

init(n), n≥1 · 0-index [left,right], 루트 (1,0,n−1) · 덧셈·합 전용; 초기값은 원소별 add · 연산 O(log n), 공간 O(n).

**분류** [[00 Maps/Range Queries|Range Queries]]

**사용 원리** [[60 Range Data Structures/Segment Tree|Segment Tree]]
