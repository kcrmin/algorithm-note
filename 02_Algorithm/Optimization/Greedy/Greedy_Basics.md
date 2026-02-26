---
type: concept
tags:
  - greedy
language: java
status: in-review
source: []
---

# Greedy (그리디)

> **한 줄 요약:** 매 단계에서 지금 보기에 최선인 선택을 반복해 (전역) 최적해를 얻는 전략

## Core Concept

- **When to use:** 선택의 순서만 정하면 되는 문제, 활동 선택·스케줄링·허프만 등
- **Key Point:**
  - **탐욕적 선택**: 매 단계에서 지역적으로 최선인 선택
  - **최적 부분 구조**: 부분 문제의 최적해가 전체 최적해에 포함됨
  - 증명이 필요한 경우가 많음 (반례 없음, 또는 exchange argument)

## API & State Change

| 관점 | 설명 |
| :--- | :--- |
| 정렬 | 기준(끝 시간, 비용 등)으로 정렬 후 순서대로 선택 |
| PQ | “현재 가능한 후보 중 최선”을 반복적으로 꺼냄 |
| 선택 규칙 | “매번 x를 선택” 같은 규칙을 세우고 검증 |

## Patterns

### Pattern A: 구간 스케줄링 (겹치지 않게 최대 개수)

- 끝 시간 기준 오름차순 정렬 후, 겹치지 않으면 선택

```java
// intervals: [시작, 끝]
Arrays.sort(intervals, Comparator.comparingInt(a -> a[1]));
int count = 0, last = -1;
for (int[] iv : intervals) {
    if (iv[0] >= last) {
        count++;
        last = iv[1];
    }
}
```

### Pattern B: PQ 그리디 (매번 “현재 최선” 선택)

- 예: 합치기 비용 최소 (작은 것 두 개씩 합치기)

```java
PriorityQueue<Long> pq = new PriorityQueue<>();
for (long x : a) pq.add(x);
long total = 0;
while (pq.size() > 1) {
    long a1 = pq.poll(), a2 = pq.poll();
    total += a1 + a2;
    pq.add(a1 + a2);
}
```

### Pattern C: 정렬 후 한 방향 스캔

- “비용 대비 이득” 등으로 정렬한 뒤 순서대로 선택/포기

```java
Arrays.sort(items, (a, b) -> ...);  // 기준에 맞게
for (Item it : items) {
    if (possible(it)) take(it);
}
```

## Pitfalls & Tips

- 그리디가 맞는지 반례를 찾아 보거나 증명 시도
- 정렬 기준이 잘못되면 틀림 (끝 시간 vs 시작 시간 등)
- 동일한 “가치”일 때 부가 기준(끝 시간 등)으로 2차 정렬
