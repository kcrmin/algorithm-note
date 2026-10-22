---
type: map
level: family
category: range-queries
categories: [range-queries]
branch: sequences
---

# Range Queries

[[00 Maps/Sequences|Sequences]]

```text
원소:   a  b  c  d  e  f
조회:     [─────────]      구간을 모아 답하기
갱신:        ↑             한 원소 또는 구간 바꾸기
```

| 데이터가 어떻게 바뀌는가 | 필요한 답 / 역할 | 템플릿 |
| --- | --- | --- |
| 한 원소에 변화량 더하기 | 구간 합 · 블록을 따라 + / − 이동 | [[60 Range Data Structures/Fenwick Tree\|Fenwick Tree]] |
| 한 원소를 새 값으로 대입 | 구간 합·min·max · 부모를 다시 계산 | [[60 Range Data Structures/Segment Tree\|Segment Tree]] |
| 구간 전체에 더하기 | 구간 합 · 자식 갱신을 미뤄 두기 | [[60 Range Data Structures/Lazy Propagation\|Lazy Propagation]] |
| 값이 바뀌지 않음 | 구간 min·max·gcd · 겹치는 두 블록 | [[60 Range Data Structures/Sparse Table\|Sparse Table]] |
| 좌표 값이 너무 큼 | 순서만 보존하며 작은 인덱스로 바꾸기 | [[60 Range Data Structures/Coordinate Compression\|Coordinate Compression]] |

Sparse Table의 겹치는 두 블록 조회는 합에 사용할 수 없다. Lazy Propagation 노트의 코드는 **구간 덧셈 + 구간 합**용이다.
