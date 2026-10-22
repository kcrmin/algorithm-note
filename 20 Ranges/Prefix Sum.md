---
type: template
tags: ["Ranges"]
aliases: ["Prefix_Sum"]
category: prefix-sums
categories: [prefix-sums]
---

# Prefix Sum

```text
index       0   1   2   3
arr         2   4   1   3
prefix  0   2   6   7  10
        ↑ 앞 0개        ↑ 앞 4개

sum[1..2] = prefix[3] - prefix[1]
          = (2+4+1) - (2) = 5
```

## Steps

| Step | Pseudocode |
| --- | --- |
| **Build** | // 앞에서부터 합을 쌓는다. 앞에 원소가 0개인 합도 저장한다. |
| **Query** | // 오른쪽 끝까지의 합에서 왼쪽 시작 전까지의 합을 뺀다. |

## Java

```java
int n = arr.length;
long[] prefix = new long[n + 1];

for (int i = 0; i < n; i++) {
    prefix[i + 1] = prefix[i] + arr[i];
}

return prefix[right + 1] - prefix[left];
```

고정 배열 · `0 ≤ left ≤ right < n` · 전처리 `O(n)`, 조회 `O(1)`

## Watch points

| 선택 | 이유 / 실수 |
| --- | --- |
| `prefix[i]` | 앞 i개의 합. arr의 i번째까지가 아니라 i개다. |
| 오른쪽만 `+ 1` | 양 끝 포함 [left,right]에서 right까지는 right+1개, left 전까지는 left개다. |
| 원소 갱신 | 원소가 바뀌면 뒤쪽 누적 합도 바뀐다. 빈번한 갱신에는 Fenwick Tree를 쓴다. |
| `long` | 개별 원소가 int여도 여러 개의 합은 int를 넘을 수 있다. |

---

**분류** [[00 Maps/Prefix Sums|Prefix Sums]]

**비교** [[60 Range Data Structures/Fenwick Tree|Fenwick Tree]]
