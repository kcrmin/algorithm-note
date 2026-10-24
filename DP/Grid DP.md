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
> **상태** · `dp[y][x]` = 시작점 `(0, 0)`에서 `(x, y)`에 도착한 결과.
>
> **좌표** · x는 오른쪽으로 증가하는 열, y는 아래로 증가하는 행. 배열은 `[y][x]`로 접근.
>
> **조건** · 직사각형 배열. 이동에 사이클이 없으므로 위·왼쪽 결과부터 계산 가능.
>
> **비용** · O(height × width) 시간·공간.

---

## Path Count

오른쪽·아래로 이동하여 `(0, 0)`에서 `(width - 1, height - 1)`까지 가는 경로 수를 mod로 나눈 값을 구한다.

`blocked[y][x] == true`면 통과 불가. `mod ≥ 2`.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// 막히지 않은 시작 칸의 경로 수 = 1</span>
<span class="algorithm-keyword">for</span> (각 행 y) {
    <span class="algorithm-keyword">for</span> (각 열 x) {
        <span class="algorithm-keyword">if</span> (막힌 칸이면) <span class="algorithm-keyword">continue</span>;
        <span class="algorithm-comment">// 위와 왼쪽의 경로 수를 더하기</span>
    }
}
<span class="algorithm-keyword">return</span> 도착 칸의 경로 수;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">long</span> countPaths(<span class="algorithm-keyword">boolean</span>[][] blocked, <span class="algorithm-keyword">int</span> mod) {
    <span class="algorithm-keyword">int</span> height = blocked.length, width = height == 0 ? 0 : blocked[0].length;
    <span class="algorithm-keyword">if</span> (height == 0 || width == 0 || blocked[0][0]) {
        <span class="algorithm-keyword">return</span> 0;
    }
    <span class="algorithm-keyword">long</span>[][] dp = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[height][width];
    dp[0][0] = 1;    <span class="algorithm-comment">// 출발점에 도달하는 빈 이동 한 가지</span>

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> y = 0; y &lt; height; y++) {
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> x = 0; x &lt; width; x++) {
            <span class="algorithm-keyword">if</span> (blocked[y][x]) {
                <span class="algorithm-keyword">continue</span>;
            }
            <span class="algorithm-keyword">if</span> (y &gt; 0) {
                dp[y][x] += dp[y - 1][x];
            }
            <span class="algorithm-keyword">if</span> (x &gt; 0) {
                dp[y][x] += dp[y][x - 1];
            }
            dp[y][x] %= mod;
        }
    }

    <span class="algorithm-keyword">return</span> dp[height - 1][width - 1];
}</code></pre>

---

## Minimum Cost

오른쪽·아래로 이동하여 `(0, 0)`에서 `(width - 1, height - 1)`까지 가는 최소 비용을 구한다.

장애물 없는 격자. 시작·도착 칸 비용도 포함하며, 빈 격자는 `Long.MAX_VALUE`.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-keyword">for</span> (각 행 y) {
    <span class="algorithm-keyword">for</span> (각 열 x) {
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
    <span class="algorithm-keyword">int</span> height = cost.length, width = height == 0 ? 0 : cost[0].length;
    <span class="algorithm-keyword">if</span> (height == 0 || width == 0) {
        <span class="algorithm-keyword">return</span> Long.MAX_VALUE;
    }
    <span class="algorithm-keyword">long</span>[][] dp = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[height][width];

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> y = 0; y &lt; height; y++) {
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> x = 0; x &lt; width; x++) {
            <span class="algorithm-keyword">if</span> (x == 0 &amp;&amp; y == 0) {
                dp[y][x] = cost[y][x];
                <span class="algorithm-keyword">continue</span>;
            }
            <span class="algorithm-keyword">long</span> previous = Long.MAX_VALUE;
            <span class="algorithm-keyword">if</span> (y &gt; 0) {
                previous = Math.min(previous, dp[y - 1][x]);
            }
            <span class="algorithm-keyword">if</span> (x &gt; 0) {
                previous = Math.min(previous, dp[y][x - 1]);
            }
            dp[y][x] = previous + cost[y][x];
        }
    }

    <span class="algorithm-keyword">return</span> dp[height - 1][width - 1];
}</code></pre>
