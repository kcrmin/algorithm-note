---
deprecated: true
note: "Inbox 정리용 노트 (deprecated)"
---

# Java Scanner
> **한 줄 요약:** Java에서 가장 직관적인 입력 방식이지만, 대용량 입력에는 부적합한 도구

## 1. Core Concept
* **When to use:**  
* **Key Point:**
    * 공백 단위 / 타입 단위 입력을 자동 처리
    * 내부적으로 정규식 파싱 → 속도가 느림
* **Time Complexity:** 입력량이 많을수록 매우 느려짐

## 2. API & State Change
| Operation | Code Snippet | State / Return | 설명 |
|---|---|---|---|
| Init | `Scanner sc = new Scanner(System.in);` | 입력 스트림 연결 | Scanner 생성 |
| Read String | `sc.next()` | String | 공백 기준 |
| Read Line | `sc.nextLine()` | String | 한 줄 전체 |
| Read Int | `sc.nextInt()` | int | 정수 파싱 |

## 3. Code Templates

### Standard
```java
Scanner sc = new Scanner(System.in);
int n = sc.nextInt();
String s = sc.next();
```

### Line + Token 혼합
```java
int n = sc.nextInt();
sc.nextLine();
String line = sc.nextLine();
```

## 4. Pitfalls
* nextInt 후 nextLine 사용 시 개행 주의
* 대용량 입력에서 시간 초과 가능