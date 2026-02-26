---
deprecated: true
note: "Inbox 정리용 노트 (deprecated)"
---

# Java BufferedReader
> **한 줄 요약:** 알고리즘 문제 풀이용 고속 입력 표준

## 1. Core Concept
* **When to use:**  
* **Key Point:**
    * `readLine()` + `StringTokenizer` 조합이 사실상 표준
    * `BufferedReader`는 **입력(라인 생성)** 담당
    * `StringTokenizer`는 **파싱(토큰 분리)** 담당
    * 두 역할을 분리해서 생각하면 헷갈리지 않음
* **Time Complexity:** O(N)

## 2. API & State Change
### BufferedReader (입력 담당)
| Operation | Code Snippet | 설명 |
|---|---|---|
| Init | `new BufferedReader(new InputStreamReader(System.in))` | 입력 스트림 생성 |
| Read | `readLine()` | 한 줄 전체 문자열 반환 |
| EOF | `null` | 더 이상 입력이 없을 때 |

### StringTokenizer (파싱 담당)
| Operation | Code Snippet | 설명 |
|---|---|---|
| Init | `new StringTokenizer(line)` | 문자열을 토큰 단위로 분리 |
| Next | `nextToken()` | 다음 토큰 반환 |
| Check | `hasMoreTokens()` | 남은 토큰 존재 여부 |

> **입력 흐름**
> `BufferedReader` → String(line)  
> `StringTokenizer` → Token 단위 처리

## 3. Code Templates

### Standard (String / char / int)
```java
BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
StringTokenizer st = new StringTokenizer(br.readLine());

String s = st.nextToken();                // 문자열
char c = st.nextToken().charAt(0);        // 문자
int n = Integer.parseInt(st.nextToken()); // 정수
```

### Matrix
```java
int n = Integer.parseInt(br.readLine());

for (int y = 0; y < n; y++) {
    StringTokenizer st = new StringTokenizer(br.readLine());
    for (int x = 0; x < n; x++) {
        matrix[y][x] = Integer.parseInt(st.nextToken());
    }
}
```

### Custom Delimiter
```java
StringTokenizer st = new StringTokenizer(line, ",:");
```

## 4. Pitfalls
* 토큰 없을 때 `nextToken()` 호출 금지
* 토큰 개수 불확실하면 `hasMoreTokens()` 체크
* `readLine()`이 `null`일 수 있음 (EOF)
* `StringTokenizer`는 정규식 지원 안 함
* `BufferedReader` 사용 시 `throws IOException` 필요