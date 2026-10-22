---
type: template
tags: ["Java"]
category: java-essentials
categories: [java-essentials]
---

# Sorting

```text
(start,end)   (2,5)  (1,3)  (1,5)
                    ↓ end ↑, 같으면 start ↑
sorted        (1,3)  (1,5)  (2,5)

compare(a,b)    음수 → a 먼저
               0    → 같은 우선순위
               양수 → b 먼저
```

## Steps

| Step | Pseudocode |
| --- | --- |
| **Primary** | // 종료 시각을 비교한다. 다르면 이 결과로 순서를 결정한다. |
| **Tie-break** | // 종료 시각이 같을 때만 시작 시각을 비교한다. |

## Java

```java
Arrays.sort(meetings, (a, b) -> {
    int order = Integer.compare(a[1], b[1]);

    if (order != 0) {
        return order;
    }

    return Integer.compare(a[0], b[0]);
});
```

`meetings[i] = {start, end}` · 끝↑, 동점이면 시작↑ · `O(n log n)`

## Watch points

| 선택 | 이유 / 실수 |
| --- | --- |
| `compare(a, b)` | a가 작으면 음수 → a가 먼저 온다. 오름차순이다. |
| 내림차순 | 뺄셈이나 결과 부호를 억지로 바꾸지 말고 `Integer.compare(b, a)`로 인자 순서를 바꾼다. |
| `a - b` 금지 | 차이가 int 범위를 넘으면 부호가 뒤집혀 정렬 기준이 깨진다. |
| 정렬 대상 | 이 예시는 int[][]의 행을 비교한다. int[]에는 Comparator를 넘길 수 없다. |

---

**분류** [[00 Maps/Java Essentials|Java Essentials]]

**비교** [[90 Data Structures/Frequency Array and Buckets|Frequency Array and Buckets]]
