---
tags:
  - graphs
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# DFS

![DFS memory cue](../attachments/reference-dfs.svg)

> [!reference] 개념과 사용 조건
> **목적** · 깊이 우선 탐색, 연결 성분과 사이클 판정.
>
> **입력** · `edges[i] = {from, to}`, 정점 `0..n-1`.
>
> **구분** · 연결 성분은 무방향. 방향 사이클은 방문 중인 정점으로 돌아오는지 본다.
>
> **비용** · `O(V+E)`. 사이클 판정은 재귀이며, 긴 DFS 경로에서는 `StackOverflowError` 가능.

---

## Connected Components

무방향 그래프의 각 정점이 속한 연결 성분 번호를 구한다.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-keyword">for</span> (각 정점) {
    <span class="algorithm-keyword">if</span> (이미 방문했으면) <span class="algorithm-keyword">continue</span>;
    <span class="algorithm-comment">// 새 연결 성분의 시작점 삽입</span>
    <span class="algorithm-keyword">while</span> (스택이 비어 있지 않으면) {
        <span class="algorithm-comment">// vertex 꺼내기 → 미방문 이웃에 같은 componentId 지정</span>
    }
}
<span class="algorithm-keyword">return</span> component;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span>[] components(<span class="algorithm-keyword">int</span> n, <span class="algorithm-keyword">int</span>[][] edges) {
    List&lt;List&lt;Integer&gt;&gt; graph = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;(n);
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> vertex = 0; vertex &lt; n; vertex++) graph.add(<span class="algorithm-keyword">new</span> ArrayList&lt;&gt;());
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span>[] row : edges) {
        graph.get(row[0]).add(row[1]);
        graph.get(row[1]).add(row[0]);
    }

    <span class="algorithm-keyword">int</span>[] component = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[n];
    Arrays.fill(component, -1);
    Deque&lt;Integer&gt; stack = <span class="algorithm-keyword">new</span> ArrayDeque&lt;&gt;();
    <span class="algorithm-keyword">int</span> componentId = 0;
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> start = 0; start &lt; n; start++) {
        <span class="algorithm-keyword">if</span> (component[start] != -1) <span class="algorithm-keyword">continue</span>;
        component[start] = componentId;
        stack.push(start);
        <span class="algorithm-keyword">while</span> (!stack.isEmpty()) {
            <span class="algorithm-keyword">int</span> vertex = stack.pop();
            <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> to : graph.get(vertex)) {
                <span class="algorithm-keyword">if</span> (component[to] != -1) <span class="algorithm-keyword">continue</span>;
                component[to] = componentId;
                stack.push(to);
            }
        }
        componentId++;
    }
    <span class="algorithm-keyword">return</span> component;
}</code></pre>

---

## Directed Cycle

방향 그래프에 사이클이 있는지 판정한다.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-keyword">boolean</span> cycleDfs(vertex) {
    <span class="algorithm-comment">// vertex = 방문 중</span>
    <span class="algorithm-keyword">for</span> (각 이웃) {
        <span class="algorithm-keyword">if</span> (방문 중인 이웃이면) <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">true</span>;
        <span class="algorithm-keyword">if</span> (미방문 이웃에서 사이클을 찾으면) <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">true</span>;
    }
    <span class="algorithm-comment">// vertex = 방문 완료</span>
    <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">false</span>;
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">boolean</span> hasDirectedCycle(<span class="algorithm-keyword">int</span> n, <span class="algorithm-keyword">int</span>[][] edges) {
    List&lt;List&lt;Integer&gt;&gt; graph = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;(n);
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> vertex = 0; vertex &lt; n; vertex++) graph.add(<span class="algorithm-keyword">new</span> ArrayList&lt;&gt;());
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span>[] row : edges) graph.get(row[0]).add(row[1]);

    <span class="algorithm-keyword">int</span>[] color = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[n];
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> vertex = 0; vertex &lt; n; vertex++) {
        <span class="algorithm-keyword">if</span> (color[vertex] == 0 &amp;&amp; cycleDfs(vertex, graph, color)) <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">true</span>;
    }
    <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">false</span>;
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">boolean</span> cycleDfs(<span class="algorithm-keyword">int</span> vertex, List&lt;List&lt;Integer&gt;&gt; graph, <span class="algorithm-keyword">int</span>[] color) {
    color[vertex] = 1;
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> to : graph.get(vertex)) {
        <span class="algorithm-keyword">if</span> (color[to] == 1) <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">true</span>;    <span class="algorithm-comment">// 현재 탐색 경로로 돌아가는 간선</span>
        <span class="algorithm-keyword">if</span> (color[to] == 0 &amp;&amp; cycleDfs(to, graph, color)) <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">true</span>;
    }
    color[vertex] = 2;    <span class="algorithm-comment">// 완료된 다른 경로로 가는 간선은 사이클이 아님</span>
    <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">false</span>;
}</code></pre>

