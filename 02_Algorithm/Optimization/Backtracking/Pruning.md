---
type: technique
tags:
  - pruning
  - backtracking
language: java
status: in-review
source: []
---

# Pruning 정리

## Pruning이란?
Pruning(가지치기)은 **정답이 될 수 없는 탐색 분기를 미리 차단**해 탐색량을 줄이는 기법이다.  
정답은 유지하면서 **불필요한 DFS/백트래킹 호출을 제거**하는 것이 목적이다.

## 대표적인 Pruning 패턴

### 1. 범위 초과
```java
if (sum > target) return;
```

### 2. 이미 최적해보다 나쁜 경우
```java
if (time >= best) return;
```

### 3. 남은 것을 다 써도 목표 도달 불가
```java
if (current + maxPossible < target) return;
```

### 4. 방문 중복 제거
```java
if (visited[x][y]) return;
```

## 주의사항
- Pruning은 **정답에는 영향을 주면 안 된다**
- 가능성이 확실히 없을 때만 잘라야 한다

## 요약
Pruning은 탐색의 양만 줄이고,
정답의 집합은 그대로 유지하는 가지치기 기법이다.
