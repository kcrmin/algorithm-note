---
cssclasses: [algorithm-reference-sample]
code-styler-ignore: true
type: template
category: binary-search
categories: [binary-search]
---

# Binary Search

![First True와 Last True의 경계](../attachments/binary-search-boundaries.svg)

> [!reference] 개념과 사용 조건
> **단조** · 판정이 한 방향으로 최대 한 번만 바뀐다.
>
> **Parametric Search** · 답 후보의 가능 여부로 최솟값·최댓값을 찾을 때 사용.
>
> **`valid(x)`** · 후보 x가 조건을 만족하는지 판정하는 문제별 함수.
>
> **탐색 범위** · 정수 후보 `0..max`, `0 ≤ max < Long.MAX_VALUE`.

---

## First True · FFF│TTT

시작 조건: <code>valid(<mark class="algorithm-change">max</mark>) == true</code>. F라면 이 범위에 답이 없다.

**Structure**

<pre class="algorithm-code"><code><span class="algorithm-keyword">while</span> (left &lt; right) {
    <span class="algorithm-comment">// mid 계산</span>

    <span class="algorithm-keyword">if</span> (mid가 가능하면) {
        <span class="algorithm-comment">// <mark class="algorithm-change">right를 mid로</mark></span>
    } <span class="algorithm-keyword">else</span> {
        <span class="algorithm-comment">// <mark class="algorithm-change">left를 mid 다음으로</mark></span>
    }
}

<span class="algorithm-keyword">return</span> left;</code></pre>

**Java**

<pre class="algorithm-code"><code><span class="algorithm-keyword">static long</span> firstTrue(<span class="algorithm-keyword">long</span> max) {
    <span class="algorithm-keyword">long</span> left = 0, right = max;

    <span class="algorithm-keyword">while</span> (left &lt; right) {
        <span class="algorithm-keyword">long</span> mid = left + (right - left) / 2;    <span class="algorithm-comment">// 합의 오버플로 방지</span>

        <span class="algorithm-keyword">if</span> (valid(mid)) {
            <mark class="algorithm-change">right</mark> = mid;
        } <span class="algorithm-keyword">else</span> {
            <mark class="algorithm-change">left</mark> = mid <mark class="algorithm-change">+ 1</mark>;    <span class="algorithm-comment">// +1 없으면 두 후보에서 반복</span>
        }
    }

    <span class="algorithm-keyword">return</span> left;
}</code></pre>

---

## Last True · TTT│FFF

시작 조건: <code>valid(<mark class="algorithm-change">0</mark>) == true</code>. F라면 이 범위에 답이 없다.

**Structure**

<pre class="algorithm-code"><code><span class="algorithm-keyword">while</span> (left &lt; right) {
    <span class="algorithm-comment">// mid 계산</span>

    <span class="algorithm-keyword">if</span> (mid가 가능하면) {
        <span class="algorithm-comment">// <mark class="algorithm-change">left를 mid로</mark></span>
    } <span class="algorithm-keyword">else</span> {
        <span class="algorithm-comment">// <mark class="algorithm-change">right를 mid 이전으로</mark></span>
    }
}

<span class="algorithm-keyword">return</span> left;</code></pre>

**Java**

<pre class="algorithm-code"><code><span class="algorithm-keyword">static long</span> lastTrue(<span class="algorithm-keyword">long</span> max) {
    <span class="algorithm-keyword">long</span> left = 0, right = max;

    <span class="algorithm-keyword">while</span> (left &lt; right) {
        <span class="algorithm-keyword">long</span> mid = left + (right - left <mark class="algorithm-change">+ 1</mark>) / 2;    <span class="algorithm-comment">// 무한 반복 방지</span>

        <span class="algorithm-keyword">if</span> (valid(mid)) {
            <mark class="algorithm-change">left</mark> = mid;
        } <span class="algorithm-keyword">else</span> {
            <mark class="algorithm-change">right</mark> = mid <mark class="algorithm-change">- 1</mark>;
        }
    }

    <span class="algorithm-keyword">return</span> left;
}</code></pre>

#search