---

## Undirected Cycle

무방향 그래프에 사이클이 있는지 판정한다.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-keyword">boolean</span> cycleDfs(vertex, parentEdge) {
    <span class="algorithm-comment">// 방문 처리</span>
    <span class="algorithm-keyword">for</span> (각 간선) {
        <span class="algorithm-keyword">if</span> (들어왔던 간선이면) <span class="algorithm-keyword">continue</span>;
        <span class="algorithm-keyword">if</span> (이미 방문했거나 다음 탐색에서 사이클이면) <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">true</span>;
    }
    <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">false</span>;
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> Edge {
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span> to, id;
    Edge(<span class="algorithm-keyword">int</span> to, <span class="algorithm-keyword">int</span> id) {
        <span class="algorithm-keyword">this</span>.to = to;
        <span class="algorithm-keyword">this</span>.id = id;
    }
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">boolean</span> hasUndirectedCycle(<span class="algorithm-keyword">int</span> n, <span class="algorithm-keyword">int</span>[][] edges) {
    List&lt;List&lt;Edge&gt;&gt; graph = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;(n);
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> vertex = 0; vertex &lt; n; vertex++) graph.add(<span class="algorithm-keyword">new</span> ArrayList&lt;&gt;());
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> id = 0; id &lt; edges.length; id++) {
        <span class="algorithm-keyword">int</span> a = edges[id][0], b = edges[id][1];
        graph.get(a).add(<span class="algorithm-keyword">new</span> Edge(b, id));
        graph.get(b).add(<span class="algorithm-keyword">new</span> Edge(a, id));
    }
    <span class="algorithm-keyword">boolean</span>[] visited = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">boolean</span>[n];
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> vertex = 0; vertex &lt; n; vertex++) {
        <span class="algorithm-keyword">if</span> (!visited[vertex] &amp;&amp; cycleDfs(vertex, -1, graph, visited)) <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">true</span>;
    }
    <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">false</span>;
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">boolean</span> cycleDfs(<span class="algorithm-keyword">int</span> vertex, <span class="algorithm-keyword">int</span> parentEdge, List&lt;List&lt;Edge&gt;&gt; graph,
                        <span class="algorithm-keyword">boolean</span>[] visited) {
    visited[vertex] = <span class="algorithm-keyword">true</span>;
    <span class="algorithm-keyword">for</span> (Edge edge : graph.get(vertex)) {
        <span class="algorithm-keyword">if</span> (edge.id == parentEdge) <span class="algorithm-keyword">continue</span>;    <span class="algorithm-comment">// 정점이 아닌 간선을 제외해야 평행 간선을 감지</span>
        <span class="algorithm-keyword">if</span> (visited[edge.to]) <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">true</span>;
        <span class="algorithm-keyword">if</span> (cycleDfs(edge.to, edge.id, graph, visited)) <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">true</span>;
    }
    <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">false</span>;
}</code></pre>

[[BFS]] · [[Topological Sort]]
