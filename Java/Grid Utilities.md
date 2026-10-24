---
tags:
  - java
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Grid Utilities

![Grid Utilities memory cue](../attachments/reference-grid-utilities.svg)

> [!reference] 개념과 사용 조건
> **목적** · 격자 탐색에서 방향·범위·회전 표현을 일정하게 유지.
>
> **좌표** · `grid[y][x]`. x는 오른쪽, y는 아래로 증가. 좌표 인자는 `(x, y)` 순서.
>
> **조건** · 회전 함수는 비어 있지 않은 직사각형 배열. 방향 0·1·2·3은 위·오른쪽·아래·왼쪽.

---

## Directions

상하좌우 이동량과 경계 검사를 준비하고, 현재 방향을 90° 돌린다.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-keyword">for</span> (네 방향) {
    <span class="algorithm-comment">// nx = x + DX[direction], ny = y + DY[direction]</span>
    <span class="algorithm-keyword">if</span> (범위를 벗어나면) <span class="algorithm-keyword">continue</span>;
    <span class="algorithm-comment">// 다음 칸 처리</span>
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span>[] DX = {0, 1, 0, -1};
<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span>[] DY = {-1, 0, 1, 0};

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">boolean</span> inside(<span class="algorithm-keyword">int</span> x, <span class="algorithm-keyword">int</span> y, <span class="algorithm-keyword">int</span> width, <span class="algorithm-keyword">int</span> height) {
    <span class="algorithm-keyword">return</span> 0 &lt;= x &amp;&amp; x &lt; width &amp;&amp; 0 &lt;= y &amp;&amp; y &lt; height;
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span> turnRight(<span class="algorithm-keyword">int</span> direction) {
    <span class="algorithm-keyword">return</span> (direction + 1) % 4;
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span> turnLeft(<span class="algorithm-keyword">int</span> direction) {
    <span class="algorithm-keyword">return</span> (direction + 3) % 4;    <span class="algorithm-comment">// Java의 음수 나머지를 피함</span>
}</code></pre>

---

## Rotate Clockwise

격자 전체를 시계 방향으로 90° 회전한 새 배열을 만든다.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// 결과 크기: 높이 = 기존 width, 너비 = 기존 height</span>
<span class="algorithm-keyword">for</span> (각 y) {
    <span class="algorithm-keyword">for</span> (각 x) {
        <span class="algorithm-comment">// (x, y) → (height - 1 - y, x)</span>
    }
}
<span class="algorithm-keyword">return</span> rotated;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span>[][] rotateClockwise(<span class="algorithm-keyword">int</span>[][] grid) {
    <span class="algorithm-keyword">int</span> height = grid.length, width = grid[0].length;
    <span class="algorithm-keyword">int</span>[][] rotated = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[width][height];

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> y = 0; y &lt; height; y++) {
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> x = 0; x &lt; width; x++) {
            rotated[x][height - 1 - y] = grid[y][x];
        }
    }
    <span class="algorithm-keyword">return</span> rotated;
}</code></pre>

[[BFS]] · [[DFS]]
