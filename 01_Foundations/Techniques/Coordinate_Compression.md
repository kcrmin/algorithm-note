---
type: technique
tags:
  - coordinate-compression
language: java
status: in-review
source: []
---

# Coordinate Compression (좌표 압축)

> **한 줄 요약:** 값의 상대적 순서만 유지하고, 큰/희소한 값을 0..(M-1)의 작은 인덱스로 압축

## Core Concept

- **When to use:**
  - 값의 범위가 매우 크지만(예: 1e9), 서로 다른 값 개수는 적을 때
  - 좌표(시간, 위치 등)를 인덱스로 바꿔 배열/펜윅/세그트리를 쓰고 싶을 때
- **Key Point:**
  - 정렬 후 중복 제거한 배열 `vals`를 만든다
  - 원래 값 x는 `lower_bound(vals, x)`의 인덱스로 치환
  - 값의 크기 자체는 버리고 "순서/동등성"만 보존

## API & State Change

| Step | Code | 의미 |
| :--- | :--- | :--- |
| Collect | `vals.add(x)` | 값 수집 |
| Sort+Unique | `sort(vals); unique` | 압축 기준 만들기 |
| Compress | `idx = lower_bound(vals, x)` | 값 → 인덱스 |
| Decompress | `x = vals[idx]` | 인덱스 → 원래 값 |

## Patterns

### Pattern A: 1D Coordinate Compression (Java)

```java
int[] vals = a.clone();
Arrays.sort(vals);
int m = 0;
int[] uniq = new int[n];
for (int i = 0; i < n; i++) {
    if (i == 0 || vals[i] != vals[i - 1]) uniq[m++] = vals[i];
}
int[] comp = new int[n];
for (int i = 0; i < n; i++) {
    int idx = Arrays.binarySearch(uniq, 0, m, a[i]);
    comp[i] = idx;
}
```

### Pattern B: Map 기반 압축 (많이 씀)

```java
int[] sorted = a.clone();
Arrays.sort(sorted);
ArrayList<Integer> uniq = new ArrayList<>();
for (int x : sorted) {
    if (uniq.isEmpty() || uniq.get(uniq.size() - 1) != x) uniq.add(x);
}
HashMap<Integer, Integer> map = new HashMap<>();
for (int i = 0; i < uniq.size(); i++) map.put(uniq.get(i), i);
int[] comp = new int[n];
for (int i = 0; i < n; i++) comp[i] = map.get(a[i]);
```

## Pitfalls & Tips

- 압축은 "순서"만 유지한다 (거리/차이는 보존되지 않음)
- 값이 입력에 반드시 존재하면 binarySearch 사용 가능; 없으면 lower_bound 구현
- 중복 제거를 빼먹으면 인덱스가 커지고 의미가 흐려짐
- 좌표가 long이면 long 배열/맵 사용
