---
type: template
tags: ["DataStructures"]
aliases: ["Binary Indexed Tree"]
category: range-queries
categories: [range-queries]
---

# Fenwick Tree

```text
원소 번호:  1  2  3  4  5  6  7  8

prefix(7): [1───────4] [5──6] [7]  → 세 블록의 합
읽는 순서: 7 → 6 → 4 → 0
            -1  -2  -4             끝의 1비트를 뺀다

add(3):    [3] → [1──────4] → [1──────────────8]
갱신 위치:  3  →     4     →         8
            +1      +4              3을 포함하는 블록들
```

| Step | Pseudocode |
| --- | --- |
| Init | // 1부터 시작하는 합 저장 공간을 0으로 만든다 |
| Add | // 바뀐 원소를 포함하는 더 큰 블록들에 변화량을 더한다 |
| Prefix | // 끝에서부터 겹치지 않는 블록을 모아 앞부분 합을 구한다 |
| Sum | // 오른쪽까지의 합에서 왼쪽 직전까지의 합을 뺀다 |

## Java

```java
int n;
long[] tree;

void init(int length) {
    n = length;
    tree = new long[n + 1];
}

void add(int index, long delta) {
    for (int i = index; i <= n; i += i & -i) {
        tree[i] += delta;
    }
}

long prefix(int index) {
    long result = 0;
    for (int i = index; i > 0; i -= i & -i) {
        result += tree[i];
    }
    return result;
}

long sum(int left, int right) {
    return prefix(right) - prefix(left - 1);
}
```

## Watch points

| 선택 | 이유 / 실수 |
| --- | --- |
| `i & -i` | 맨 오른쪽 1비트의 값 = **i에서 끝나는 블록의 길이**. `6 & -6 = 2`이므로 `tree[6]`은 `[5,6]`의 합 |
| Add는 `+`, Prefix는 `-` | **갱신**은 나를 포함하는 큰 블록으로 이동. **조회**는 방금 더한 블록 길이만큼 왼쪽 이동 |
| `add(0, ...)` 금지 | 0의 블록 길이는 0이라 반복이 진행되지 않는다. `prefix(0)`은 합 0으로 정상 종료 |
| 변화량 / 대입 | `add`는 더하기. 값을 바꾸려면 **새 값 − 기존 값**을 더한다 |
| 초기값 | `init(n)`은 전부 0. 실제 배열 값은 각 위치에 `add`해서 넣는다 |

1-index, 구간 양 끝 포함 `[left, right]` · `1 ≤ left ≤ right ≤ n` · 연산 `O(log n)`, 공간 `O(n)`.

**분류** [[00 Maps/Range Queries|Range Queries]]
