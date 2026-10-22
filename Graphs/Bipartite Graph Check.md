---
tags:
  - graphs
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Bipartite Graph Check

![Bipartite Graph Check memory cue](../attachments/reference-bipartite-graph-check.svg)

> [!reference] 개념과 사용 조건
> **목적** · 무방향 그래프를 두 집합으로 나눠 모든 간선이 집합 사이에만 있는지 검사.
>
> **입력** · `edges[i] = {a, b}`. 연결되지 않은 성분도 모두 검사.
>
> **반환** · 가능하면 정점별 `0/1` 색, 불가능하면 `null`.
>
> **비용** · `O(V+E)`. 자기 루프와 홀수 사이클은 불가능.

---

## Template

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-keyword">for</span> (각 미방문 시작점) {
    <span class="algorithm-comment">// 첫 색 지정 → 큐에 삽입</span>
    <span class="algorithm-keyword">while</span> (큐가 비어 있지 않으면) {
        <span class="algorithm-comment">// 꺼내기</span>
        <span class="algorithm-keyword">for</span> (각 이웃) {
            <span class="algorithm-keyword">if</span> (색이 없으면) { <span class="algorithm-comment">/* 반대 색으로 칠하고 삽입 */</span> }
            <span class="algorithm-keyword">else</span> <span class="algorithm-keyword">if</span> (같은 색이면) <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">null</span>;
        }
    }
}
<span class="algorithm-keyword">return</span> color;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span>[] bipartiteColor(<span class="algorithm-keyword">int</span> n, <span class="algorithm-keyword">int</span>[][] edges) {
    List&lt;List&lt;Integer&gt;&gt; graph = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;(n);
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> v = 0; v &lt; n; v++) graph.add(<span class="algorithm-keyword">new</span> ArrayList&lt;&gt;());
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span>[] row : edges) {
        graph.get(row[0]).add(row[1]);
        graph.get(row[1]).add(row[0]);
    }
    <span class="algorithm-keyword">int</span>[] color = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[n];
    Arrays.fill(color, -1);
    Queue&lt;Integer&gt; queue = <span class="algorithm-keyword">new</span> ArrayDeque&lt;&gt;();
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> start = 0; start &lt; n; start++) {
        <span class="algorithm-keyword">if</span> (color[start] != -1) <span class="algorithm-keyword">continue</span>;
        color[start] = 0;
        queue.offer(start);
        <span class="algorithm-keyword">while</span> (!queue.isEmpty()) {
            <span class="algorithm-keyword">int</span> v = queue.poll();
            <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> to : graph.get(v)) {
                <span class="algorithm-keyword">if</span> (color[to] == -1) {
                    color[to] = color[v] ^ 1;
                    queue.offer(to);
                } <span class="algorithm-keyword">else</span> <span class="algorithm-keyword">if</span> (color[to] == color[v]) {
                    <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">null</span>;
                }
            }
        }
    }
    <span class="algorithm-keyword">return</span> color;
}</code></pre>

[[BFS]] · [[Bipartite Matching]]
