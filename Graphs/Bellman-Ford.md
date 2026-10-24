---
tags:
  - graphs
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Bellman-Ford

![Bellman-Ford memory cue](../attachments/reference-bellman-ford.svg)

> [!reference] 개념과 사용 조건
> **목적** · 음수 간선이 있는 방향 그래프의 최단 거리와 음수 사이클 검사.
>
> **입력** · `List<Edge>`에 `new Edge(from, to, weight)` 추가. 정점 `0..n-1`.
>
> **반환** · 시작점 버전은 도달 가능한 음수 사이클이면 `null`, 미도달 거리는 `Long.MAX_VALUE / 4`.
>
> **비용·범위** · `O(VE)`. 유한 거리의 절댓값은 INF 미만, 완화 중 덧셈은 `long` 범위 안.

---

## From One Start

시작점에서 각 정점까지의 최단 거리를 구하고 도달 가능한 음수 사이클을 검사한다.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// start = 0, 나머지 = ∞</span>
<span class="algorithm-keyword">for</span> (V번 반복) {
    <span class="algorithm-comment">// changed = false</span>
    <span class="algorithm-keyword">for</span> (각 간선) {
        <span class="algorithm-keyword">if</span> (출발점에 닿을 수 없으면) <span class="algorithm-keyword">continue</span>;
        <span class="algorithm-keyword">if</span> (더 짧아지면) { <span class="algorithm-comment">/* 갱신, changed = true */</span> }
    }
    <span class="algorithm-keyword">if</span> (변화가 없으면) <span class="algorithm-keyword">return</span> dist;
    <span class="algorithm-keyword">if</span> (V번째에도 변하면) <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">null</span>;
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> Edge {
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span> from, to, weight;
    Edge(<span class="algorithm-keyword">int</span> from, <span class="algorithm-keyword">int</span> to, <span class="algorithm-keyword">int</span> weight) {
        <span class="algorithm-keyword">this</span>.from = from;
        <span class="algorithm-keyword">this</span>.to = to;
        <span class="algorithm-keyword">this</span>.weight = weight;
    }
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">long</span>[] bellmanFord(<span class="algorithm-keyword">int</span> n, List&lt;Edge&gt; edges, <span class="algorithm-keyword">int</span> start) {
    <span class="algorithm-keyword">long</span> inf = Long.MAX_VALUE / 4;
    <span class="algorithm-keyword">long</span>[] dist = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[n];
    Arrays.fill(dist, inf);
    dist[start] = 0;

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> round = 0; round &lt; n; round++) {
        <span class="algorithm-keyword">boolean</span> changed = <span class="algorithm-keyword">false</span>;
        <span class="algorithm-keyword">for</span> (Edge edge : edges) {
            <span class="algorithm-keyword">if</span> (dist[edge.from] == inf) <span class="algorithm-keyword">continue</span>;    <span class="algorithm-comment">// 시작점에서 닿는 간선만 완화</span>
            <span class="algorithm-keyword">long</span> nextDistance = dist[edge.from] + edge.weight;
            <span class="algorithm-keyword">if</span> (nextDistance &gt;= dist[edge.to]) <span class="algorithm-keyword">continue</span>;
            dist[edge.to] = nextDistance;
            changed = <span class="algorithm-keyword">true</span>;
        }
        <span class="algorithm-keyword">if</span> (!changed) <span class="algorithm-keyword">return</span> dist;
        <span class="algorithm-keyword">if</span> (round == n - 1) <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">null</span>;    <span class="algorithm-comment">// 단순 경로는 최대 V-1개 간선</span>
    }
    <span class="algorithm-keyword">return</span> dist;
}</code></pre>

---

## Anywhere in the Graph

연결되지 않은 성분의 음수 사이클까지 검사한다.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// 모든 거리 = 0</span>
<span class="algorithm-keyword">for</span> (V번 반복) {
    <span class="algorithm-comment">// 전체 간선 완화</span>
    <span class="algorithm-keyword">if</span> (변화가 없으면) <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">false</span>;
    <span class="algorithm-keyword">if</span> (V번째에도 변하면) <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">true</span>;
}
<span class="algorithm-keyword">return</span> <span class="algorithm-keyword">false</span>;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> Edge {
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span> from, to, weight;
    Edge(<span class="algorithm-keyword">int</span> from, <span class="algorithm-keyword">int</span> to, <span class="algorithm-keyword">int</span> weight) {
        <span class="algorithm-keyword">this</span>.from = from;
        <span class="algorithm-keyword">this</span>.to = to;
        <span class="algorithm-keyword">this</span>.weight = weight;
    }
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">boolean</span> hasNegativeCycle(<span class="algorithm-keyword">int</span> n, List&lt;Edge&gt; edges) {
    <span class="algorithm-keyword">long</span>[] dist = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[n];    <span class="algorithm-comment">// 모든 정점에 비용 0으로 도착한 가상 시작점</span>
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> round = 0; round &lt; n; round++) {
        <span class="algorithm-keyword">boolean</span> changed = <span class="algorithm-keyword">false</span>;
        <span class="algorithm-keyword">for</span> (Edge edge : edges) {
            <span class="algorithm-keyword">long</span> nextDistance = dist[edge.from] + edge.weight;
            <span class="algorithm-keyword">if</span> (nextDistance &gt;= dist[edge.to]) <span class="algorithm-keyword">continue</span>;
            dist[edge.to] = nextDistance;
            changed = <span class="algorithm-keyword">true</span>;
        }
        <span class="algorithm-keyword">if</span> (!changed) <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">false</span>;
        <span class="algorithm-keyword">if</span> (round == n - 1) <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">true</span>;
    }
    <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">false</span>;
}</code></pre>

[[Dijkstra]] · [[Floyd-Warshall]]
