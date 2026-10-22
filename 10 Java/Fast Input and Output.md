---
type: template
tags: ["Java"]
---

# Fast Input and Output

```java
// java.io.*, java.util.* 필요. main ... throws Exception 내부 예시.
BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
// 한 줄을 읽어 나눠 쓰면 숫자마다 입력을 새로 요청하지 않는다.
StringTokenizer st = new StringTokenizer(br.readLine());
// nextToken은 현재 줄의 다음 값이다. 다음 줄은 Tokenizer도 새로 만든다.
int a = Integer.parseInt(st.nextToken());
int b = Integer.parseInt(st.nextToken());

// 출력을 모아 한 번 보내면 반복 출력 호출 비용을 줄인다.
StringBuilder out = new StringBuilder();
// 덧셈 전 long 변환으로 int 합의 넘침을 막는다.
out.append((long) a + b).append('\n');
System.out.print(out);
```

**예시**

```text
입력 "12 7" → readLine → "12 7"
                      → nextToken → "12", "7"
                      → parseInt  → 12, 7 → 출력 19
```

**주의** 다음 줄은 br.readLine()으로 읽고 StringTokenizer도 새로 만든다.

[[Java Basics]] · [[Sorting]]
