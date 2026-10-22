---
type: template
tags: ["Java"]
category: java-essentials
categories: [java-essentials]
---

# Fast Input and Output

```java
BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
// 다음 줄은 Tokenizer도 새로 만든다.
StringTokenizer st = new StringTokenizer(br.readLine());
int a = Integer.parseInt(st.nextToken());
int b = Integer.parseInt(st.nextToken());
StringBuilder out = new StringBuilder(); // 출력을 모아 보내 호출 비용을 줄인다.
out.append((long) a + b).append('\n');
System.out.print(out);
```

**조건** 한 줄에 정수 두 개. java.io.*, java.util.*, throws Exception.

**분류** [[00 Maps/Java Essentials|Java Essentials]]
