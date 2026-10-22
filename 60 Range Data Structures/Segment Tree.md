---
type: template
tags: ["DataStructures"]
aliases: ["Segment_Tree"]
---

# Segment Tree

```java
// 필드 int size; long[] tree; 를 준비하고 init(arr)부터 호출한다.
// 0-index. set은 한 원소 대입, sum은 오른쪽을 제외한 [left,right)의 합이다.
void init(long[] arr) {
    size = 1;
    // 리프를 같은 높이에 두려면 크기를 2의 거듭제곱으로 맞춘다.
    while (size < arr.length) size <<= 1;
    tree = new long[size * 2];
    for (int i = 0; i < arr.length; i++) tree[size + i] = arr[i];
    for (int i = size - 1; i > 0; i--) {
        tree[i] = tree[i * 2] + tree[i * 2 + 1];
    }
}
void set(int index, long value) {
    int node = size + index; // 원소는 리프에 있고 부모 번호는 자식 번호 / 2다.
    tree[node] = value;
    for (node /= 2; node > 0; node /= 2) {
        // 바뀐 리프의 조상만 다시 합친다.
        tree[node] = tree[node * 2] + tree[node * 2 + 1];
    }
}
long sum(int left, int right) {
    long result = 0;
    for (left += size, right += size; left < right; left /= 2, right /= 2) {
        // 오른쪽 자식은 혼자 더해야 범위 밖의 왼쪽 형제를 피한다.
        if ((left & 1) == 1) result += tree[left++];
        // 끝은 제외이므로 바로 앞 왼쪽 자식만 더한다.
        if ((right & 1) == 1) result += tree[--right];
    }
    return result;
}
```

**예시**

```text
             10             tree[1]
           /    \
          3      7           tree[2..3]
         / \    / \
        2   1  3   4         tree[size + index], size=4
[1,3) 합: 인덱스 1,2의 값 1+3 = 4 (3번은 제외)
```

**주의** 합의 항등원은 0. min/max로 바꾸면 합치는 연산과 항등원도 바꾼다. 생성 O(N)·연산 O(log N)·공간 O(N).

[[Fenwick Tree]] · [[Lazy Propagation]] · [[Sparse Table]]
