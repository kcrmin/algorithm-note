---
deprecated: true
note: "Inbox 정리용 노트 (deprecated)"
---

# Java Regex (정규식)
> **한 줄 요약:** 문자열을 “패턴”으로 검사/추출/치환/분리하기 위한 표현식 언어 + Java(Pattern/Matcher) 실전 사용법

## 1. Core Concept
* **When to use:**  
* **Key Point:**
    * 정규식은 **문자열을 직접 비교**하는 대신, **패턴(규칙)**으로 처리한다.
    * Java에서는 보통 `Pattern`(컴파일된 정규식) + `Matcher`(입력에 대한 매칭 상태) 조합을 사용한다.
    * `String.matches()`는 **전체 문자열이 패턴과 완전히 일치**해야 `true` (부분 일치 아님).
    * `split`, `replaceAll`, `replaceFirst`는 **정규식 기반**이라 강력하지만, 반복 호출 시 비용이 커질 수 있다.
    * 이스케이프가 2단계:
      * 정규식 이스케이프: `\d`, `\s` 같은 형태
      * Java 문자열 이스케이프까지 포함하면: `"\d"` 처럼 **백슬래시가 2개** 필요
* **Time Complexity:** O(N) ~ (패턴에 따라 증가 가능)  
  (대부분 입력 길이 N에 선형에 가깝지만, 패턴/입력에 따라 백트래킹으로 느려질 수 있음)

## 2. API & State Change
| Operation | Code Snippet | State / Return | 설명 |
| :--- | :--- | :--- | :--- |
| Full Match | `s.matches(regex)` | boolean | 전체 일치 여부 |
| Split | `s.split(regex)` | String[] | 정규식으로 분리 |
| Replace All | `s.replaceAll(regex, r)` | String | 모든 매치 치환 |
| Replace First | `s.replaceFirst(regex, r)` | String | 첫 매치만 치환 |
| Compile | `Pattern.compile(regex)` | Pattern | 패턴 컴파일(재사용) |
| Matcher | `p.matcher(s)` | Matcher | 입력에 대한 매처 생성 |
| Find | `m.find()` | boolean | 다음 부분 매치 찾기 |
| Group | `m.group()` | String | 이번 매치 문자열 |
| Group(i) | `m.group(i)` | String | 캡처 그룹 i |
| Start/End | `m.start()` / `m.end()` | int | 매치 구간(끝은 미포함) |

## 3. Code Templates (Patterns)

### Pattern A: 가장 흔한 토큰들 (치트시트)
설명: 가장 자주 쓰는 메타 문자/클래스
```java
// 자주 쓰는 “문자 클래스”
String digit = "\\d";     // 숫자 1개 (0-9)
String nonDigit = "\\D";  // 숫자 아닌 것
String space = "\\s";     // 공백(스페이스/탭/개행 등)
String nonSpace = "\\S";  // 공백 아닌 것
String word = "\\w";      // [A-Za-z0-9_]
String nonWord = "\\W";   // 단어문자 아닌 것

// 반복(수량자)
String oneOrMore = "\\d+";      // 1개 이상
String zeroOrMore = "\\d*";     // 0개 이상
String optional = "\\d?";       // 0 또는 1개
String range = "\\d{2,4}";      // 2~4개
String exact = "\\d{3}";        // 정확히 3개

// 경계(Anchors / Boundaries)
String start = "^abc";            // 문자열 시작에서 abc
String end = "abc$";              // 문자열 끝에서 abc
String wordBoundary = "\\bcat\\b"; // 단어 경계 cat
```

### Pattern B: matches는 “전체 일치”
설명: 전체를 검증하고 싶을 때 사용
```java
String s1 = "abc123";
boolean ok1 = s1.matches("[a-z]+\\d+"); // true

String s2 = "xx-12";
boolean ok2 = s2.matches("[a-z]+\\d+"); // false (전체 일치가 아니므로)
```

### Pattern C: 문자 집합(Character Class)
설명: 대괄호 `[]`는 “가능한 문자 중 하나”
```java
String s = "A7z";

// [A-Za-z] : 알파벳 1글자
boolean a = s.matches("[A-Za-z].*"); // true

// [0-9] 또는 \d : 숫자 1글자
boolean b = s.matches(".*[0-9].*");  // true

// 부정: [^...] : 제외
boolean c = s.matches("[^0-9]+");    // false (숫자가 포함됨)
```

