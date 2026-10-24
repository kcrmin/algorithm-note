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

각 정점까지 가장 가까운 시작점에서의 최소 간선 수를 구한다.

단일 시작점은 `new int[]{start}`. **방향 그래프는 `directed = true`**, 무방향은 `false`.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// 모든 시작점을 거리 0으로 큐에 삽입</span>
<span class="algorithm-keyword">while</span> (큐가 비어 있지 않으면) {
    <span class="algorithm-comment">// 앞에서 vertex 꺼내기</span>
    <span class="algorithm-keyword">for</span> (각 이웃 to) {
        <span class="algorithm-keyword">if</span> (아직 방문하지 않았다면) {
            <span class="algorithm-comment">// 거리 기록 → 큐에 삽입</span>
        }
    }
}
<span class="algorithm-keyword">return</span> dist;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span>[] bfs(<span class="algorithm-keyword">int</span> n, <span class="algorithm-keyword">int</span>[][] edges, <span class="algorithm-keyword">int</span>[] starts, <span class="algorithm-keyword">boolean</span> directed) {
    List&lt;List&lt;Integer&gt;&gt; graph = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;(n);
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> vertex = 0; vertex &lt; n; vertex++) {
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
        <span class="algorithm-keyword">int</span> vertex = queue.poll();
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> to : graph.get(vertex)) {
            <span class="algorithm-keyword">if</span> (dist[to] != -1) <span class="algorithm-keyword">continue</span>;
            dist[to] = dist[vertex] + 1;    <span class="algorithm-comment">// 넣을 때 방문 처리해야 중복 삽입을 막음</span>
            queue.offer(to);
        }
    }
    <span class="algorithm-keyword">return</span> dist;
}</code></pre>

---

## Grid

시작 칸에서 각 칸까지의 최소 이동 횟수를 구한다.

비어 있지 않은 직사각형 격자. 시작 좌표는 범위 안.

`x`는 열(오른쪽), `y`는 행(아래쪽), 배열 접근은 `board[y][x]`.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// 시작 칸의 거리 = 0</span>
<span class="algorithm-keyword">while</span> (큐에 칸이 남아 있으면) {
    <span class="algorithm-comment">// current = 앞에서 꺼낸 칸</span>
    <span class="algorithm-keyword">for</span> (상하좌우) {
        <span class="algorithm-keyword">if</span> (범위 밖 / 벽 / 방문한 칸이면) <span class="algorithm-keyword">continue</span>;
        <span class="algorithm-comment">// 거리 기록 → 큐에 삽입</span>
    }
}
<span class="algorithm-keyword">return</span> dist;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> Cell {
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span> x, y;
    Cell(<span class="algorithm-keyword">int</span> x, <span class="algorithm-keyword">int</span> y) {
        <span class="algorithm-keyword">this</span>.x = x;
        <span class="algorithm-keyword">this</span>.y = y;
    }
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span>[][] gridBfs(<span class="algorithm-keyword">int</span>[][] board, <span class="algorithm-keyword">int</span> startX, <span class="algorithm-keyword">int</span> startY) {
    <span class="algorithm-keyword">int</span> h = board.length, w = board[0].length;
    <span class="algorithm-keyword">int</span>[][] dist = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[h][w];
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span>[] row : dist) Arrays.fill(row, -1);
    <span class="algorithm-keyword">if</span> (board[startY][startX] == 1) <span class="algorithm-keyword">return</span> dist;

    <span class="algorithm-keyword">int</span>[] dx = {0, 1, 0, -1}, dy = {-1, 0, 1, 0};
    Queue&lt;Cell&gt; queue = <span class="algorithm-keyword">new</span> ArrayDeque&lt;&gt;();
    dist[startY][startX] = 0;
    queue.offer(<span class="algorithm-keyword">new</span> Cell(startX, startY));

    <span class="algorithm-keyword">while</span> (!queue.isEmpty()) {
        Cell current = queue.poll();
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> direction = 0; direction &lt; 4; direction++) {
            <span class="algorithm-keyword">int</span> nx = current.x + dx[direction], ny = current.y + dy[direction];
            <span class="algorithm-keyword">if</span> (nx &lt; 0 || nx &gt;= w || ny &lt; 0 || ny &gt;= h) <span class="algorithm-keyword">continue</span>;
            <span class="algorithm-keyword">if</span> (board[ny][nx] == 1 || dist[ny][nx] != -1) <span class="algorithm-keyword">continue</span>;
            dist[ny][nx] = dist[current.y][current.x] + 1;
            queue.offer(<span class="algorithm-keyword">new</span> Cell(nx, ny));
        }
    }
    <span class="algorithm-keyword">return</span> dist;
}</code></pre>

[[DFS]] · [[0-1 BFS]]
