---
type: template
tags: ["DataStructures"]
aliases: ["Segment_Tree"]
category: range-queries
categories: [range-queries]
---

# Segment Tree

```java
int size;
long[] tree;
// init: 리프→부모 순서로 구간 합 구성
void init(long[] arr) {
    size = 1;
    while (size < arr.length) size <<= 1;
    tree = new long[size * 2];
    for (int i = 0; i < arr.length; i++) tree[size + i] = arr[i];
    for (int i = size - 1; i > 0; i--) tree[i] = tree[i * 2] + tree[i * 2 + 1];
}
// set: 원소 대입 후 조상만 갱신
void set(int index, long value) {
    int node = size + index;
    tree[node] = value;
    for (node /= 2; node > 0; node /= 2) {
        tree[node] = tree[node * 2] + tree[node * 2 + 1];
    }
}
// sum: 양 경계에서 포함되는 구간만 골라 합산
long sum(int left, int right) {
    long result = 0;
    for (left += size, right += size; left < right; left /= 2, right /= 2) {
        // takeLeft: 오른쪽 자식만 범위에 들면 혼자 더하기
        if ((left & 1) == 1) result += tree[left++];
        // takeRight: 끝은 제외하므로 바로 앞 노드 포함
        if ((right & 1) == 1) result += tree[--right];
    }
    return result;
}
```

init(arr) 후 0-index [left,right) 합·한 원소 대입 · min/max는 연산·항등원 변경 · 생성 O(n), 연산 O(log n), 공간 O(n).

**분류** [[00 Maps/Range Queries|Range Queries]]

**비교** [[60 Range Data Structures/Fenwick Tree|Fenwick Tree]]
