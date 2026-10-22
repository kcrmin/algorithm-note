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
> **좌표** · `grid[row][col]`. 행은 아래로, 열은 오른쪽으로 증가.
>
> **조건** · 회전 함수는 비어 있지 않은 직사각형 배열. 방향 0·1·2·3은 위·오른쪽·아래·왼쪽.

---

## Directions

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-keyword">for</span> (네 방향) {
    <span class="algorithm-comment">// 다음 행·열 계산</span>
    <span class="algorithm-keyword">if</span> (범위를 벗어나면) <span class="algorithm-keyword">continue</span>;
    <span class="algorithm-comment">// 다음 칸 처리</span>
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span>[] DR = {-1, 0, 1, 0};
<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span>[] DC = {0, 1, 0, -1};

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">boolean</span> inside(<span class="algorithm-keyword">int</span> row, <span class="algorithm-keyword">int</span> col, <span class="algorithm-keyword">int</span> rows, <span class="algorithm-keyword">int</span> cols) {
    <span class="algorithm-keyword">return</span> 0 &lt;= row &amp;&amp; row &lt; rows &amp;&amp; 0 &lt;= col &amp;&amp; col &lt; cols;
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span> turnRight(<span class="algorithm-keyword">int</span> direction) {
    <span class="algorithm-keyword">return</span> (direction + 1) % 4;
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span> turnLeft(<span class="algorithm-keyword">int</span> direction) {
    <span class="algorithm-keyword">return</span> (direction + 3) % 4;    <span class="algorithm-comment">// Java의 음수 나머지를 피함</span>
}</code></pre>

---

## Rotate Clockwise

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// 결과 크기: cols × rows</span>
<span class="algorithm-keyword">for</span> (각 행) {
    <span class="algorithm-keyword">for</span> (각 열) {
        <span class="algorithm-comment">// 원래 행은 반전된 열로, 원래 열은 새 행으로</span>
    }
}
<span class="algorithm-keyword">return</span> rotated;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span>[][] rotateClockwise(<span class="algorithm-keyword">int</span>[][] grid) {
    <span class="algorithm-keyword">int</span> rows = grid.length, cols = grid[0].length;
    <span class="algorithm-keyword">int</span>[][] rotated = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[cols][rows];

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> row = 0; row &lt; rows; row++) {
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> col = 0; col &lt; cols; col++) {
            rotated[col][rows - 1 - row] = grid[row][col];
        }
    }
    <span class="algorithm-keyword">return</span> rotated;
}</code></pre>

[[BFS]] · [[DFS]]
