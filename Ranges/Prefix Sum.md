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
> **비용** · 1D 전처리 O(n), 2D 전처리 O(height × width). 질의 O(1).

---

## 1D

누적합을 미리 만들어 배열의 `[left, right)` 구간 합을 구한다.

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

직사각형 `[x1, x2) × [y1, y2)` 안의 합을 구한다.

x는 오른쪽으로 증가하는 열, y는 아래로 증가하는 행이며 `grid[y][x]`로 접근한다. 입력은 각 행의 길이가 같은 배열이다.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-keyword">for</span> (각 행 y) {
    <span class="algorithm-keyword">for</span> (각 열 x) {
        <span class="algorithm-comment">// 위 + 왼쪽 − 겹친 부분 + 현재 값</span>
    }
}

sum(x1, y1, x2, y2) {
    <span class="algorithm-comment">// 전체 − 위 − 왼쪽 + 두 번 뺀 부분</span>
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">long</span>[][] prefixSum2D(<span class="algorithm-keyword">int</span>[][] grid) {
    <span class="algorithm-keyword">int</span> height = grid.length, width = height == 0 ? 0 : grid[0].length;
    <span class="algorithm-keyword">long</span>[][] prefix = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[height + 1][width + 1];

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> y = 0; y &lt; height; y++) {
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> x = 0; x &lt; width; x++) {
            prefix[y + 1][x + 1] = prefix[y][x + 1] + prefix[y + 1][x]
                    - prefix[y][x] + grid[y][x];    <span class="algorithm-comment">// 위·왼쪽에 중복된 영역 제거</span>
        }
    }

    <span class="algorithm-keyword">return</span> prefix;
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">long</span> rectangleSum(<span class="algorithm-keyword">long</span>[][] prefix, <span class="algorithm-keyword">int</span> x1, <span class="algorithm-keyword">int</span> y1, <span class="algorithm-keyword">int</span> x2, <span class="algorithm-keyword">int</span> y2) {
    <span class="algorithm-keyword">return</span> prefix[y2][x2] - prefix[y1][x2]
            - prefix[y2][x1] + prefix[y1][x1];
}</code></pre>

[[Difference Array]] · [[Fenwick Tree]]
