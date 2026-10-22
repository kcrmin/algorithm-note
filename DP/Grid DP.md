---
tags:
  - dp
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Grid DP

![Grid DP memory cue](../attachments/reference-grid-dp.svg)

> [!reference] 개념과 사용 조건
> **목적** · 오른쪽·아래로만 이동하는 격자의 경로 수 또는 최소 비용.
>
> **상태** · `dp[r][c]` = 시작점 `(0,0)`에서 `(r,c)`에 도착한 결과.
>
> **조건** · 직사각형 배열. 이동에 사이클이 없으므로 위·왼쪽 결과부터 계산 가능.
>
> **비용** · O(rows × cols) 시간·공간.

---

## Path Count

`blocked[r][c] == true`면 통과 불가. `mod ≥ 2`, 반환은 경로 수를 mod로 나눈 값.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// 막히지 않은 시작 칸의 경로 수 = 1</span>
<span class="algorithm-keyword">for</span> (각 행) {
    <span class="algorithm-keyword">for</span> (각 열) {
        <span class="algorithm-keyword">if</span> (막힌 칸이면) <span class="algorithm-keyword">continue</span>;
        <span class="algorithm-comment">// 위와 왼쪽의 경로 수를 더하기</span>
    }
}
<span class="algorithm-keyword">return</span> 도착 칸의 경로 수;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">long</span> countPaths(<span class="algorithm-keyword">boolean</span>[][] blocked, <span class="algorithm-keyword">int</span> mod) {
    <span class="algorithm-keyword">int</span> rows = blocked.length, cols = rows == 0 ? 0 : blocked[0].length;
    <span class="algorithm-keyword">if</span> (rows == 0 || cols == 0 || blocked[0][0]) {
        <span class="algorithm-keyword">return</span> 0;
    }
    <span class="algorithm-keyword">long</span>[][] dp = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[rows][cols];
    dp[0][0] = 1;    <span class="algorithm-comment">// 출발점에 도달하는 빈 이동 한 가지</span>

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> r = 0; r &lt; rows; r++) {
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> c = 0; c &lt; cols; c++) {
            <span class="algorithm-keyword">if</span> (blocked[r][c]) {
                <span class="algorithm-keyword">continue</span>;
            }
            <span class="algorithm-keyword">if</span> (r &gt; 0) {
                dp[r][c] += dp[r - 1][c];
            }
            <span class="algorithm-keyword">if</span> (c &gt; 0) {
                dp[r][c] += dp[r][c - 1];
            }
            dp[r][c] %= mod;
        }
    }

    <span class="algorithm-keyword">return</span> dp[rows - 1][cols - 1];
}</code></pre>

---

## Minimum Cost

장애물 없는 격자. 시작·도착 칸 비용도 포함하며, 빈 격자는 `Long.MAX_VALUE`.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-keyword">for</span> (각 행) {
    <span class="algorithm-keyword">for</span> (각 열) {
        <span class="algorithm-keyword">if</span> (시작 칸이면) {
            <span class="algorithm-comment">// 시작 비용 기록</span>
        } <span class="algorithm-keyword">else</span> {
            <span class="algorithm-comment">// 위·왼쪽 중 작은 비용 + 현재 칸 비용</span>
        }
    }
}
<span class="algorithm-keyword">return</span> 도착 칸의 최소 비용;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">long</span> minPathCost(<span class="algorithm-keyword">int</span>[][] cost) {
    <span class="algorithm-keyword">int</span> rows = cost.length, cols = rows == 0 ? 0 : cost[0].length;
    <span class="algorithm-keyword">if</span> (rows == 0 || cols == 0) {
        <span class="algorithm-keyword">return</span> Long.MAX_VALUE;
    }
    <span class="algorithm-keyword">long</span>[][] dp = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[rows][cols];

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> r = 0; r &lt; rows; r++) {
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> c = 0; c &lt; cols; c++) {
            <span class="algorithm-keyword">if</span> (r == 0 &amp;&amp; c == 0) {
                dp[r][c] = cost[r][c];
                <span class="algorithm-keyword">continue</span>;
            }
            <span class="algorithm-keyword">long</span> previous = Long.MAX_VALUE;
            <span class="algorithm-keyword">if</span> (r &gt; 0) {
                previous = Math.min(previous, dp[r - 1][c]);
            }
            <span class="algorithm-keyword">if</span> (c &gt; 0) {
                previous = Math.min(previous, dp[r][c - 1]);
            }
            dp[r][c] = previous + cost[r][c];
        }
    }

    <span class="algorithm-keyword">return</span> dp[rows - 1][cols - 1];
}</code></pre>
