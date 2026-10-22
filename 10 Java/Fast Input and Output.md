---
type: template
tags: ["Java"]
---

# Fast Input and Output

**언제** 입력을 많이 읽고 출력할 때. 아래 예시는 한 줄에 정수 두 개.

```text
입력 "12 7" → readLine → "12 7"
                      → nextToken → "12", "7"
                      → parseInt  → 12, 7 → 출력 19
```

**코드** `java.io.*`, `java.util.*` 사용. `main ... throws Exception` 내부.

```java
BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
StringTokenizer st = new StringTokenizer(br.readLine());
int a = Integer.parseInt(st.nextToken());
int b = Integer.parseInt(st.nextToken());

StringBuilder out = new StringBuilder();
out.append((long) a + b).append('\n');
System.out.print(out);
```

| 암기 포인트 | 이유 |
|---|---|
| `readLine → StringTokenizer` | 읽는 단위는 한 줄, 꺼내는 단위는 공백으로 나눈 값이다. |
| `StringBuilder → print` | 출력을 쌓아 한 번 보내면 반복 출력 비용을 줄인다. |
| `(long) a + b` | 덧셈 전에 long으로 바꿔야 int 넘침을 막는다. |

**주의** 다음 줄은 br.readLine()으로 읽고 StringTokenizer도 새로 만든다.

[[Java Basics]] · [[Sorting]]
