---
tags:
  - graphs
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Kruskal

![Kruskal memory cue](../attachments/reference-kruskal.svg)

> [!reference] 개념과 사용 조건
> **목적** · 무방향 그래프의 최소 신장 트리. 음수 가중치도 가능.
>
> **입력** · 간선마다 `new Edge(a, b, weight)` 한 번만 추가. 정점 `0..n-1`.
>
> **반환** · 최소 비용 `Long`. 연결되지 않으면 `null`; 빈 그래프는 `0`.
>
> **비용** · `O(E log E)`. 평행 간선·자기 루프 허용.

---

## Template

무방향 그래프에서 모든 정점을 연결하는 최소 신장 트리의 비용을 구한다.

`List<Edge> edges = new ArrayList<>();`에 무방향 간선을 한 번씩 넣는다.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// 간선 비용으로 정렬, 집합 초기화</span>
<span class="algorithm-keyword">for</span> (싼 간선부터) {
    <span class="algorithm-keyword">if</span> (양끝이 이미 같은 집합이면) <span class="algorithm-keyword">continue</span>;
    <span class="algorithm-comment">// 합치기 → 비용 추가</span>
    <span class="algorithm-keyword">if</span> (V-1개를 선택했으면) <span class="algorithm-keyword">break</span>;
}
<span class="algorithm-keyword">return</span> 연결 여부와 총비용;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> Edge {
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span> from, to, weight;
    Edge(<span class="algorithm-keyword">int</span> from, <span class="algorithm-keyword">int</span> to, <span class="algorithm-keyword">int</span> weight) {
        <span class="algorithm-keyword">this</span>.from = from;
        <span class="algorithm-keyword">this</span>.to = to;
        <span class="algorithm-keyword">this</span>.weight = weight;
    }
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> UnionFind {
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span>[] parent, size;

    UnionFind(<span class="algorithm-keyword">int</span> n) {
        parent = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[n];
        size = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[n];
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> v = 0; v &lt; n; v++) {
            parent[v] = v;
            size[v] = 1;
        }
    }

    <span class="algorithm-keyword">int</span> find(<span class="algorithm-keyword">int</span> v) {
        <span class="algorithm-keyword">while</span> (v != parent[v]) {
            parent[v] = parent[parent[v]];    <span class="algorithm-comment">// 지나가는 경로를 짧게 만들어 다음 조회를 줄임</span>
            v = parent[v];
        }
        <span class="algorithm-keyword">return</span> v;
    }

    <span class="algorithm-keyword">boolean</span> union(<span class="algorithm-keyword">int</span> a, <span class="algorithm-keyword">int</span> b) {
        a = find(a);
        b = find(b);
        <span class="algorithm-keyword">if</span> (a == b) <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">false</span>;
        <span class="algorithm-keyword">if</span> (size[a] &lt; size[b]) {
            <span class="algorithm-keyword">int</span> temp = a;
            a = b;
            b = temp;
        }
        parent[b] = a;    <span class="algorithm-comment">// 작은 트리를 큰 트리 아래에 붙여 높이 억제</span>
        size[a] += size[b];
        <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">true</span>;
    }
}

<span class="algorithm-keyword">static</span> Long kruskal(<span class="algorithm-keyword">int</span> n, List&lt;Edge&gt; edges) {
    List&lt;Edge&gt; sorted = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;(edges);
    sorted.sort(Comparator.comparingInt(edge -&gt; edge.weight));
    UnionFind uf = <span class="algorithm-keyword">new</span> UnionFind(n);
    <span class="algorithm-keyword">long</span> total = 0;
    <span class="algorithm-keyword">int</span> used = 0;
    <span class="algorithm-keyword">for</span> (Edge edge : sorted) {
        <span class="algorithm-keyword">if</span> (!uf.union(edge.from, edge.to)) <span class="algorithm-keyword">continue</span>;    <span class="algorithm-comment">// 같은 집합을 연결하면 사이클</span>
        total += edge.weight;
        <span class="algorithm-keyword">if</span> (++used == n - 1) <span class="algorithm-keyword">break</span>;
    }
    <span class="algorithm-keyword">return</span> used == Math.max(0, n - 1) ? total : <span class="algorithm-keyword">null</span>;
}</code></pre>

[[Union-Find]]
