---
deprecated: true
note: "Inbox 정리용 노트 (deprecated)"
---

# Java String Format
> **한 줄 요약:** 숫자·문자열·정렬·패딩을 한 줄로 표현하는 출력 포맷 표준 정리

## 1. Core Concept
* **When to use:**  
* **Key Point:**
    * `String.format`은 **문자열을 반환**
    * `System.out.printf`는 **즉시 출력**
    * 포맷 문자열 + 값(arguments) 구조
    * 출력 모양이 중요한 문제에서 매우 강력
    * **형식** : `%[flags][width][.precision]conversion`
* **Time Complexity:** O(N)  
  (생성되는 문자열 길이 기준)

## 2. API & State Change
| Operation | Code Snippet | Return | 설명 |
| :--- | :--- | :--- | :--- |
| Format | `String.format(fmt, args)` | String | 포맷 문자열 생성 |
| Print | `System.out.printf(fmt, args)` | void | 포맷 출력 |
| Locale | `String.format(Locale.US, fmt, args)` | String | 로케일 지정 |

### Conversion (필수)
| 타입 | 문자 | 설명 |
| :--- | :--- | :--- |
| String | `%s` | 문자열 |
| char | `%c` | 문자 |
| int | `%d` | 10진수 |
| oct | `%o` | 8진수 |
| hex | `%x` / `%X` | 16진수 |
| float | `%f` | 실수 |
| newline | `%n` | 줄바꿈 |

## 3. Code Templates (Patterns)

### Pattern A: 가장 기본
```java
String s = String.format("%s %d", "count", 3);
// "count 3"
```

### Pattern B: width (폭)와 정렬
```java
int x = 7;

String a = String.format("%3d", x);   // "  7" (폭 3, 오른쪽 정렬)
String b = String.format("%-3d", x);  // "7  " (폭 3, 왼쪽 정렬)
```

### Pattern C: 0 padding
```java
int x = 7;

String a = String.format("%03d", x); // "007"
```

### Pattern D: precision (소수점 / 문자열 자르기)
```java
double pi = 3.14159;

String a = String.format("%.2f", pi); // "3.14"

String s = "algorithm";
String b = String.format("%.4s", s);  // "algo"
```

### Pattern E: width + precision
```java
double pi = 3.14159;

String a = String.format("%6.2f", pi); // "  3.14"
```

### Pattern F: 부호 플래그
```java
int x = 7;

String a = String.format("%+d", x); // "+7"
String b = String.format("% d", x); // " 7"
```

### Pattern G: 진법 출력
```java
int x = 26;

String dec = String.format("%d", x); // "26"
String hex = String.format("%x", x); // "1a"
String HEX = String.format("%X", x); // "1A"
String oct = String.format("%o", x); // "32"
```

### Pattern H: 여러 값 포맷
```java
String name = "Kim";
int id = 7;
double score = 98.5;

String out = String.format("name=%-5s id=%03d score=%.1f",
                           name, id, score);
// "name=Kim   id=007 score=98.5"
```

### Pattern I: 테이블 출력 (코테/로그 단골)
```java
for (int i = 1; i <= 3; i++) {
    System.out.printf("%2d | %-6s | %03d%n", i, "row", i);
}
/*
 1 | row    | 001
 2 | row    | 002
 3 | row    | 003
*/
```

### Pattern J: StringBuilder vs format
```java
// format: 가독성 좋음
String s = String.format("%03d-%s", 7, "ID");

// StringBuilder: 반복 출력에 유리
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 3; i++) {
    sb.append(String.format("%03d ", i));
}
```

## 4. Pitfalls & Tips
* **성능:** 대량 반복 출력에서는 `StringBuilder`가 더 빠름
* **Locale:** 소수점/구분자 문제 생기면 Locale 지정
* **줄바꿈:** `\n` 대신 `%n` 사용 권장
* **가독성:** 출력 형식이 복잡할수록 format이 유리

## 5. Quick Cheat Sheet
```java
%03d   // 3자리, 0패딩
%-5s   // 폭 5, 왼쪽 정렬
%.2f   // 소수점 2자리
%6.2f  // 폭 6, 소수점 2자리
%x     // 16진수
%n     // 줄바꿈
```