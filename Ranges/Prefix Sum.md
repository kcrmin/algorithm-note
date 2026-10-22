---
tags:
  - ranges
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Prefix Sum

![Prefix Sum memory cue](../attachments/reference-prefix-sum.svg)

> [!reference] 개념과 사용 조건
> **목적** · 값이 바뀌지 않는 배열의 구간 합.
>
> **범위** · 모든 구간은 `[left, right)` — 왼쪽 포함, 오른쪽 제외.
>
> **비용** · 1D 전처리 O(n), 2D 전처리 O(rows × cols). 질의 O(1).

---

## 1D

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-keyword">for</span> (각 원소) {
    <span class="algorithm-comment">// 이전 누적값 + 현재 값</span>
}

sum(left, right) {
    <span class="algorithm-keyword">return</span> 끝 누적 - 앞 누적;
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">long</span>[] prefixSum(<span class="algorithm-keyword">int</span>[] a) {
    <span class="algorithm-keyword">long</span>[] prefix = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[a.length + 1];    <span class="algorithm-comment">// prefix[0] = 0으로 시작 경계를 통일</span>

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = 0; i &lt; a.length; i++) {
        prefix[i + 1] = prefix[i] + a[i];
    }

    <span class="algorithm-keyword">return</span> prefix;
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">long</span> rangeSum(<span class="algorithm-keyword">long</span>[] prefix, <span class="algorithm-keyword">int</span> left, <span class="algorithm-keyword">int</span> right) {
    <span class="algorithm-keyword">return</span> prefix[right] - prefix[left];
}</code></pre>

---

## 2D

`[top, bottom) × [left, right)` 직사각형. 입력은 각 행의 길이가 같은 배열.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-keyword">for</span> (각 행) {
    <span class="algorithm-keyword">for</span> (각 열) {
        <span class="algorithm-comment">// 위 + 왼쪽 − 겹친 부분 + 현재 값</span>
    }
}

sum(top, left, bottom, right) {
    <span class="algorithm-comment">// 전체 − 위 − 왼쪽 + 두 번 뺀 부분</span>
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">long</span>[][] prefixSum2D(<span class="algorithm-keyword">int</span>[][] a) {
    <span class="algorithm-keyword">int</span> rows = a.length, cols = rows == 0 ? 0 : a[0].length;
    <span class="algorithm-keyword">long</span>[][] prefix = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[rows + 1][cols + 1];

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> r = 0; r &lt; rows; r++) {
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> c = 0; c &lt; cols; c++) {
            prefix[r + 1][c + 1] = prefix[r][c + 1] + prefix[r + 1][c]
                    - prefix[r][c] + a[r][c];    <span class="algorithm-comment">// 위·왼쪽에 중복된 영역 제거</span>
        }
    }

    <span class="algorithm-keyword">return</span> prefix;
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">long</span> rectangleSum(<span class="algorithm-keyword">long</span>[][] prefix, <span class="algorithm-keyword">int</span> top, <span class="algorithm-keyword">int</span> left, <span class="algorithm-keyword">int</span> bottom, <span class="algorithm-keyword">int</span> right) {
    <span class="algorithm-keyword">return</span> prefix[bottom][right] - prefix[top][right]
            - prefix[bottom][left] + prefix[top][left];
}</code></pre>

[[Difference Array]] · [[Fenwick Tree]]
