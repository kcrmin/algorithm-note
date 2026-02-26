---
deprecated: true
note: "Inbox 정리용 노트 (deprecated)"
---

# Java HashMap
> **한 줄 요약:** key → value 매핑 자료구조 (빈도 카운팅과 빠른 조회의 표준)

## 1. Core Concept
* **When to use:**  
* **Key Point:**
    * 가장 많이 쓰는 목적은 빈도 카운팅
    * `getOrDefault + put` 조합이 사실상 표준
    * 순서 필요 없음 (순서 필요 시 TreeMap / LinkedHashMap)
* **Time Complexity:** 평균 O(1)

## 2. API & State Change (필수 함수)
| Operation | Code Snippet | Return / State | 설명 |
| :--- | :--- | :--- | :--- |
| Init | `new HashMap<>()` | map 생성 | 초기화 |
| Put | `map.put(k,v)` | 상태 변경 | 삽입 / 갱신 |
| Get | `map.get(k)` | V / null | 값 조회 |
| Get(Default) | `map.getOrDefault(k,d)` | V | 없으면 d |
| Check | `map.containsKey(k)` | boolean | 키 존재 |
| Remove | `map.remove(k)` | 상태 변경 | 삭제 |
| Size | `map.size()` | int | 원소 수 |
| Iterate(Key) | `map.keySet()` | Set<K> | key 순회 |
| Iterate(Entry) | `map.entrySet()` | Set<Entry<K,V>> | key+value 순회 |

## 3. Code Templates (Patterns)

### Pattern A: 빈도 카운팅 (가장 기본)
설명: 숫자/문자 등장 횟수를 세는 표준 패턴.
```java
HashMap<Integer, Integer> map = new HashMap<>();

for (int x : arr) {
    map.put(x, map.getOrDefault(x, 0) + 1);
}
```

### Pattern B: 빈도 감소 + 0 제거
설명: Sliding Window / 투입-제거 패턴에서 자주 사용.
```java
map.put(x, map.get(x) - 1);
if (map.get(x) == 0) map.remove(x);
```

### Pattern C: entrySet()으로 순회 (가장 권장)
설명: key와 value를 동시에 사용할 때 가장 효율적.
```java
for (Map.Entry<Integer, Integer> e : map.entrySet()) {
    int key = e.getKey();
    int value = e.getValue();
}
```

### Pattern D: keySet()으로 순회
설명: key만 필요할 때 사용 (value 접근은 추가 lookup 발생).
```java
for (int key : map.keySet()) {
    int value = map.get(key);
}
```

## 4. Logic Trace (Step-by-Step)
| Step | Operation | State | Explanation |
| :--- | :--- | :--- | :--- |
| 1 | Init | empty map | 생성 |
| 2 | Update | count 변경 | put |
| 3 | Iterate | entrySet/keySet | 순회 |
| End | Output | 결과 | 사용 |

## 5. Pitfalls & Tips
* **Null 주의:** `get()` 결과 바로 연산 금지 → `getOrDefault`
* **entrySet 권장:** key+value 둘 다 쓰면 `entrySet()`이 더 빠름
* **오버플로우:** 카운트 커지면 `long`
* **Trick:**  
  * 빈도/매핑 → HashMap  
  * 존재 여부만 → HashSet