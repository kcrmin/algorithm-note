---
tags:
  - graphs
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# 0-1 BFS

![0-1 BFS memory cue](../attachments/reference-0-1-bfs.svg)

> [!reference] 개념과 사용 조건
> **목적** · 가중치가 정확히 `0` 또는 `1`인 최단 거리.
>
> **입력** · `edges[i] = {from, to, weight}`, 정점 `0..n-1`. `directed=false`면 역방향도 추가.
>
> **반환** · 도달 불가는 `Integer.MAX_VALUE`. 방문 여부 대신 거리 개선 여부로 판단.
>
> **비용** · `O(V+E)`.

---

## Template

가중치가 0 또는 1인 그래프에서 시작점부터 각 정점까지의 최단 거리를 구한다.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// 시작 거리 = 0 → deque 앞에 삽입</span>
<span class="algorithm-keyword">while</span> (deque에 후보가 있으면) {
    <span class="algorithm-comment">// 앞에서 꺼내기</span>
    <span class="algorithm-keyword">if</span> (옛 후보면) <span class="algorithm-keyword">continue</span>;
    <span class="algorithm-keyword">for</span> (각 간선) {
        <span class="algorithm-keyword">if</span> (더 짧아지지 않으면) <span class="algorithm-keyword">continue</span>;
        <span class="algorithm-comment">// 거리 갱신</span>
        <span class="algorithm-keyword">if</span> (가중치가 0이면) { <span class="algorithm-comment">/* 앞에 삽입 */</span> }
        <span class="algorithm-keyword">else</span> { <span class="algorithm-comment">/* 뒤에 삽입 */</span> }
    }
}
<span class="algorithm-keyword">return</span> dist;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> Edge {
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span> to, weight;
    Edge(<span class="algorithm-keyword">int</span> to, <span class="algorithm-keyword">int</span> weight) {
        <span class="algorithm-keyword">this</span>.to = to;
        <span class="algorithm-keyword">this</span>.weight = weight;
    }
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> State {
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span> vertex, distance;
    State(<span class="algorithm-keyword">int</span> vertex, <span class="algorithm-keyword">int</span> distance) {
        <span class="algorithm-keyword">this</span>.vertex = vertex;
        <span class="algorithm-keyword">this</span>.distance = distance;
    }
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span>[] zeroOneBfs(<span class="algorithm-keyword">int</span> n, <span class="algorithm-keyword">int</span>[][] edges, <span class="algorithm-keyword">int</span> start, <span class="algorithm-keyword">boolean</span> directed) {
    List&lt;List&lt;Edge&gt;&gt; graph = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;(n);
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> v = 0; v &lt; n; v++) graph.add(<span class="algorithm-keyword">new</span> ArrayList&lt;&gt;());
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span>[] row : edges) {
        graph.get(row[0]).add(<span class="algorithm-keyword">new</span> Edge(row[1], row[2]));
        <span class="algorithm-keyword">if</span> (!directed) graph.get(row[1]).add(<span class="algorithm-keyword">new</span> Edge(row[0], row[2]));
    }
    <span class="algorithm-keyword">int</span>[] dist = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[n];
    Arrays.fill(dist, Integer.MAX_VALUE);
    Deque&lt;State&gt; deque = <span class="algorithm-keyword">new</span> ArrayDeque&lt;&gt;();
    dist[start] = 0;
    deque.offerFirst(<span class="algorithm-keyword">new</span> State(start, 0));

    <span class="algorithm-keyword">while</span> (!deque.isEmpty()) {
        State cur = deque.pollFirst();
        <span class="algorithm-keyword">if</span> (cur.distance != dist[cur.vertex]) <span class="algorithm-keyword">continue</span>;
        <span class="algorithm-keyword">for</span> (Edge edge : graph.get(cur.vertex)) {
            <span class="algorithm-keyword">int</span> next = cur.distance + edge.weight;
            <span class="algorithm-keyword">if</span> (next &gt;= dist[edge.to]) <span class="algorithm-keyword">continue</span>;
            dist[edge.to] = next;
            State candidate = <span class="algorithm-keyword">new</span> State(edge.to, next);
            <span class="algorithm-keyword">if</span> (edge.weight == 0) deque.offerFirst(candidate);    <span class="algorithm-comment">// 같은 거리 후보를 먼저 처리</span>
            <span class="algorithm-keyword">else</span> deque.offerLast(candidate);
        }
    }
    <span class="algorithm-keyword">return</span> dist;
}</code></pre>

[[BFS]] · [[Dijkstra]]
