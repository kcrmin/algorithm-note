---
tags:
  - trees
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Tree Diameter

![Tree Diameter memory cue](../attachments/reference-tree-diameter.svg)

> [!reference] 개념과 사용 조건
> **목적** · 트리에서 가장 긴 단순 경로의 길이.
>
> **조건** · 연결된 무방향 트리, 가중치 ≥ 0. 일반 그래프·음수 간선에는 이 두 번 탐색법을 적용하지 않는다.
>
> **입력·반환** · `edges[i]={a,b,weight}`. 가중치가 없으면 1로 입력. 빈 트리·정점 하나는 `0`.
>
> **비용** · 시간·공간 `O(V)`. 거리 합은 `long`.

---

## Two Traversals

**Structure**

<pre class="algorithm-code" data-language="text"><code>Farthest farthest(start) {
    <span class="algorithm-comment">// start 거리 = 0</span>
    <span class="algorithm-keyword">while</span> (큐가 비어 있지 않으면) {
        <span class="algorithm-keyword">for</span> (미방문 이웃) {
            <span class="algorithm-comment">// 거리 누적 → 가장 먼 정점 갱신</span>
        }
    }
    <span class="algorithm-keyword">return</span> 가장 먼 정점과 거리;
}

<span class="algorithm-comment">// 임의 정점 → A</span>
<span class="algorithm-comment">// A → B</span>
<span class="algorithm-keyword">return</span> A에서 B까지 거리;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> Edge {
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span> to, weight;
    Edge(<span class="algorithm-keyword">int</span> to, <span class="algorithm-keyword">int</span> weight) {
        <span class="algorithm-keyword">this</span>.to = to;
        <span class="algorithm-keyword">this</span>.weight = weight;
    }
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> Farthest {
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span> vertex;
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">long</span> distance;
    Farthest(<span class="algorithm-keyword">int</span> vertex, <span class="algorithm-keyword">long</span> distance) {
        <span class="algorithm-keyword">this</span>.vertex = vertex;
        <span class="algorithm-keyword">this</span>.distance = distance;
    }
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">long</span> treeDiameter(<span class="algorithm-keyword">int</span> n, <span class="algorithm-keyword">int</span>[][] edges) {
    <span class="algorithm-keyword">if</span> (n == 0) <span class="algorithm-keyword">return</span> 0;
    List&lt;List&lt;Edge&gt;&gt; graph = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;(n);
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> v = 0; v &lt; n; v++) graph.add(<span class="algorithm-keyword">new</span> ArrayList&lt;&gt;());
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span>[] row : edges) {
        graph.get(row[0]).add(<span class="algorithm-keyword">new</span> Edge(row[1], row[2]));
        graph.get(row[1]).add(<span class="algorithm-keyword">new</span> Edge(row[0], row[2]));
    }
    Farthest first = farthest(graph, 0);
    <span class="algorithm-keyword">return</span> farthest(graph, first.vertex).distance;
}

<span class="algorithm-keyword">static</span> Farthest farthest(List&lt;List&lt;Edge&gt;&gt; graph, <span class="algorithm-keyword">int</span> start) {
    <span class="algorithm-keyword">long</span>[] dist = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[graph.size()];
    Arrays.fill(dist, -1);
    Queue&lt;Integer&gt; queue = <span class="algorithm-keyword">new</span> ArrayDeque&lt;&gt;();
    dist[start] = 0;
    queue.offer(start);
    <span class="algorithm-keyword">int</span> best = start;
    <span class="algorithm-keyword">while</span> (!queue.isEmpty()) {
        <span class="algorithm-keyword">int</span> v = queue.poll();
        <span class="algorithm-keyword">for</span> (Edge edge : graph.get(v)) {
            <span class="algorithm-keyword">if</span> (dist[edge.to] != -1) <span class="algorithm-keyword">continue</span>;
            dist[edge.to] = dist[v] + edge.weight;    <span class="algorithm-comment">// 트리는 경로가 유일하므로 힙 불필요</span>
            <span class="algorithm-keyword">if</span> (dist[edge.to] &gt; dist[best]) best = edge.to;
            queue.offer(edge.to);
        }
    }
    <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">new</span> Farthest(best, dist[best]);
}</code></pre>

[[Rooted Tree Traversal]]
