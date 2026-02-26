---
deprecated: true
note: "Inbox 정리용 노트 (deprecated)"
---

# Java Type Conversion
> **한 줄 요약:** 알고리즘 문제 풀이에서 가장 자주 쓰는 Java 형변환 패턴

## 1. Core Concept
* **When to use:**  
* **Key Point:**
    * 입력은 거의 항상 `String`
    * 계산은 `int / long`
    * 출력은 다시 `String`
    * 문자 기반 문제는 **ASCII 연산이 가장 빠름**
    * 형변환은 “방향”을 먼저 생각하면 실수 줄어듦
      ```
      String → Number → String
      char   ↔ Number
      ```
* **Time Complexity:** O(N)

## 2. API & State Change
| Operation | Code Snippet | State / Return | 설명 |
| :--- | :--- | :--- | :--- |
| String → int | `Integer.parseInt(s)` | int | 가장 기본 |
| String → long | `Long.parseLong(s)` | long | 범위 클 때 |
| int → String | `String.valueOf(n)` | String | 출력용 |
| char → int | `c - '0'` | int | 숫자 문자 |
| int → char | `(char)('0' + n)` | char | 한 자리 숫자 |

## 3. Code Templates (Patterns)

### Pattern A: String → Number
설명: 입력 문자열을 숫자로 변환하는 기본 패턴
```java
int n = Integer.parseInt(s);
long l = Long.parseLong(s);
```

### Pattern B: char ↔ int (ASCII)
설명: 문자 기반 문제에서 가장 많이 쓰는 패턴
```java
int x = c - '0';            // '7' -> 7
char c2 = (char)('0' + x);  // 7 -> '7'
```

### Pattern C: String → int[]
설명: 숫자 문자열을 각 자리 숫자로 분리
```java
String s = "12345";
int[] arr = new int[s.length()];

for (int i = 0; i < s.length(); i++) {
    arr[i] = s.charAt(i) - '0';
}
```

### Pattern D: Base Conversion
설명: 진법 변환 관련 문제
```java
String bin = Integer.toBinaryString(n);
int dec = Integer.parseInt(bin, 2);
String hex = Integer.toString(n, 16);
```

## 4. Pitfalls & Tips
* **범위 주의:** `int` 오버플로우 나면 바로 `long`
* **char 변환:** `(char)('0' + n)`은 `0~9`만 가능
* **성능:** 반복 `parseInt`는 비용 큼 → char 연산 고려
* **생각법:**  
  > “지금 이 값은 문자열인가, 숫자인가, 문자인가?”