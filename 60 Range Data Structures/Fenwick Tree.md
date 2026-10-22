---
type: template
tags: ["DataStructures"]
aliases: ["Binary Indexed Tree"]
---

# Fenwick Tree

```java
// 전부 1-index. 원소 수 n과 초기 0인 long[] tree = new long[n+1]을 준비한다.
// i & -i는 최하위 1비트: 6(110₂) -> 2(010₂).
// tree[i]는 끝이 i이고 길이가 i & -i인 구간 합. 초기값도 add(i,value)로 넣는다.
void add(int index, long delta) {
    // 이 원소를 포함하는 상위 구간도 바뀐다. index=0 금지.
    for (int i = index; i <= n; i += i & -i) {
        tree[i] += delta; // 대입하려면 delta에 새 값-기존 값을 넣는다.
    }
}
long prefix(int index) {
    long result = 0;
    // 더한 구간 길이만큼 건너뛰어 중복을 피한다.
    for (int i = index; i > 0; i -= i & -i) {
        result += tree[i];
    }
    return result;
}
long sum(int left, int right) {
    // [left,right]: 시작 직전까지의 합을 뺀다.
    return prefix(right) - prefix(left - 1);
}
```

**예시**

```text
내부 번호  1  2  3  4  5  6     값: 2 1 3 4 5 6
tree[6] = [5..6] 합 11       ← 길이 lowbit(6)=2
tree[4] = [1..4] 합 10       ← 길이 lowbit(4)=4
prefix(6): 6 → 4 → 0, 합 11+10=21
add(3,+7): 3 → 4 → (8)       ← 포함하는 큰 구간으로
```

**주의** `add(0,…)`은 무한 반복. 대입은 `새 값-기존 값`을 더한다. 연산 O(log N)·공간 O(N).

[[Prefix Sum]] · [[Segment Tree]] · [[Coordinate Compression]]
