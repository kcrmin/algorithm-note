---
type: template
tags: ["DataStructures"]
aliases: ["Sparse_Table"]
category: range-queries
categories: [range-queries]
---

# Sparse Table

```text
고정 배열: [8, 6, 7, 3, 9, 2]
미리 저장: 길이 1, 2, 4, 8, ... 구간의 최솟값

min(0,5): 길이 6 → 들어가는 가장 큰 2의 거듭제곱 = 4

왼쪽 블록: [8, 6, 7, 3]          → 3
오른쪽 블록:      [7, 3, 9, 2]    → 2
                        합친 답: min(3,2) = 2

겹친 7, 3을 두 번 봐도 최솟값은 같다.
```

| Step | Pseudocode |
| --- | --- |
| Seed | // 길이 1 구간은 원소 자체를 저장한다 |
| Build | // 길이가 절반인 두 구간의 최솟값을 합친다 |
| Select | // 조회 길이 이하의 가장 큰 2의 거듭제곱 길이를 골라, 같은 길이의 블록을 양 끝에 둔다 |
| Answer | // 두 블록의 최솟값을 비교한다. 겹친 부분은 답에 영향을 주지 않는다 |

## Java

```java
int[][] table;

void build(int[] arr) {
    int n = arr.length;
    int levels = 32 - Integer.numberOfLeadingZeros(n);
    table = new int[levels][n];
    table[0] = arr.clone();

    for (int k = 1; k < levels; k++) {
        int half = 1 << (k - 1);
        for (int i = 0; i + (1 << k) <= n; i++) {
            table[k][i] = Math.min(
                    table[k - 1][i],
                    table[k - 1][i + half]);
        }
    }
}

int min(int left, int right) {
    int k = 31 - Integer.numberOfLeadingZeros(right - left + 1);
    return Math.min(table[k][left], table[k][right - (1 << k) + 1]);
}
```

## Watch points

| 선택 | 이유 / 실수 |
| --- | --- |
| `table[k][i]` | **i에서 시작하는 길이 2ᵏ 구간**의 최솟값. k는 길이 자체가 아니다 |
| `31 - ...` / `32 - ...` | 조회는 `floor(log₂ 길이)`라는 **층 번호**. 배열 할당은 0층도 포함한 **층 개수**라서 1 더 큼 |
| 오른쪽 시작 위치 | 끝 포함이므로 `right - 블록 길이 + 1`. `+1`을 빼먹으면 구간이 한 칸 왼쪽으로 밀림 |
| min / max / GCD | 같은 값을 중복 반영해도 답이 같은 연산은 가능. 합은 겹친 원소를 두 번 더하므로 이 조회식 사용 불가 |
| 고정 배열 | 원소가 바뀌면 저장된 구간값이 낡는다. 변경이 잦으면 Segment Tree 등 사용 |

비어 있지 않은 배열 · 0-index, 양 끝 포함 `[left, right]` · 전처리·공간 `O(n log n)`, 조회 `O(1)`.

**분류** [[00 Maps/Range Queries|Range Queries]]

**비교** [[60 Range Data Structures/Segment Tree|Segment Tree]]
