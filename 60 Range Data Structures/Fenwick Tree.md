---
type: template
tags: ["DataStructures"]
aliases: ["Binary Indexed Tree"]
---

# Fenwick Tree

**언제** 원소 하나에 값을 더하면서 구간 합을 반복해서 물을 때.

```text
내부 번호  1  2  3  4  5  6     값: 2 1 3 4 5 6
tree[6] = [5..6] 합 11       ← 길이 lowbit(6)=2
tree[4] = [1..4] 합 10       ← 길이 lowbit(4)=4
prefix(6): 6 → 4 → 0, 합 11+10=21
add(3,+7): 3 → 4 → (8)       ← 포함하는 큰 구간으로
```

**코드** 전부 **1-index**. 원소 수 `n`과 초기 0인 `tree = new long[n+1]`을 먼저 준비한다. 입력은 `add(i, value)`로 채운다.

```java
void add(int index, long delta) {
    for (int i = index; i <= n; i += i & -i) { // ①
        tree[i] += delta;
    }
}
long prefix(int index) {
    long result = 0;
    for (int i = index; i > 0; i -= i & -i) { // ②
        result += tree[i];
    }
    return result;
}
long sum(int left, int right) {
    return prefix(right) - prefix(left - 1);
}
```

| 암기 포인트 | 이유 |
|---|---|
| `i & -i` = 담당 길이 | 가장 오른쪽 1비트만 남겨 구간 크기를 얻는다. |
| ① 더하며 올라가기 | 해당 원소를 포함하는 상위 구간들도 바뀐다. |
| ② 빼며 왼쪽으로 | 방금 더한 구간을 건너뛰므로 중복 없이 접두 합이 된다. |

**주의** `add(0,…)`은 무한 반복. 대입은 `새 값-기존 값`을 더한다. 연산 O(log N)·공간 O(N).

[[Prefix Sum]] · [[Segment Tree]] · [[Coordinate Compression]]
