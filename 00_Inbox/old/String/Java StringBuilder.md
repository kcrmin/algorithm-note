---
deprecated: true
note: "Inbox 정리용 노트 (deprecated)"
---

# Java StringBuilder
> **한 줄 요약:** 문자열을 효율적으로 생성·수정하기 위한 가변 객체

## 1. Core Concept
* **When to use:**  
* **Key Point:**
    * mutable(가변 객체)
    * 반복 append에 최적
* **Time Complexity:** O(N)

## 2. API & State Change
| Operation | Code Snippet | 설명 |
| :--- | :--- | :--- |
| Init | `new StringBuilder()` | 생성 |
| Append | `sb.append(x)` | 뒤에 추가 |
| Insert | `sb.insert(i,x)` | 중간 삽입 |
| Delete | `sb.delete(a,b)` | [a,b) 범위 삭제 |
| Delete Char | `sb.deleteCharAt(i)` | 인덱스 i 문자 삭제 |
| Replace | `sb.replace(a,b,x)` | [a,b) 범위를 x로 교체 |
| Reverse | `sb.reverse()` | 반전 |
| Build | `sb.toString()` | String 변환 |

## 3. Code Templates (Patterns)

### Pattern A: 출력 누적
```java
StringBuilder sb = new StringBuilder();

for (int i = 1; i <= 3; i++) {
    sb.append(i).append(" ");
}
// sb = "1 2 3 "
String result = sb.toString();
```

### Pattern B: 중간 수정 (insert + delete 계열 자세히)
```java
StringBuilder sb = new StringBuilder("abc123");

sb.insert(3, "-");           // "abc-123"

// delete(a,b): [a,b) 범위를 삭제
sb.delete(0, 2);             // "c-123"   (0~1 삭제)

// deleteCharAt(i): 한 글자 삭제
sb.deleteCharAt(1);          // "c123"    (인덱스 1 위치 문자 삭제)

// 특정 구간만 날리기 예시
StringBuilder sb2 = new StringBuilder("hello_world");
sb2.delete(5, 6);            // "helloworld" ('_' 제거)

// 뒤쪽 N글자 삭제 예시
StringBuilder sb3 = new StringBuilder("abcdef"); // "abcdef"
sb3.delete(sb3.length() - 2, sb3.length());      // "abcd" (마지막 2글자 삭제)
```

### Pattern C: 뒤집기
```java
StringBuilder sb = new StringBuilder("abcd");
sb.reverse(); // "dcba"
```

## 4. Pitfalls & Tips
* 최종 출력 직전에만 `toString()`
* 멀티스레드 환경에서는 `StringBuffer` 고려
* `delete(a,b)`는 b 미포함 규칙을 항상 기억