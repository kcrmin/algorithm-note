---
type: data-structure
tags:
  - data-structure
  - hash
language: java
status: in-review
source: []
---

# Hash (HashMap / HashSet)
> **한 줄 요약:** 키를 해시로 매핑해 평균 O(1) 삽입/조회/삭제를 제공하는 기본 자료구조

## Core Concept
* **When to use:**
  - 값 존재 여부 확인, 빈도 카운트, 중복 제거
  - 문자열/좌표/상태를 key로 빠르게 매핑할 때
* **Key Point:**
  - 평균 O(1)이지만 충돌이 많아지면 느려질 수 있음
  - 키 타입(`int`, `long`, `String`, 커스텀 객체`)에 따라 성능 차이가 큼
  - B형에서는 범위가 작으면 해시보다 배열 버킷이 더 빠른 경우가 많음

## API & State Change
| Operation | Code Snippet | State / Return | 설명 |
| :--- | :--- | :--- | :--- |
| Put | `map.put(k, v);` | 상태 변경 | 값 저장/갱신 |
| Get | `map.get(k)` | 값 반환 | 키 조회 |
| Count up | `map.merge(k, 1, Integer::sum);` | 상태 변경 | 빈도 +1 |
| Exists | `set.contains(x)` | true/false | 존재 여부 |

## Patterns
### Pattern A: 빈도 카운트
```java
Map<Integer, Integer> freq = new HashMap<>();

for (int x : arr) {
    // 분기(if containsKey) 없이 한 번에 누적해 코드 단순화 + 평균 O(1)
    freq.put(x, freq.getOrDefault(x, 0) + 1);
}
```

### Pattern B: 문자열 key 대신 정수 인코딩
- 상태를 정수/long으로 인코딩하면 해시 비용과 메모리를 줄일 수 있음
```java
// (x, y)를 64-bit key로 인코딩
long key = (((long) x) << 32) ^ (y & 0xffffffffL);
HashSet<Long> seen = new HashSet<>();
seen.add(key);
```

### Pattern C: 좌표 압축 + 배열로 대체
- key 범위가 크지만 고유 개수가 작을 때 실전 최적화 패턴
```java
int[] vals = arr.clone();
Arrays.sort(vals); // stream distinct보다 정렬+직접 unique가 상수 시간 유리한 편
int[] uniq = new int[vals.length];
int m = 0;
for (int i = 0; i < vals.length; i++) {
    if (i == 0 || vals[i] != vals[i - 1]) uniq[m++] = vals[i];
}
Map<Integer, Integer> comp = new HashMap<>(m * 2); // rehash 횟수 감소
for (int i = 0; i < m; i++) comp.put(uniq[i], i); // 큰 key를 0..M-1로 압축

int[] cnt = new int[m];
for (int x : arr) cnt[comp.get(x)]++; // 이후는 배열 접근으로 HashMap 반복 조회를 최소화
```

## Pitfalls & Tips
- `map.get(k) + 1`는 null일 때 NPE, `getOrDefault` 사용
- 커스텀 객체를 key로 쓰면 `equals/hashCode`를 정확히 구현해야 함
- 반복문 내부에서 문자열 key를 계속 새로 만들면 느려짐 (가능하면 정수 인코딩)
