---
type: template
tags: ["DataStructures"]
aliases: ["Lazy Segment Tree"]
---

# Lazy Propagation

```java
// n>=1; 필드 long[] tree = new long[4*n], lazy = new long[4*n]; 초기값은 0이다.
// tree는 최신 합, lazy는 자식에게 아직 전달하지 않은 원소당 덧셈이다.
// lo,hi는 현재 노드 담당 범위, left,right는 요청한 범위다.
// 0-index·양끝 포함. 루트는 (1,0,n-1). 초기 a[i]는 add(1,0,n-1,i,i,a[i])로 넣는다.
void apply(int node, int lo, int hi, long delta) {
    // 원소마다 delta를 더하므로 합은 길이 배만큼 늘어난다.
    tree[node] += delta * (hi - lo + 1);
    lazy[node] += delta;
}
void push(int node, int lo, int hi) {
    if (lazy[node] == 0 || lo == hi) return;
    int mid = (lo + hi) >>> 1;
    apply(node * 2, lo, mid, lazy[node]);
    apply(node * 2 + 1, mid + 1, hi, lazy[node]);
    lazy[node] = 0; // 전달한 값을 지우지 않으면 자식에게 두 번 적용된다.
}
void add(int node, int lo, int hi, int left, int right, long delta) {
    if (right < lo || hi < left) return;
    if (left <= lo && hi <= right) {
        apply(node, lo, hi, delta);
        return;
    }
    // 과거 상태의 자식을 고쳐 합치면 부모의 갱신을 잃으므로 먼저 전달한다.
    push(node, lo, hi);
    int mid = (lo + hi) >>> 1;
    add(node * 2, lo, mid, left, right, delta);
    add(node * 2 + 1, mid + 1, hi, left, right, delta);
    tree[node] = tree[node * 2] + tree[node * 2 + 1];
}
long sum(int node, int lo, int hi, int left, int right) {
    if (right < lo || hi < left) return 0;
    // 현재 노드 합은 이미 최신이라 더 내려갈 필요가 없다.
    if (left <= lo && hi <= right) return tree[node];
    push(node, lo, hi);
    int mid = (lo + hi) >>> 1;
    return sum(node * 2, lo, mid, left, right)
         + sum(node * 2 + 1, mid + 1, hi, left, right);
}
```

**예시**

```text
[1,2,3,4] 전체 +5       루트 sum=10+5×4=30, lazy=5
           30(+5)       ← 부모 합은 이미 최신
          /      \
         3        7     ← 자식은 아직 과거
부분 조회 → push        왼쪽 13, 오른쪽 17, 부모 lazy=0
```

**주의** 이 태그는 **덧셈 전용**. 대입 태그와 섞지 않는다. 초기화 O(N log N)·연산 O(log N)·공간 O(N).

[[Segment Tree]] · [[Fenwick Tree]]
