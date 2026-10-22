---
type: template
tags: ["DataStructures"]
aliases: ["Segment_Tree"]
category: range-queries
categories: [range-queries]
---

# Segment Tree

```text
배열: [1, 2, 3, 4]       각 노드 = 담당 구간의 합

                 [0,4) = 10
                /          \
         [0,2) = 3        [2,4) = 7
          /    \           /    \
         1      2         3      4

sum(1,4): [1,2) + [2,4) = 2 + 7 = 9
set(2,8): 리프 3 → 8, 부모 7 → 12, 루트 10 → 15
```

| Step | Pseudocode |
| --- | --- |
| Init | // 리프에 원소를 놓고 두 자식의 합으로 부모를 채운다 |
| Set | // 바뀐 리프에서 루트까지의 합만 다시 계산한다 |
| Sum | // 양 경계에서 온전히 포함되는 노드를 더하고 위로 올라간다 |

## Java

```java
int size;
long[] tree;

void init(long[] arr) {
    size = 1;
    while (size < arr.length) {
        size <<= 1;
    }

    tree = new long[size * 2];
    for (int i = 0; i < arr.length; i++) {
        tree[size + i] = arr[i];
    }
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
        // 홀수는 오른쪽 자식: 왼쪽 형제는 범위 밖
        if ((left & 1) == 1) {
            result += tree[left++];
        }
        // right 자체는 제외: 바로 앞 왼쪽 자식을 가져온다
        if ((right & 1) == 1) {
            result += tree[--right];
        }
    }

    return result;
}
```

## Watch points

| 선택 | 이유 / 실수 |
| --- | --- |
| `[left, right)` | 오른쪽 끝은 제외. 양 끝 포함 문제의 조회는 `sum(left, right + 1)` |
| `left++` / `--right` | 왼쪽은 **현재 노드를 더한 뒤** 이동. 오른쪽은 제외 경계이므로 **먼저 한 칸 줄인 뒤** 더한다 |
| 홀수 / 짝수 | 홀수 노드는 오른쪽 자식. 경계에 홀로 남은 노드를 먼저 처리해야 부모에 범위 밖 원소가 섞이지 않는다 |
| `set` / `add` | 여기서는 새 값으로 대입. 변화량 갱신이 필요하면 기존 값에 더한 값을 대입 |
| 합 → min / max | 결합 연산과 초기값을 함께 변경. 미사용 리프와 누적 결과도 각각 `+∞` / `−∞`로 초기화 |

`init(arr)` 후 사용 · n=arr.length, 0-index · `set`: `0 ≤ index < n` · `sum`: `0 ≤ left ≤ right ≤ n` (빈 구간 합 0) · 생성 `O(n)`, 연산 `O(log n)`, 공간 `O(n)`.

**분류** [[00 Maps/Range Queries|Range Queries]]

**비교** [[60 Range Data Structures/Fenwick Tree|Fenwick Tree]]
