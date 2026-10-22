---
tags:
  - java
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Java Essentials

![Java Essentials memory cue](../attachments/reference-java-essentials.svg)

> [!reference] 개념과 사용 조건
> **목적** · 알고리즘을 구현할 때 자주 틀리는 Java 표현만 모은다.
>
> **범위** · 아래 코드는 메서드 내부에서 사용. 배열·문자열의 인덱스는 0-based.
>
> **숫자** · int는 약 ±21억, long은 약 ±9×10¹⁸. 계산 결과까지 범위를 확인.

---

## Numbers

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// 연산 전에 필요한 타입으로 승격</span>
<span class="algorithm-comment">// 음수 나머지가 가능하면 정규화</span>
<span class="algorithm-comment">// 비교는 빼기가 아닌 compare</span></code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">int</span> a = 1_000_000_000, b = 3;
<span class="algorithm-keyword">long</span> product = (<span class="algorithm-keyword">long</span>) a * b;    <span class="algorithm-comment">// int로 곱한 뒤 long에 넣으면 이미 넘친 값</span>

<span class="algorithm-keyword">int</span> mod = 7;
<span class="algorithm-keyword">int</span> normalized = Math.floorMod(-3, mod);
<span class="algorithm-keyword">int</span> order = Integer.compare(a, b);    <span class="algorithm-comment">// a - b는 비교 도중 넘칠 수 있음</span>

<span class="algorithm-keyword">int</span> low = 0, high = 100;
<span class="algorithm-keyword">int</span> mid = low + (high - low) / 2;</code></pre>

---

## Arrays

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// 초기값 채우기</span>
<span class="algorithm-comment">// 1D 복사 / 2D 행별 복사</span></code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">int</span>[] values = {3, 1, 4};
<span class="algorithm-keyword">int</span>[] copy = values.clone();
<span class="algorithm-keyword">int</span>[] distance = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[values.length];
Arrays.fill(distance, -1);

<span class="algorithm-keyword">int</span>[][] grid = {{1, 2}, {3, 4}};
<span class="algorithm-keyword">int</span>[][] copiedGrid = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[grid.length][];
<span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> row = 0; row &lt; grid.length; row++) {
    copiedGrid[row] = grid[row].clone();    <span class="algorithm-comment">// grid.clone()만 하면 각 행을 공유함</span>
}</code></pre>

---

## Strings

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// 내용 비교 / 문자 접근 / 부분 문자열</span>
<span class="algorithm-comment">// 반복 연결은 StringBuilder</span></code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code>String text = "algorithm";
<span class="algorithm-keyword">boolean</span> same = "algorithm".equals(text);
<span class="algorithm-keyword">char</span> first = text.charAt(0);
String part = text.substring(0, 4);    <span class="algorithm-comment">// 끝 인덱스는 포함하지 않음</span>
<span class="algorithm-keyword">char</span>[] letters = text.toCharArray();

StringBuilder output = <span class="algorithm-keyword">new</span> StringBuilder();
output.append(first).append(' ').append(part);
String result = output.toString();</code></pre>
