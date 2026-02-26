---
type: data-structure
tags:
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

## API & State Change
| Operation | Code Snippet | State / Return | 설명 |
| :--- | :--- | :--- | :--- |
| Init | `build(1, 0, n-1);` | 트리 생성 | 초기 배열로 트리 구성 |
| Query | `query(1, 0, n-1, l, r);` | 값 반환 | 구간 [l,r] 질의 |
| Update | `update(1, 0, n-1, idx, val);` | 상태 변경 | 단일 값 갱신 |

## Patterns
### Pattern A: 구간 합 세그먼트 트리 (기본)
- 가장 전형적인 세그먼트 트리 형태
```java
int[] tree;
int n;

void build(int node, int start, int end) {
    if (start == end) {
        tree[node] = arr[start];
        return;
    }
    int mid = (start + end) / 2;
    build(node*2, start, mid);
    build(node*2+1, mid+1, end);
    tree[node] = tree[node*2] + tree[node*2+1];
}

int query(int node, int start, int end, int l, int r) {
    if (r < start || end < l) return 0;
    if (l <= start && end <= r) return tree[node];
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

## Pitfalls & Tips
- 트리 크기는 보통 `4 * N`으로 잡으면 안전
- mid 계산 실수, 인덱스 범위 실수 빈번
- 값이 안 바뀌면 Prefix Sum이 더 단순하고 빠르다
