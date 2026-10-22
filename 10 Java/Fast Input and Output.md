---
type: template
tags: ["Java"]
category: java-essentials
categories: [java-essentials]
---

# Fast Input and Output

```text
input         "12 30\n"
                  ↓ readLine
line          "12 30"
                  ↓ tokenize → parse
numbers        12, 30
                  ↓ StringBuilder
output        "42\n" → 한 번에 출력
```

## Steps

| Step | Pseudocode |
| --- | --- |
| **Read** | // 한 줄을 읽고 공백으로 나눈다. |
| **Parse** | // 토큰을 필요한 숫자 타입으로 바꾼다. |
| **Collect** | // 출력할 결과와 줄바꿈을 모은다. |
| **Print** | // 모은 문자열을 한 번에 출력한다. |

## Java

```java
BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
StringTokenizer st = new StringTokenizer(br.readLine());

int a = Integer.parseInt(st.nextToken());
int b = Integer.parseInt(st.nextToken());

StringBuilder out = new StringBuilder();
out.append((long) a + b).append('\n');
System.out.print(out);
```

표준 입력 / 출력 템플릿 · 출력량이 매우 크면 StringBuilder도 적절히 나눠 출력

## Watch points

| 선택 | 이유 / 실수 |
| --- | --- |
| 입력 계약 | 이 코드는 한 줄에 정수 두 개가 있는 입력용이다. 새 줄은 readLine과 새 StringTokenizer로 읽는다. |
| `nextToken()` | 현재 줄의 토큰이 남아 있어야 한다. 임의의 줄바꿈을 허용하는 스캐너와 다르다. |
| 큰 정수 | int를 넘는 입력은 Long.parseLong으로 읽는다. |
| 예외와 import | `java.io.*`, `java.util.*`가 필요하다. 호출 메서드에서 IOException을 처리하거나 throws Exception을 둔다. |

---

**분류** [[00 Maps/Java Essentials|Java Essentials]]
