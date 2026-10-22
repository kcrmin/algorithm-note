---
tags:
  - graphs
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# BFS

![BFS memory cue](../attachments/reference-bfs.svg)

> [!reference] 개념과 사용 조건
> **목적** · 동일한 간선 비용에서 최단 거리. 정점은 `0..n-1`.
>
> **입력** · `edges[i] = {from, to}`. `starts`에 시작점 하나 또는 여러 개를 넣는다.
>
> **반환** · 도달 불가는 `-1`. Grid에서는 `0`이 이동 가능, `1`이 벽.
>
> **비용** · Graph `O(V+E)`, Grid `O(HW)`.

---

## Graph · Single / Multi-source

단일 시작점은 `new int[]{start}`. **방향 그래프는 `directed = true`**, 무방향은 `false`.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// 모든 시작점을 거리 0으로 큐에 삽입</span>
<span class="algorithm-keyword">while</span> (큐가 비어 있지 않으면) {
    <span class="algorithm-comment">// 앞에서 정점 꺼내기</span>
    <span class="algorithm-keyword">for</span> (각 이웃) {
        <span class="algorithm-keyword">if</span> (아직 방문하지 않았다면) {
            <span class="algorithm-comment">// 거리 기록 → 큐에 삽입</span>
        }
    }
}
<span class="algorithm-keyword">return</span> dist;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span>[] bfs(<span class="algorithm-keyword">int</span> n, <span class="algorithm-keyword">int</span>[][] edges, <span class="algorithm-keyword">int</span>[] starts, <span class="algorithm-keyword">boolean</span> directed) {
    List&lt;List&lt;Integer&gt;&gt; graph = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;(n);
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> v = 0; v &lt; n; v++) {
        graph.add(<span class="algorithm-keyword">new</span> ArrayList&lt;&gt;());
    }
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span>[] row : edges) {
        graph.get(row[0]).add(row[1]);
        <span class="algorithm-keyword">if</span> (!directed) graph.get(row[1]).add(row[0]);
    }

    <span class="algorithm-keyword">int</span>[] dist = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[n];
    Arrays.fill(dist, -1);
    Queue&lt;Integer&gt; queue = <span class="algorithm-keyword">new</span> ArrayDeque&lt;&gt;();
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> start : starts) {
        <span class="algorithm-keyword">if</span> (dist[start] != -1) <span class="algorithm-keyword">continue</span>;
        dist[start] = 0;
        queue.offer(start);
    }

    <span class="algorithm-keyword">while</span> (!queue.isEmpty()) {
        <span class="algorithm-keyword">int</span> v = queue.poll();
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> to : graph.get(v)) {
            <span class="algorithm-keyword">if</span> (dist[to] != -1) <span class="algorithm-keyword">continue</span>;
            dist[to] = dist[v] + 1;    <span class="algorithm-comment">// 넣을 때 방문 처리해야 중복 삽입을 막음</span>
            queue.offer(to);
        }
    }
    <span class="algorithm-keyword">return</span> dist;
}</code></pre>

---

## Grid

비어 있지 않은 직사각형 격자. 시작 좌표는 범위 안.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// 시작 칸의 거리 = 0</span>
<span class="algorithm-keyword">while</span> (큐에 칸이 남아 있으면) {
    <span class="algorithm-comment">// 앞의 칸 꺼내기</span>
    <span class="algorithm-keyword">for</span> (상하좌우) {
        <span class="algorithm-keyword">if</span> (범위 밖 / 벽 / 방문한 칸이면) <span class="algorithm-keyword">continue</span>;
        <span class="algorithm-comment">// 거리 기록 → 큐에 삽입</span>
    }
}
<span class="algorithm-keyword">return</span> dist;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> Cell {
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span> row, col;
    Cell(<span class="algorithm-keyword">int</span> row, <span class="algorithm-keyword">int</span> col) {
        <span class="algorithm-keyword">this</span>.row = row;
        <span class="algorithm-keyword">this</span>.col = col;
    }
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span>[][] gridBfs(<span class="algorithm-keyword">int</span>[][] board, <span class="algorithm-keyword">int</span> startRow, <span class="algorithm-keyword">int</span> startCol) {
    <span class="algorithm-keyword">int</span> h = board.length, w = board[0].length;
    <span class="algorithm-keyword">int</span>[][] dist = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[h][w];
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span>[] row : dist) Arrays.fill(row, -1);
    <span class="algorithm-keyword">if</span> (board[startRow][startCol] == 1) <span class="algorithm-keyword">return</span> dist;

    <span class="algorithm-keyword">int</span>[] dr = {-1, 1, 0, 0}, dc = {0, 0, -1, 1};
    Queue&lt;Cell&gt; queue = <span class="algorithm-keyword">new</span> ArrayDeque&lt;&gt;();
    dist[startRow][startCol] = 0;
    queue.offer(<span class="algorithm-keyword">new</span> Cell(startRow, startCol));

    <span class="algorithm-keyword">while</span> (!queue.isEmpty()) {
        Cell cur = queue.poll();
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> d = 0; d &lt; 4; d++) {
            <span class="algorithm-keyword">int</span> nr = cur.row + dr[d], nc = cur.col + dc[d];
            <span class="algorithm-keyword">if</span> (nr &lt; 0 || nr &gt;= h || nc &lt; 0 || nc &gt;= w) <span class="algorithm-keyword">continue</span>;
            <span class="algorithm-keyword">if</span> (board[nr][nc] == 1 || dist[nr][nc] != -1) <span class="algorithm-keyword">continue</span>;
            dist[nr][nc] = dist[cur.row][cur.col] + 1;
            queue.offer(<span class="algorithm-keyword">new</span> Cell(nr, nc));
        }
    }
    <span class="algorithm-keyword">return</span> dist;
}</code></pre>

[[DFS]] · [[0-1 BFS]]
