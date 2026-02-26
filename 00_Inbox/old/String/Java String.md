---
deprecated: true
note: "Inbox 정리용 노트 (deprecated)"
---

# Java String
> **한 줄 요약:** 코딩테스트에서 가장 자주 쓰는 String API와 “바로 가져다 쓰는” 패턴 모음

## 1. Core Concept
* **When to use:**  
* **Key Point:**
    * `String`은 immutable(불변 객체)라서 변경 연산은 항상 **새 String 반환**
    * 값 비교는 `equals`, 대소문자 무시는 `equalsIgnoreCase`
    * 검색은 `indexOf / lastIndexOf / contains / startsWith / endsWith`
    * 부분 문자열은 `substring`
    * 문자 단위 처리는 `charAt`, `toCharArray`
* **Time Complexity:** O(N)  
  (대부분 문자열 길이 기준)

## 2. API & State Change
| Operation | Code Snippet | Return | 설명 |
| :--- | :--- | :--- | :--- |
| Init | `String s = "abc";` | String | 문자열 생성 |
| Compare | `s.equals(t)` | boolean | 값 비교 |
| Compare Ignore Case | `s.equalsIgnoreCase(t)` | boolean | 대소문자 무시 |
| Length | `s.length()` | int | 길이 |
| Access | `s.charAt(i)` | char | 문자 접근 |
| Substring | `s.substring(a,b)` | String | 부분 문자열 (a 포함, b 미포함) |
| Search | `s.indexOf(x)` | int | 첫 위치, 없으면 -1 |
| Search (from) | `s.indexOf(x, from)` | int | from부터 검색 |
| Search Last | `s.lastIndexOf(x)` | int | 마지막 위치 |
| Contains | `s.contains(x)` | boolean | 포함 여부 |
| Prefix | `s.startsWith(p)` | boolean | 접두사 |
| Suffix | `s.endsWith(p)` | boolean | 접미사 |
| Split | `s.split(" ")` | String[] | 분리(정규식) |
| Join | `String.join(d, arr)` | String | 결합 |
| Replace | `s.replace(a,b)` | String | 단순 치환 |
| Trim | `s.trim()` | String | 양끝 공백 제거 |

## 3. Code Templates (Patterns)

### Pattern A: 비교 (equals / ignoreCase)
```java
String a = "Hello";
String b = "hello";

boolean x = a.equals(b);            // false
boolean y = a.equalsIgnoreCase(b);  // true
```

### Pattern B: substring (인덱스 규칙 포함)
```java
String s = "algorithm";

String p1 = s.substring(0, 3); // "alg" (0 포함, 3 미포함)
String p2 = s.substring(4);    // "rithm" (4부터 끝까지)
```

### Pattern C: 검색 (indexOf / lastIndexOf / contains)
```java
String s = "bananas";

int i1 = s.indexOf("na");      // 2
int i2 = s.indexOf("na", 3);   // 4  (from=3부터 검색)
int i3 = s.lastIndexOf("na");  // 4
boolean has = s.contains("ana"); // true
```

### Pattern D: 접두/접미 검사 (startsWith / endsWith)
```java
String s = "file.txt";

boolean a = s.startsWith("fi"); // true
boolean b = s.endsWith(".txt"); // true
```

### Pattern E: split + join (파싱/재조합)
```java
String s = "a b c d";
String[] arr = s.split(" ");   // ["a","b","c","d"]

String out = String.join("-", arr); // "a-b-c-d"
```

### Pattern F: char 기반 처리 (숫자 문자열 -> 합)
```java
String s = "12345";
int sum = 0;

for (int k = 0; k < s.length(); k++) {
    sum += s.charAt(k) - '0';  // '1'->1
}
// sum = 15
```

### Pattern G: toCharArray로 in-place 처리 후 재생성
```java
String s = "aabca";
char[] ch = s.toCharArray();   // ['a','a','b','c','a']

for (int i = 0; i < ch.length; i++) {
    if (ch[i] == 'a') ch[i] = 'z';
}
// ch = ['z','z','b','c','z']

String result = new String(ch); // "zzbcz"
```

### Pattern H: trim + 빈 문자열 체크
```java
String s = "   ";
String t = s.trim();       // ""

boolean empty1 = t.isEmpty();     // true
boolean empty2 = t.length() == 0; // true
```

## 4. Pitfalls & Tips
* **Immutability:** `replace/substring/trim` 등은 원본을 바꾸지 않고 새 String을 반환
* **Index Rule:** `substring(a,b)`는 a 포함, b 미포함
* **Split 주의:** `split`은 정규식 기반이라 반복 호출이 많으면 비용이 큼
* **Performance:** 반복 수정/누적 출력은 `StringBuilder` 사용