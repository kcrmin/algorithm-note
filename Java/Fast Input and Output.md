---
tags:
  - java
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Fast Input and Output

![Fast Input and Output memory cue](../attachments/reference-fast-input-and-output.svg)

> [!reference] 개념과 사용 조건
> **목적** · 숫자·토큰 입력이 많을 때 바이트 버퍼 사용. 공백을 포함한 문장은 readLine.
>
> **조건** · FastScanner는 ASCII 토큰 입력. 정수는 Java long 범위, nextInt는 int 범위.
>
> **반환** · next()는 EOF에서 null, 숫자 입력은 EOFException. 출력은 StringBuilder로 누적.

---

## FastScanner

공백으로 구분된 정수와 문자열을 버퍼로 빠르게 읽는다.

필요한 import: `java.io.*`. 입력은 유효한 정수 토큰이라고 가정한다.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-keyword">class</span> FastScanner {
    read()       <span class="algorithm-comment">// 버퍼가 비면 다시 채움</span>
    next()       <span class="algorithm-comment">// 공백을 건너뛰고 토큰 읽기</span>
    nextLong()   <span class="algorithm-comment">// 부호 처리 후 자릿수 누적</span>
    nextInt()    <span class="algorithm-comment">// long 결과를 int 범위로 변환</span>
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> FastScanner {
    <span class="algorithm-keyword">final</span> InputStream in;
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">byte</span>[] buffer = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">byte</span>[1 &lt;&lt; 16];
    <span class="algorithm-keyword">int</span> pos, size;

    FastScanner(InputStream in) {
        <span class="algorithm-keyword">this</span>.in = in;
    }

    <span class="algorithm-keyword">int</span> read() <span class="algorithm-keyword">throws</span> IOException {
        <span class="algorithm-keyword">if</span> (pos &gt;= size) {
            size = in.read(buffer);
            pos = 0;
            <span class="algorithm-keyword">if</span> (size &lt;= 0) <span class="algorithm-keyword">return</span> -1;
        }
        <span class="algorithm-keyword">return</span> buffer[pos++] &amp; 0xff;
    }

    String next() <span class="algorithm-keyword">throws</span> IOException {
        <span class="algorithm-keyword">int</span> c;
        <span class="algorithm-keyword">do</span> { c = read(); } <span class="algorithm-keyword">while</span> (c != -1 &amp;&amp; c &lt;= ' ');
        <span class="algorithm-keyword">if</span> (c == -1) <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">null</span>;
        StringBuilder value = <span class="algorithm-keyword">new</span> StringBuilder();
        <span class="algorithm-keyword">while</span> (c &gt; ' ') {
            value.append((<span class="algorithm-keyword">char</span>) c);
            c = read();
        }
        <span class="algorithm-keyword">return</span> value.toString();
    }

    <span class="algorithm-keyword">long</span> nextLong() <span class="algorithm-keyword">throws</span> IOException {
        <span class="algorithm-keyword">int</span> c;
        <span class="algorithm-keyword">do</span> { c = read(); } <span class="algorithm-keyword">while</span> (c != -1 &amp;&amp; c &lt;= ' ');
        <span class="algorithm-keyword">if</span> (c == -1) <span class="algorithm-keyword">throw</span> <span class="algorithm-keyword">new</span> EOFException();
        <span class="algorithm-keyword">boolean</span> negative = c == '-';
        <span class="algorithm-keyword">if</span> (c == '-' || c == '+') c = read();
        <span class="algorithm-keyword">long</span> value = 0;
        <span class="algorithm-keyword">while</span> (c &gt; ' ') {
            value = value * 10 - (c - '0');    <span class="algorithm-comment">// 음수로 누적하면 Long.MIN_VALUE도 표현 가능</span>
            c = read();
        }
        <span class="algorithm-keyword">return</span> negative ? value : -value;
    }

    <span class="algorithm-keyword">int</span> nextInt() <span class="algorithm-keyword">throws</span> IOException {
        <span class="algorithm-keyword">return</span> Math.toIntExact(nextLong());
    }
}</code></pre>

---

## Use

정수 n개를 읽어 한 줄에 하나씩 모아서 출력한다.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// scanner 생성 → 필요한 값 읽기</span>
<span class="algorithm-comment">// 결과를 모아서 출력</span></code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code>FastScanner scanner = <span class="algorithm-keyword">new</span> FastScanner(System.in);
<span class="algorithm-keyword">int</span> n = scanner.nextInt();
StringBuilder output = <span class="algorithm-keyword">new</span> StringBuilder();
<span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = 0; i &lt; n; i++) {
    <span class="algorithm-keyword">long</span> value = scanner.nextLong();
    output.append(value).append('\n');
}
System.out.print(output);</code></pre>

---

## Read a Whole Line

공백이 포함된 한 줄을 읽고, 필요하면 토큰으로 나눈다.

한 실행에서는 필요한 입력 방식 하나를 선택한다. 같은 System.in을 여러 버퍼로 동시에 읽지 않는다.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// 줄 전체를 읽고 필요할 때 토큰으로 분리</span></code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code>BufferedReader reader = <span class="algorithm-keyword">new</span> BufferedReader(<span class="algorithm-keyword">new</span> InputStreamReader(System.in));
String line = reader.readLine();
<span class="algorithm-keyword">if</span> (line != <span class="algorithm-keyword">null</span>) {
    StringTokenizer tokens = <span class="algorithm-keyword">new</span> StringTokenizer(line);
    <span class="algorithm-keyword">while</span> (tokens.hasMoreTokens()) {
        String token = tokens.nextToken();
    }
}</code></pre>
