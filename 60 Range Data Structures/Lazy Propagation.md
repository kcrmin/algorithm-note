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
// init: 구간 합·미전달 태그 초기화
void init(int n) { tree = new long[4 * n]; lazy = new long[4 * n]; }
// apply: 합에 길이×delta, 태그에 delta 누적
void apply(int node, int lo, int hi, long delta) {
    tree[node] += delta * (hi - lo + 1);
    lazy[node] += delta;
}
// push: 미전달 갱신을 자식에 넘기고 부모 태그 비우기
void push(int node, int lo, int hi) {
    if (lazy[node] == 0 || lo == hi) return;
    int mid = (lo + hi) >>> 1;
    apply(node * 2, lo, mid, lazy[node]);
    apply(node * 2 + 1, mid + 1, hi, lazy[node]);
    lazy[node] = 0;
}
// add: 덮으면 적용, 일부면 나눠 갱신 후 합치기
void add(int node, int lo, int hi, int left, int right, long delta) {
    if (right < lo || hi < left) return;
    if (left <= lo && hi <= right) {
        apply(node, lo, hi, delta);
        return;
    }
    // beforeSplit: 부모 갱신부터 내려야 자식이 최신
    push(node, lo, hi);
    int mid = (lo + hi) >>> 1;
    add(node * 2, lo, mid, left, right, delta);
    add(node * 2 + 1, mid + 1, hi, left, right, delta);
    tree[node] = tree[node * 2] + tree[node * 2 + 1];
}
// sum: 덮으면 반환, 일부면 나눠 합산
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
