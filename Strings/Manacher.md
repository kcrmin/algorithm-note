---
tags:
  - strings
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Manacher

![Manacher memory cue](../attachments/reference-manacher.svg)

> [!reference] 개념과 사용 조건
> **목적** · 모든 중심의 최대 팰린드롬 반지름을 O(n)에 구한다.
>
> **Odd** · 중심은 문자 i. 반지름에 중심 포함. 길이 = 2 × radius[i] − 1.
>
> **Even** · 중심은 i−1과 i 사이. 반지름은 한쪽 길이. 길이 = 2 × radius[i].

---

## Odd

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-keyword">for</span> (각 중심 i) {
    <span class="algorithm-comment">// 오른쪽 경계 안이면 대칭 중심에서 반지름 복사</span>
    <span class="algorithm-keyword">while</span> (양끝이 같으면) { <span class="algorithm-comment">/* 반지름 확장 */</span> }
    <span class="algorithm-comment">// 반지름 저장</span>
    <span class="algorithm-keyword">if</span> (오른쪽 경계를 넓혔으면) { <span class="algorithm-comment">/* left, right 갱신 */</span> }
}
<span class="algorithm-keyword">return</span> radius;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span>[] oddRadius(String s) {
    <span class="algorithm-keyword">int</span> n = s.length();
    <span class="algorithm-keyword">int</span>[] radius = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[n];
    <span class="algorithm-keyword">int</span> left = 0, right = -1;

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = 0; i &lt; n; i++) {
        <span class="algorithm-keyword">int</span> k = i &gt; right ? 1 : Math.min(<mark class="algorithm-change">radius[left + right - i]</mark>, right - i + 1);
        <span class="algorithm-keyword">while</span> (i - k &gt;= 0 &amp;&amp; i + k &lt; n &amp;&amp; s.charAt(i - k) == s.charAt(i + k)) {
            k++;
        }
        radius[i] = k;

        <span class="algorithm-keyword">if</span> (i + k - 1 &gt; right) {
            left = <mark class="algorithm-change">i - k + 1</mark>;
            right = i + k - 1;    <span class="algorithm-comment">// 확인한 오른쪽 경계까지만 다음 중심에 재사용</span>
        }
    }
    <span class="algorithm-keyword">return</span> radius;
}</code></pre>

---

## Even

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-keyword">for</span> (문자 사이의 각 중심) {
    <span class="algorithm-comment">// 대칭 중심에서 반지름 복사, 최소 반지름은 0</span>
    <span class="algorithm-keyword">while</span> (왼쪽 i−k−1과 오른쪽 i+k가 같으면) { <span class="algorithm-comment">/* 확장 */</span> }
    <span class="algorithm-comment">// 반지름 저장</span>
    <span class="algorithm-keyword">if</span> (오른쪽 경계를 넓혔으면) { <span class="algorithm-comment">/* left, right 갱신 */</span> }
}
<span class="algorithm-keyword">return</span> radius;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span>[] evenRadius(String s) {
    <span class="algorithm-keyword">int</span> n = s.length();
    <span class="algorithm-keyword">int</span>[] radius = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[n];
    <span class="algorithm-keyword">int</span> left = 0, right = -1;

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = 0; i &lt; n; i++) {
        <span class="algorithm-keyword">int</span> k = i &gt; right ? 0 : Math.min(<mark class="algorithm-change">radius[left + right - i + 1]</mark>, right - i + 1);
        <span class="algorithm-keyword">while</span> (<mark class="algorithm-change">i - k - 1</mark> &gt;= 0 &amp;&amp; i + k &lt; n &amp;&amp; s.charAt(<mark class="algorithm-change">i - k - 1</mark>) == s.charAt(i + k)) {
            k++;
        }
        radius[i] = k;

        <span class="algorithm-keyword">if</span> (i + k - 1 &gt; right) {
            <mark class="algorithm-change">left = i - k;</mark>    <span class="algorithm-comment">// 중심이 문자 사이여서 Odd와 왼쪽 경계가 다름</span>
            right = i + k - 1;
        }
    }
    <span class="algorithm-keyword">return</span> radius;
}</code></pre>
