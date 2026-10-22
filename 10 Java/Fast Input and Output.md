---
type: template
tags: ["Java"]
category: java-essentials
categories: [java-essentials]
---

# Fast Input and Output

```java
BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
StringTokenizer st = new StringTokenizer(br.readLine()); // read: 한 줄 분리; 줄마다 새 Tokenizer
int a = Integer.parseInt(st.nextToken()); // parse: 토큰을 숫자로
int b = Integer.parseInt(st.nextToken());
StringBuilder out = new StringBuilder(); // write: 결과를 모아 한 번에 출력
out.append((long) a + b).append('\n');
System.out.print(out);
```

**조건** 한 줄에 정수 두 개. java.io.*, java.util.*, throws Exception.

**분류** [[00 Maps/Java Essentials|Java Essentials]]
