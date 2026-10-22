---
tags:
  - graphs
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Topological Sort

![Topological Sort memory cue](../attachments/reference-topological-sort.svg)

> [!reference] 개념과 사용 조건
> **목적** · 방향 그래프에서 모든 선행 조건을 지키는 순서.
>
> **입력** · `edges[i] = {before, after}`, 정점 `0..n-1`.
>
> **반환** · 사이클이 있으면 `null`. 여러 답이 있으면 Queue는 임의 순서, Heap은 사전순 최소.
>
> **비용** · Queue `O(V+E)`, Heap `O(E+V log V)`.

---

## Queue

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// 진입 차수 0인 정점 삽입</span>
<span class="algorithm-keyword">while</span> (처리 가능한 정점이 있으면) {
    <span class="algorithm-comment">// 꺼내기 → 순서에 기록</span>
    <span class="algorithm-keyword">for</span> (각 다음 정점) {
        <span class="algorithm-comment">// 진입 차수 감소</span>
        <span class="algorithm-keyword">if</span> (0이 되면) { <span class="algorithm-comment">/* 큐에 삽입 */</span> }
    }
}
<span class="algorithm-keyword">return</span> 모든 정점을 처리했는지 확인;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span>[] topologicalSort(<span class="algorithm-keyword">int</span> n, <span class="algorithm-keyword">int</span>[][] edges) {
    List&lt;List&lt;Integer&gt;&gt; graph = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;(n);
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> v = 0; v &lt; n; v++) graph.add(<span class="algorithm-keyword">new</span> ArrayList&lt;&gt;());
    <span class="algorithm-keyword">int</span>[] indegree = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[n];
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span>[] row : edges) {
        graph.get(row[0]).add(row[1]);
        indegree[row[1]]++;
    }

    Queue&lt;Integer&gt; ready = <span class="algorithm-keyword">new</span> ArrayDeque&lt;&gt;();
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> v = 0; v &lt; n; v++) {
        <span class="algorithm-keyword">if</span> (indegree[v] == 0) ready.offer(v);
    }
    <span class="algorithm-keyword">int</span>[] order = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[n];
    <span class="algorithm-keyword">int</span> count = 0;
    <span class="algorithm-keyword">while</span> (!ready.isEmpty()) {
        <span class="algorithm-keyword">int</span> v = ready.poll();
        order[count++] = v;
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> to : graph.get(v)) {
            <span class="algorithm-keyword">if</span> (--indegree[to] == 0) ready.offer(to);    <span class="algorithm-comment">// 모든 선행 정점이 끝난 순간에만 삽입</span>
        }
    }
    <span class="algorithm-keyword">return</span> count == n ? order : <span class="algorithm-keyword">null</span>;    <span class="algorithm-comment">// 남은 정점에는 사이클 의존성이 있음</span>
}</code></pre>

---

## Lexicographically Smallest

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// 진입 차수 0인 정점을 최소 힙에 삽입</span>
<span class="algorithm-keyword">while</span> (처리 가능한 정점이 있으면) {
    <span class="algorithm-comment">// 가장 작은 번호 꺼내기 → 순서에 기록</span>
    <span class="algorithm-keyword">for</span> (각 다음 정점) {
        <span class="algorithm-keyword">if</span> (진입 차수를 줄여 0이 되면) { <span class="algorithm-comment">/* 힙에 삽입 */</span> }
    }
}
<span class="algorithm-keyword">return</span> 모든 정점을 처리했는지 확인;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span>[] lexicographicTopologicalSort(<span class="algorithm-keyword">int</span> n, <span class="algorithm-keyword">int</span>[][] edges) {
    List&lt;List&lt;Integer&gt;&gt; graph = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;(n);
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> v = 0; v &lt; n; v++) graph.add(<span class="algorithm-keyword">new</span> ArrayList&lt;&gt;());
    <span class="algorithm-keyword">int</span>[] indegree = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[n];
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span>[] row : edges) {
        graph.get(row[0]).add(row[1]);
        indegree[row[1]]++;
    }

    Queue&lt;Integer&gt; ready = <span class="algorithm-keyword">new</span> PriorityQueue&lt;&gt;();
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> v = 0; v &lt; n; v++) {
        <span class="algorithm-keyword">if</span> (indegree[v] == 0) ready.offer(v);
    }
    <span class="algorithm-keyword">int</span>[] order = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[n];
    <span class="algorithm-keyword">int</span> count = 0;
    <span class="algorithm-keyword">while</span> (!ready.isEmpty()) {
        <span class="algorithm-keyword">int</span> v = ready.poll();
        order[count++] = v;
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> to : graph.get(v)) {
            <span class="algorithm-keyword">if</span> (--indegree[to] == 0) ready.offer(to);    <span class="algorithm-comment">// 모든 선행 정점이 끝난 순간에만 삽입</span>
        }
    }
    <span class="algorithm-keyword">return</span> count == n ? order : <span class="algorithm-keyword">null</span>;    <span class="algorithm-comment">// 남은 정점에는 사이클 의존성이 있음</span>
}</code></pre>

[[DAG DP]] · [[DFS]]
