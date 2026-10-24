---
tags:
  - graphs
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Bridges and Articulation Points

![Bridges and Articulation Points memory cue](../attachments/reference-bridges-and-articulation-points.svg)

> [!reference] 개념과 사용 조건
> **목적** · 무방향 그래프에서 제거하면 연결 성분 수가 늘어나는 간선·정점.
>
> **기억** · Bridge는 `low[child] > entered[parent]`. 일반 정점의 Cut 조건은 `≥`.
>
> **예외** · DFS 루트는 자식이 2개 이상일 때만 단절점. `low`는 조상으로 돌아갈 수 있는 최소 방문 번호.
>
> **사용·비용** · `new LowLink(n, edges)`의 `bridge[간선번호]`, `articulation[정점]`. `O(V+E)`. 재귀이므로 긴 DFS 경로에서는 `StackOverflowError` 가능.

---

## Low-link DFS

무방향 그래프에서 제거하면 연결 성분 수가 늘어나는 간선과 정점을 찾는다.

`edges[i]={a,b}`. 자기 루프·평행 간선·분리된 성분을 허용한다.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-keyword">void</span> dfs(v, parentEdge) {
    <span class="algorithm-comment">// entered[v] = low[v] = 새 방문 번호</span>
    <span class="algorithm-keyword">for</span> (각 간선) {
        <span class="algorithm-keyword">if</span> (들어왔던 간선이면) <span class="algorithm-keyword">continue</span>;
        <span class="algorithm-keyword">if</span> (미방문 이웃이면) {
            <span class="algorithm-comment">// 자식 탐색 → low 갱신</span>
            <span class="algorithm-keyword">if</span> (자식 low &gt; 내 방문 번호면) { <span class="algorithm-comment">/* bridge */</span> }
            <span class="algorithm-keyword">if</span> (루트가 아니고 자식 low ≥ 내 방문 번호면) { <span class="algorithm-comment">/* articulation */</span> }
        } <span class="algorithm-keyword">else</span> {
            <span class="algorithm-comment">// 이웃 방문 번호로 low 갱신</span>
        }
    }
    <span class="algorithm-keyword">if</span> (루트이고 DFS 자식이 2개 이상이면) { <span class="algorithm-comment">/* articulation */</span> }
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> Edge {
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span> to, id;
    Edge(<span class="algorithm-keyword">int</span> to, <span class="algorithm-keyword">int</span> id) {
        <span class="algorithm-keyword">this</span>.to = to;
        <span class="algorithm-keyword">this</span>.id = id;
    }
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> LowLink {
    <span class="algorithm-keyword">final</span> List&lt;List&lt;Edge&gt;&gt; graph = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;();
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span>[] entered, low;
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">boolean</span>[] bridge, articulation;
    <span class="algorithm-keyword">int</span> timer;

    LowLink(<span class="algorithm-keyword">int</span> n, <span class="algorithm-keyword">int</span>[][] edges) {
        entered = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[n];
        low = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[n];
        bridge = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">boolean</span>[edges.length];
        articulation = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">boolean</span>[n];
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> v = 0; v &lt; n; v++) graph.add(<span class="algorithm-keyword">new</span> ArrayList&lt;&gt;());
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> id = 0; id &lt; edges.length; id++) {
            <span class="algorithm-keyword">int</span> a = edges[id][0], b = edges[id][1];
            graph.get(a).add(<span class="algorithm-keyword">new</span> Edge(b, id));
            graph.get(b).add(<span class="algorithm-keyword">new</span> Edge(a, id));
        }
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> v = 0; v &lt; n; v++) {
            <span class="algorithm-keyword">if</span> (entered[v] == 0) dfs(v, -1);
        }
    }

    <span class="algorithm-keyword">void</span> dfs(<span class="algorithm-keyword">int</span> v, <span class="algorithm-keyword">int</span> parentEdge) {
        entered[v] = low[v] = ++timer;
        <span class="algorithm-keyword">int</span> children = 0;
        <span class="algorithm-keyword">for</span> (Edge edge : graph.get(v)) {
            <span class="algorithm-keyword">if</span> (edge.id == parentEdge) <span class="algorithm-keyword">continue</span>;    <span class="algorithm-comment">// 평행 간선을 부모 간선과 구별</span>
            <span class="algorithm-keyword">int</span> to = edge.to;
            <span class="algorithm-keyword">if</span> (entered[to] == 0) {
                children++;
                dfs(to, edge.id);
                low[v] = Math.min(low[v], low[to]);
                <span class="algorithm-keyword">if</span> (<mark class="algorithm-change">low[to] &gt; entered[v]</mark>) bridge[edge.id] = <span class="algorithm-keyword">true</span>;    <span class="algorithm-comment">// v로도 돌아올 수 없음</span>
                <span class="algorithm-keyword">if</span> (parentEdge != -1 &amp;&amp; <mark class="algorithm-change">low[to] &gt;= entered[v]</mark>) {
                    articulation[v] = <span class="algorithm-keyword">true</span>;    <span class="algorithm-comment">// v 위쪽으로 우회할 수 없음</span>
                }
            } <span class="algorithm-keyword">else</span> {
                low[v] = Math.min(low[v], entered[to]);
            }
        }
        <span class="algorithm-keyword">if</span> (parentEdge == -1 &amp;&amp; <mark class="algorithm-change">children &gt;= 2</mark>) articulation[v] = <span class="algorithm-keyword">true</span>;
    }
}</code></pre>

[[DFS]]