### Pattern D: OR, 그룹, 캡처
설명: `()`는 그룹, `|`는 OR, 그룹은 `group(i)`로 꺼냄
```java
Pattern p = Pattern.compile("(cat|dog)-(\\d+)");
Matcher m = p.matcher("dog-42");

if (m.matches()) {
    String animal = m.group(1); // "dog"
    String num = m.group(2);    // "42"
}
```

### Pattern E: 부분 매치 찾기(find)로 여러 토큰 추출
설명: `find()`는 입력에서 “다음 매치”를 계속 탐색
```java
Pattern p = Pattern.compile("\\d+");
Matcher m = p.matcher("a1b22c333");

while (m.find()) {
    String token = m.group(); // "1", "22", "333"
    int l = m.start();        // 시작 인덱스
    int r = m.end();          // 끝 인덱스(미포함)
}
```

### Pattern F: split(regex) - 다중 구분자 / 공백 정리
설명: 여러 구분자 또는 연속 공백 처리
```java
String s1 = "a,b;c|d";
String[] arr1 = s1.split("[,;|]"); // ["a","b","c","d"]

String s2 = "a   b    c";
String[] arr2 = s2.trim().split("\\s+"); // ["a","b","c"]
```

### Pattern G: replaceAll / replaceFirst - 마스킹/정규화
설명: 패턴 치환은 데이터 정리에서 자주 씀
```java
String s = "user=kim, phone=010-1234-5678";

// 숫자 덩어리 마스킹
String masked = s.replaceAll("\\d", "*");
// "user=kim, phone=***-****-****"

String onlyFirst = s.replaceFirst("\\d+", "#");
// 첫 숫자 덩어리만 치환
```

### Pattern H: 대표 검증 패턴 예시 (코테/입력 검증)
설명: “전체 일치”로 유효성 검사
```java
// 정수(부호 옵션)
boolean intOk = "-123".matches("[+-]?\\d+"); // true

// 소문자 알파벳만
boolean low = "abc".matches("[a-z]+"); // true

// 날짜 YYYY-MM-DD (형식만)
boolean date = "2026-02-10".matches("\\d{4}-\\d{2}-\\d{2}"); // true
```

### Pattern I: Java 문자열 이스케이프 감각 잡기
설명: Java 문자열 안에서 백슬래시를 쓰려면 2번
```java
// 정규식: \d+
// Java 문자열: "\d+"
Pattern p = Pattern.compile("\\d+");

// 정규식: \s+
// Java 문자열: "\s+"
String[] parts = "a   b".split("\\s+");
```

### Pattern J: (고급) Lookaround 개념 (필요할 때만)
설명: “앞/뒤 조건”은 포함하되 캡처에 포함하지 않게 할 때 유용
```java
// (?=...) : positive lookahead (뒤에 ...가 와야 함)
// (?!...) : negative lookahead  (뒤에 ...가 오면 안 됨)
// (?<=...) : positive lookbehind (앞에 ...가 있어야 함)
// (?<!...) : negative lookbehind  (앞에 ...가 있으면 안 됨)

// 예시: 숫자 뒤에 '원'이 오는 경우의 숫자만
Pattern p = Pattern.compile("\\d+(?=원)");
Matcher m = p.matcher("100원, 200원");
while (m.find()) {
    // "100", "200"
}
```

## 4. Pitfalls & Tips
* **matches 착각:** `matches`는 “부분 포함”이 아니라 **전체 일치**
* **split/replaceAll 비용:** 정규식 기반이라 반복 호출이 많으면 비용 증가  
  반복 처리라면 `Pattern`을 미리 `compile`해서 재사용하는 편이 좋다.
* **백트래킹 주의:** `(.+)+` 같이 애매한 패턴은 최악의 경우 매우 느려질 수 있다.  
  가능하면 구체적인 패턴(예: `[^,]+`)으로 제한하는 습관이 좋다.
* **그리디 vs 레이지:** `.+`(greedy) vs `.+?`(lazy)  
  필요한 범위만 잡고 싶으면 `?`로 레이지를 고려한다.
* **이스케이프:** Java 문자열 + 정규식 이스케이프가 겹쳐 헷갈리면  
  “정규식에서 백슬래시 1개 → Java 코드에서는 2개”를 먼저 떠올린다.
* **그룹 번호:** 괄호 `()`가 늘어날수록 그룹 번호가 바뀜  
  유지보수 어려우면 비캡처 그룹 `(?:...)`를 고려한다.