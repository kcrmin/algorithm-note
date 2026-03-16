---
type: data-structure
tags:
  - data-structure
  - bucket
language: java
status: in-review
source: []
---

# Bucket (버킷)
> **한 줄 요약:** 값의 범위가 작거나 구조가 규칙적일 때, 해시/정렬 대신 배열 인덱스로 바로 접근해 속도를 끌어올리는 기법

## Core Concept
* **When to use:**
  - 값 범위가 작고 정수일 때 (빈도 계산, 중복 제거, counting sort)
  - 구간을 블록으로 나눠 빠르게 질의할 때 (sqrt bucket)
* **Key Point:**
  - `HashMap` 대신 `int[] bucket`을 쓰면 상수 시간이 매우 빠름
  - 인덱스 계산만 맞으면 구현이 단순하고 디버깅이 쉬움
  - 메모리는 `범위 크기`에 비례하므로 범위가 너무 크면 비효율

## API & State Change
| Operation | Code Snippet | State / Return | 설명 |
| :--- | :--- | :--- | :--- |
| Init | `int[] freq = new int[MAX+1];` | 초기화 | 값별 카운트 배열 |
| Count | `freq[x]++;` | 상태 변경 | x 등장 횟수 증가 |
| Query | `freq[x]` | 값 반환 | x의 빈도 조회 |
| Clear | `Arrays.fill(freq, 0);` | 초기화 | 테스트케이스 재사용 |

## Patterns
### Pattern A: 빈도 버킷 (가장 자주 등장)
```java
int[] freq = new int[MAX_VALUE + 1];

for (int x : arr) {
    freq[x]++; // 값 x의 등장 횟수 누적
}

int bestValue = 0;
int bestCount = -1;
for (int v = 0; v <= MAX_VALUE; v++) {
    if (freq[v] > bestCount) {
        bestCount = freq[v];
        bestValue = v;
    }
}
```

### Pattern B: Counting Sort
- 값 범위가 작을 때 `O(N + K)` 정렬
```java
int[] freq = new int[MAX_VALUE + 1];
for (int x : arr) freq[x]++;

int idx = 0;
for (int v = 0; v <= MAX_VALUE; v++) {
    int c = freq[v];
    while (c-- > 0) { // 빈도만큼 연속 배치: 비교 기반 정렬보다 상수 시간이 작다
        arr[idx++] = v;
    }
}
```

### Pattern C: 오프셋 버킷 (음수 값 포함)
```java
int min = -100000, max = 100000;
int offset = -min;                   // 값 x -> index x + offset
int[] freq = new int[max - min + 1];

for (int x : arr) freq[x + offset]++;
```

## Pitfalls & Tips
- 범위(`max-min`)가 큰데 버킷을 쓰면 메모리 초과 위험
- 테스트케이스가 여러 개인 문제는 배열 재할당보다 `Arrays.fill` 재사용이 빠를 때가 많음
- 범위가 애매하면 `좌표 압축 + 버킷` 조합이 실전에서 안전
