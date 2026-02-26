---
deprecated: true
note: "Inbox 정리용 노트 (deprecated)"
---

# 섹션 0 - Java 기본 입출력 & 형변환 정리

## 1. 입력 기초

### *BufferedReader
| 코드                                                                          |     요약 / 결과     | 예시                                    |
|:----------------------------------------------------------------------------|:---------------:|:--------------------------------------|
| `BufferedReader br = new BufferedReader(new InputStreamReader(System.in));` |  빠른 입력용 기본 세팅   | —                                     |
| `String s = br.readLine();`                                                 | 한 줄 입력 (String) | `"hello world"` → `"hello world"`     |
| `StringTokenizer st = new StringTokenizer(br.readLine());`                  |    공백 단위 분리     | `"hello world"` → `"hello"`,`"world"` |
| `st.nextToken();`                                                           |    다음 문자열 반환    | `"hello"`                             |
| `char c = st.nextToken().charAt(0);`                                        | 다음 문자열에서 문자 반환  | `"world"` → `'w'`                     |
| `int n = Integer.parseInt(st.nextToken());`                                 |      정수 입력      | `"10"` → `10`                         |

### Scanner
| 코드                                          |   요약 / 결과    | 예시                                |
|:--------------------------------------------|:------------:|:----------------------------------|
| `Scanner scanner = new Scanner(System.in);` |   간단한 입력용    | —                                 |
| `scanner.nextLine();`                       |  한 줄 전체 입력   | `"hello world"` → `"hello world"` |
| `scanner.next();`                           | 공백 단위 문자열 입력 | `"hello world"` → `"hello"`       |
| `scanner.next().charAt(0);`                 |    문자 입력     | `"c"` → `'c'`                     |
| `scanner.nextInt();`                        |    정수 입력     | `"10"` → `10`                     |

## 2. *형변환

### String
| 코드                                |    요약 / 결과     | 예시                 |
|:----------------------------------|:--------------:|:-------------------|
| `String.valueOf(문자)`              |    문자 → 문자열    | `'A' → "A"`        |
| `String.valueOf(123)`             |    숫자 → 문자열    | `123 → "123"`      |
| `new String(new char[]{'A','B'})` |   문자배열 → 문자열   | `['A','B'] → "AB"` |

### char
| 코드                    |  요약 / 결과   | 예시                      |
|:----------------------|:----------:|:------------------------|
| `"A".charAt(0)`       |  문자열 → 문자  | `"A" → 'A'`             |
| `(char)97`            |  숫자 → 문자   | `97 → 'a'`              |
| `(char)(7 + '0')`     |  숫자 → 문자   | `7 → '7'`               |
| `"ABC".toCharArray()` | 문자열 → 문자배열 | `"ABC" → ['A','B','C']` |

### int
| 코드                       |    요약 / 결과     | 예시          |
|:-------------------------|:--------------:|:------------|
| `Integer.parseInt("10")` |    문자열 → 숫자    | `"10" → 10` |
| `'7' - '0'`              |  문자(숫자형) → 숫자  | `'7' → 7`   |
| `(int)'A'`               |   문자 → 아스키코드   | `'A' → 65`  |

### 진법 변환
| 코드                            |    요약 / 결과     | 예시            |
|:------------------------------|:--------------:|:--------------|
| `Integer.toBinaryString(10)`  |   10진수 → 2진수   | `10 → "1010"` |
| `Integer.toHexString(255)`    |  10진수 → 16진수   | `255 → "ff"`  |
| `Integer.parseInt("1010", 2)` |   2진수 → 10진수   | `"1010" → 10` |
| `Integer.parseInt("ff", 16)`  |  16진수 → 10진수   | `"ff" → 255`  |
| `Integer.toString(15, 2)`     |   n진수 문자열 변환   | `15 → "1111"` |


## 3. 문자 판별
| 코드                               |     요약 / 결과     | 예시           |
|:---------------------------------|:---------------:|:-------------|
| `Character.isDigit(c)`           |      숫자 여부      | `'3' → true` |
| `Character.isLetter(c)`          |      문자 여부      | `'A' → true` |
| `Character.isUpperCase(c)`       |     대문자 여부      | `'Z' → true` |
| `Character.isLowerCase(c)`       |     소문자 여부      | `'a' → true` |
| `Character.getNumericValue('A')` | 문자 → 숫자 (16진용)  | `'A' → 10`   |
 

## 4. 기본 코드
```java
import java.io.*;
import java.util.*;

public class Main {
    public static void main1(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        // 기본 입력
        String str = scanner.nextLine();
        String word = scanner.next();
        char c = scanner.next().charAt(0);
        int n = scanner.nextInt();
        
        // Matrix 입력
        int[][] board = new int[n][n];
        for (int y = 0; y < n; y++) {
            for (int x = 0; x < n; x++) {
                board[y][x] = scanner.nextInt();
            }
        }
    }
    
    public static void main2(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        
        // 토큰 입력
        String str = br.readLine();
        String word = st.nextToken();
        char c = st.nextToken().charAt(0);
        int n = st.nextToken();
        
        // Matrix 입력
        int[][] board = new int[n][n];
        for (int y = 0; y < n; y++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            for (int x = 0; x < n; x++) {
                board[y][x] = Integer.parseInt(st.nextToken());
            }
        }
    }
}
```


## 5. 주의 포인트
|       코드 / 개념        | 요약                          |
|:--------------------:|:----------------------------|
|  `throws Exception`  | BufferedReader 사용하기 위해 필수   |
|   `br.readLine()`    | 항상 String, 직접 변환 필요         |
|  `StringTokenizezr`  | 매 줄마다 새 객체 생성 필요            |
| `'0'`, `'A'`, `'a'`  | ASCII 기준값 (48, 65, 97)      |
|       `String`       | 불변(Immutable), 수정 시 새 객체 생성 |


## **핵심 요약**
- 입력: `BufferedReader + StringTokenizer` 조합이 표준
- 형변환: `String.valueOf()`, `Integer.parseInt()`, `(char)(n+'0')`
- 진법 변환: `Integer.toBinaryString()`, `Integer.parseInt(s, base)`
- 판별: `Character.isDigit()`, `isLetter()`, `isUpperCase()`
- 문자열은 **불변**, 반복 연결 시 **StringBuilder 사용**