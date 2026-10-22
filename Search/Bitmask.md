---
tags:
  - search
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Bitmask

![Bitmask memory cue](../attachments/reference-bitmask.svg)

> [!reference] 개념과 사용 조건
> **목적** · 작은 집합을 정수 하나로 표현.
>
> **조건** · int 비트 연산은 원소 번호 `0..29`. 전체 부분집합 생성은 여기서 `n ≤ 20`.
>
> **구분** · 비트마스크는 상태 표현. 그 상태 사이 최적값을 전달하면 Bitmask DP.

---

## Set Operations

**Structure**

<pre class="algorithm-code" data-language="text"><code>add(mask, i)      <span class="algorithm-comment">// i번 비트를 1로</span>
remove(mask, i)   <span class="algorithm-comment">// i번 비트만 0으로</span>
contains(mask, i) <span class="algorithm-comment">// i번 비트가 1인지</span>
count(mask)      <span class="algorithm-comment">// 포함된 원소 수</span></code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span> add(<span class="algorithm-keyword">int</span> mask, <span class="algorithm-keyword">int</span> i) {
    <span class="algorithm-keyword">return</span> mask | (1 &lt;&lt; i);
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span> remove(<span class="algorithm-keyword">int</span> mask, <span class="algorithm-keyword">int</span> i) {
    <span class="algorithm-keyword">return</span> mask &amp; ~(1 &lt;&lt; i);    <span class="algorithm-comment">// XOR은 없는 원소를 오히려 추가함</span>
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">boolean</span> contains(<span class="algorithm-keyword">int</span> mask, <span class="algorithm-keyword">int</span> i) {
    <span class="algorithm-keyword">return</span> (mask &amp; (1 &lt;&lt; i)) != 0;
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span> count(<span class="algorithm-keyword">int</span> mask) {
    <span class="algorithm-keyword">return</span> Integer.bitCount(mask);
}</code></pre>

---

## All Subsets

`sums[mask]`는 그 집합의 합. 빈 집합 `sums[0] = 0`. 시간·공간 O(2ⁿ).

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-keyword">for</span> (빈 집합 다음 mask부터) {
    <span class="algorithm-comment">// 가장 낮은 비트 하나의 위치 확인</span>
    <span class="algorithm-comment">// 그 비트를 뺀 집합의 합에 해당 원소 추가</span>
}
<span class="algorithm-keyword">return</span> sums;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">long</span>[] subsetSums(<span class="algorithm-keyword">int</span>[] a) {
    <span class="algorithm-keyword">if</span> (a.length &gt; 20) <span class="algorithm-keyword">throw</span> <span class="algorithm-keyword">new</span> IllegalArgumentException();
    <span class="algorithm-keyword">long</span>[] sums = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[1 &lt;&lt; a.length];

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> mask = 1; mask &lt; sums.length; mask++) {
        <span class="algorithm-keyword">int</span> bit = Integer.numberOfTrailingZeros(mask);
        sums[mask] = sums[mask &amp; (mask - 1)] + a[bit];    <span class="algorithm-comment">// 가장 낮은 1비트를 제거한 상태는 이미 계산됨</span>
    }
    <span class="algorithm-keyword">return</span> sums;
}</code></pre>

---

## Submasks

0과 mask 자신을 포함. 시간·출력 공간 O(2ᵏ), k는 mask의 1비트 수.

**Structure**

<pre class="algorithm-code" data-language="text"><code>sub = mask;
<span class="algorithm-keyword">while</span> (<span class="algorithm-keyword">true</span>) {
    <span class="algorithm-comment">// 현재 sub 저장</span>
    <span class="algorithm-keyword">if</span> (sub == 0) <span class="algorithm-keyword">break</span>;
    <span class="algorithm-comment">// mask에 포함된 다음 부분집합으로</span>
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> List&lt;Integer&gt; submasks(<span class="algorithm-keyword">int</span> mask) {
    List&lt;Integer&gt; result = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;();
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> sub = mask; ; sub = (sub - 1) &amp; mask) {
        result.add(sub);
        <span class="algorithm-keyword">if</span> (sub == 0) <span class="algorithm-keyword">break</span>;    <span class="algorithm-comment">// 0에서 계속하면 다시 mask로 돌아감</span>
    }
    <span class="algorithm-keyword">return</span> result;
}</code></pre>

[[Bitmask DP]] · [[Meet in the Middle]]
