---
type: data-structure
tags:
  - data-structure
  - segment-tree
language: java
status: in-review
source: []
---

# Segment Tree
> **한 줄 요약:** 구간 질의와 단일 업데이트를 모두 빠르게 처리하기 위한 트리 기반 자료구조

## Core Concept
* **When to use:**  
  - 배열에서 구간 합/최솟값/최댓값을 자주 물어볼 때
  - 값이 중간에 계속 갱신되는 상황
* **Key Point:**
    * 배열을 구간 단위로 나눠 트리 형태로 관리
    * Query와 Update 모두 O(log N)
    * 정적 쿼리만 있으면 Prefix Sum이 더 적합 (되면 안 되는 케이스)
    * B형에서 재귀 세그트리도 충분히 통과되지만, **반복형(Iterative)** 구현이 상수 시간이 더 유리한 경우가 많음

## API & State Change
| Operation | Code Snippet | State / Return | 설명 |
| :--- | :--- | :--- | :--- |
| Init | `build(1, 0, n-1);` | 트리 생성 | 초기 배열로 트리 구성 |
| Query | `query(1, 0, n-1, l, r);` | 값 반환 | 구간 [l,r] 질의 |
| Update | `update(1, 0, n-1, idx, val);` | 상태 변경 | 단일 값 갱신 |
| Range update | `lazy[node] += delta;` | 지연 갱신 | 구간 업데이트 시 사용 |

## Patterns
### Pattern A: 구간 합 세그먼트 트리 (기본)
- 가장 전형적인 세그먼트 트리 형태
```java
int[] tree;
int n;

void build(int node, int start, int end) {
    if (start == end) {
        tree[node] = arr[start]; // 리프: 원본 배열 값
        return;
    }
    int mid = (start + end) / 2;
    build(node*2, start, mid);
    build(node*2+1, mid+1, end);
    tree[node] = tree[node*2] + tree[node*2+1]; // 자식 합치기
}

int query(int node, int start, int end, int l, int r) {
    if (r < start || end < l) return 0;         // 완전 불일치
    if (l <= start && end <= r) return tree[node]; // 완전 포함
    int mid = (start + end) / 2;
    return query(node*2, start, mid, l, r)
         + query(node*2+1, mid+1, end, l, r);
}

void update(int node, int start, int end, int idx, int val) {
    if (start == end) {
        tree[node] = val;
        return;
    }
    int mid = (start + end) / 2;
    if (idx <= mid) update(node*2, start, mid, idx, val);
    else update(node*2+1, mid+1, end, idx, val);
    tree[node] = tree[node*2] + tree[node*2+1];
}
```

### Pattern B: 반복형 세그먼트 트리 (상수 시간 최적화)
- 배열 기반으로 재귀 호출 비용을 줄이는 구현
```java
class IterSegTree {
    int size;      // 리프 시작 인덱스
    long[] tree;   // 1-index 사용

    IterSegTree(int n) {
        size = 1;
        while (size < n) size <<= 1;
        tree = new long[size << 1];
    }

    void build(long[] a) {
        for (int i = 0; i < a.length; i++) tree[size + i] = a[i];
        for (int i = size - 1; i > 0; i--) tree[i] = tree[i << 1] + tree[i << 1 | 1];
    }

    void set(int idx, long value) {
        int node = size + idx;
        tree[node] = value;
        node >>= 1;
        while (node > 0) {
            tree[node] = tree[node << 1] + tree[node << 1 | 1];
            node >>= 1;
        }
    }

    long query(int l, int r) { // [l, r)
        long res = 0;
        for (l += size, r += size; l < r; l >>= 1, r >>= 1) {
            if ((l & 1) == 1) res += tree[l++]; // 오른쪽 형제 없음
            if ((r & 1) == 1) res += tree[--r]; // 왼쪽 형제 선택
        }
        return res;
    }
}
```

## Pitfalls & Tips
- 트리 크기는 보통 `4 * N`으로 잡으면 안전
- mid 계산 실수, 인덱스 범위 실수 빈번
- 값이 안 바뀌면 Prefix Sum이 더 단순하고 빠르다
- 구간 업데이트 + 구간 질의가 같이 나오면 Lazy Propagation이 사실상 필수
