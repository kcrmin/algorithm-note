---
deprecated: true
note: "Inbox 정리용 노트 (deprecated)"
---

# Java HashSet
> **한 줄 요약:** 중복 없는 집합을 유지하며 존재 여부를 평균 O(1)에 확인하는 자료구조

## 1. Core Concept
* **When to use:**  
* **Key Point:**
    * `HashSet<T>`은 중복을 허용하지 않음
    * 값의 존재 여부 판단이 핵심 목적
    * 순서 보장 없음 (순서 필요 시 LinkedHashSet)
* **Time Complexity:** 평균 O(1)

## 2. API & State Change (필수 함수)
| Operation | Code Snippet | Return / State | 설명 |
| :--- | :--- | :--- | :--- |
| Init | `new HashSet<>()` | set 생성 | 초기화 |
| Add | `set.add(x)` | boolean | 새로 추가되면 true |
| Check | `set.contains(x)` | boolean | 존재 여부 |
| Remove | `set.remove(x)` | boolean | 삭제 성공 여부 |
| Size | `set.size()` | int | 원소 수 |
| Iterate | `for (T x : set)` | 순회 | 전체 순회 |

## 3. Code Templates (Patterns)

### Pattern A: 중복 제거 + 유니크 개수
설명: 입력에서 중복을 제거하고 유니크 개수를 구한다.
```java
HashSet<Integer> set = new HashSet<>();

for (int x : arr) {
    set.add(x);
}

int unique = set.size();
```

### Pattern B: 이미 본 값 검출
설명: 이전에 등장한 값인지 빠르게 판단.
```java
HashSet<Integer> seen = new HashSet<>();

for (int x : arr) {
    if (seen.contains(x)) {
        // 이미 본 값
    } else {
        seen.add(x);
    }
}
```

## 4. Logic Trace (Step-by-Step)
| Step | Operation | State | Explanation |
| :--- | :--- | :--- | :--- |
| 1 | Init | empty set | 생성 |
| 2 | Add | 값 추가 | 중복 제거 |
| 3 | Check | contains | 존재 판단 |
| End | Output | size/조건 | 결과 |

## 5. Pitfalls & Tips
* **순서 없음:** HashSet은 순서 보장하지 않음
* **객체 사용 시:** `equals()` / `hashCode()` 구현 필수
* **Trick:** 값의 존재 여부만 필요하면 HashMap 대신 HashSet