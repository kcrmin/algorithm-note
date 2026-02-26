---
deprecated: true
note: "Inbox 정리용 노트 (deprecated)"
---

# 섹션 1 - String

## 1. *String 기초

### 기본 조작
| 코드                      |      요약 / 결과      | 예시                                     |
|:------------------------|:-----------------:|:---------------------------------------|
| `s.length()`            |      문자열 길이       | `"abc".length() → 3`                   |
| `s.charAt(i)`           |      i번째 문자       | `"abc".charAt(1) → 'b'`                |
| `s.substring(a, b)`     |  a이상 b미만 부분 문자열   | `"abcdef".substring(2,5) → "cde"`      |
| `s.equals(t)`           | 문자열 비교 (대소문자 구분)  | `"abc".equals("ABC") → false`          |
| `s.equalsIgnoreCase(t)` |    대소문자 무시 비교     | `"abc".equalsIgnoreCase("ABC") → true` |
| `s.contains("ab")`      |       포함 여부       | `"abcde".contains("ab") → true`        |
| `s.toLowerCase()`       |      소문자 변환       | `"ABC".toLowerCase() → "abc"`          |
| `s.toUpperCase()`       |      대문자 변환       | `"abc".toUpperCase() → "ABC"`          |

### 분리 / 합치기
| 코드                       |       요약 / 결과        | 예시                                   |
|:-------------------------|:--------------------:|:-------------------------------------|
| `s.split(" ")`           | 공백 기준 분리 (문자열 배열 반환) | `"a b c".split(" ") → ["a","b","c"]` |
| `String.join("", arr)`   |     배열 → 문자열 연결      | `["a","b","c"] → "abc"`              |
| `String.join("-", list)` |     리스트 → 문자열 연결     | `["a","b"] → "a-b"`                  |

### 문자열 수정
| 코드                          |    요약 / 결과     | 예시                                         |
|:----------------------------|:--------------:|:-------------------------------------------|
| `s.replace('a', 'b')`       |     문자 치환      | `"apple".replace('a','b') → "bpple"`       |
| `s.replace("ab", "cd")`     |     문자열 치환     | `"abc".replace("ab","cd") → "cdc"`         |
| `s.replaceAll("[0-9]", "")` |  정규식으로 숫자 제거   | `"a1b2c3".replaceAll("[0-9]", "") → "abc"` |

### *StringBuilder
| 코드                                        |    요약 / 결과     | 예시                      |
|:------------------------------------------|:--------------:|:------------------------|
| `StringBuilder sb = new StringBuilder();` |   가변 문자열 생성    | —                       |
| `sb.append("abc");`                       |     문자열 추가     | `"abc"` 추가              |
| `sb.append(i);`                           |     숫자 추가      | `1 → "1"`               |
| `sb.reverse();`                           |    문자열 뒤집기     | `"abc" → "cba"`         |
| `sb.toString();`                          |   String 변환    | `StringBuilder → "문자열"` |


## 2. 기본 코드
```java
import java.util.*;

public class Main {
    public static void main(String args) {
        StringBuilder sb = new StringBuilder();
        for (int i : solution()) sb.append(i).append(" ");
        System.out.println(sb.toString());
    }
}
```


## 3. 주의 포인트
|       코드 / 개념       | 요약                              |
|:-------------------:|:--------------------------------|
|       문자열 비교        | `==` 대신 `equals()`              |
|        반복 연결        | `StringBuilder` 사용              |
| `substring()` 범위 주의 | `"abc".substring(0,2)` → `"ab"` |


## **핵심 요약**
- 문자열 비교: `equals()` / 대소문자 무시 시 `equalsIgnoreCase()`
- 조작: `substring`, `replace`, `split`, `toCharArray()`
- 연결: 반복 시 `StringBuilder.append()`

## 문제 다시 풀기
### **[Inflearn](../../src/inflearn/section01_string)**
| 분류  | 문제 이름                                                                    | 포인트 메모     |
|:---:|:-------------------------------------------------------------------------|:-----------|
| 기초  | [01-05 특정 문자 뒤집기](../../src/inflearn/section01_string/INF_S01_P05.java)  | 문제 잘 읽기    |
| 감잡기 | [01-10 가장 짧은 문자거리](../../src/inflearn/section01_string/INF_S01_P10.java) | 오버플로우 주의   |
| 감잡기 | [01-11 문자열 압축](../../src/inflearn/section01_string/INF_S01_P11.java)     | 마지막 문자 주의  |
| 응용  | [01-12 암호](../../src/inflearn/section01_string/INF_S01_P12.java)         | radix, 캐스팅 |