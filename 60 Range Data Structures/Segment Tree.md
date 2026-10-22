---
type: template
tags: ["DataStructures"]
aliases: ["Segment_Tree"]
---

# Segment Tree

**언제** 배열 값이 바뀌고 구간 합을 물을 때. 부모는 두 자식의 합이다.

```text
             10             tree[1]
           /    \
          3      7           tree[2..3]
         / \    / \
        2   1  3   4         tree[size + index], size=4
[1,3) 합: 인덱스 1,2의 값 1+3 = 4 (3번은 제외)
```

**코드** 필드 `int size; long[] tree;`. `init(a)` 후 `set(index,value)`, `sum(left,right)` 호출. **0-index·[left,right)**.

```java
void init(long[] arr) {
    size = 1;
    while (size < arr.length) size <<= 1;
    tree = new long[size * 2];
    for (int i = 0; i < arr.length; i++) tree[size + i] = arr[i];
    for (int i = size - 1; i > 0; i--) {
        tree[i] = tree[i * 2] + tree[i * 2 + 1];
    }
}
void set(int index, long value) {
    int node = size + index;
    tree[node] = value;
    for (node /= 2; node > 0; node /= 2) {
        tree[node] = tree[node * 2] + tree[node * 2 + 1];
    }
}
long sum(int left, int right) {
    long result = 0;
    for (left += size, right += size; left < right; left /= 2, right /= 2) {
        if ((left & 1) == 1) result += tree[left++]; // ①
        if ((right & 1) == 1) result += tree[--right]; // ②
    }
    return result;
}
```

| 암기 포인트 | 이유 |
|---|---|
| `size + index` → `/ 2` | 리프에서 시작해 바뀐 조상만 다시 합친다. |
| ① 왼쪽이 홀수 | 오른쪽 자식이다. 혼자 더하고 제외해야 범위 밖 형제를 피한다. |
| ② 오른쪽이 홀수 | 오른쪽 끝은 제외다. 바로 앞의 왼쪽 자식만 더하고 올라간다. |

**주의** 합의 항등원은 0. min/max로 바꾸면 합치는 연산과 항등원도 바꾼다. 생성 O(N)·연산 O(log N)·공간 O(N).

[[Fenwick Tree]] · [[Lazy Propagation]] · [[Sparse Table]]
