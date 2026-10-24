---
tags:
  - graphs
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Eulerian Trail

![Eulerian Trail memory cue](../attachments/reference-eulerian-trail.svg)

> [!reference] 개념과 사용 조건
> **목적** · 모든 간선을 정확히 한 번 사용하는 경로. 정점은 여러 번 방문 가능.
>
> **방향 조건** · out−in이 +1인 시작점과 −1인 끝점 각 하나, 나머지 0. 회로는 전부 0. 차수 외에 시작점에서 모든 간선을 소비했는지도 확인.
>
> **무방향 조건** · 홀수 차수 정점이 0개 또는 2개. 간선이 있는 정점들이 연결되어야 한다.
>
> **반환·비용** · 정점 순서 배열, 불가능하면 `null`. 빈 간선은 `[0]`(n=0이면 `[]`). `O(V+E)`.

---

## Directed

방향 그래프의 모든 간선을 정확히 한 번 사용하는 경로를 구한다.

`edges[i]={from,to}`. 각 인접 목록의 커서가 이미 사용한 간선을 건너뛴다.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// in/out 차수로 시작점과 가능 여부 결정</span>
<span class="algorithm-keyword">while</span> (스택에 정점이 있으면) {
    <span class="algorithm-keyword">if</span> (현재 정점에 남은 간선이 있으면) {
        <span class="algorithm-comment">// 간선 소비 → 다음 정점 push</span>
    } <span class="algorithm-keyword">else</span> {
        <span class="algorithm-comment">// pop한 정점을 답의 뒤부터 기록</span>
    }
}
<span class="algorithm-keyword">return</span> 모든 간선을 썼는지 확인;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span>[] directedEulerTrail(<span class="algorithm-keyword">int</span> n, <span class="algorithm-keyword">int</span>[][] edges) {
    <span class="algorithm-keyword">if</span> (n == 0) <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[0];
    List&lt;List&lt;Integer&gt;&gt; graph = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;(n);
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> v = 0; v &lt; n; v++) graph.add(<span class="algorithm-keyword">new</span> ArrayList&lt;&gt;());
    <span class="algorithm-keyword">int</span>[] in = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[n], out = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[n];
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span>[] row : edges) {
        graph.get(row[0]).add(row[1]);
        out[row[0]]++;
        in[row[1]]++;
    }
    <span class="algorithm-keyword">int</span> start = 0, plus = 0, minus = 0;
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> v = 0; v &lt; n; v++) {
        <span class="algorithm-keyword">if</span> (out[v] &gt; 0) start = v;
        <span class="algorithm-keyword">int</span> delta = out[v] - in[v];
        <span class="algorithm-keyword">if</span> (delta == 1) plus++;
        <span class="algorithm-keyword">else</span> <span class="algorithm-keyword">if</span> (delta == -1) minus++;
        <span class="algorithm-keyword">else</span> <span class="algorithm-keyword">if</span> (delta != 0) <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">null</span>;
    }
    <span class="algorithm-keyword">if</span> (!((plus == 0 &amp;&amp; minus == 0) || (plus == 1 &amp;&amp; minus == 1))) <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">null</span>;
    <span class="algorithm-keyword">if</span> (plus == 1) {
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> v = 0; v &lt; n; v++) {
            <span class="algorithm-keyword">if</span> (out[v] - in[v] == 1) start = v;
        }
    }

    <span class="algorithm-keyword">int</span>[] next = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[n], path = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[edges.length + 1];
    <span class="algorithm-keyword">int</span> index = edges.length;
    Deque&lt;Integer&gt; stack = <span class="algorithm-keyword">new</span> ArrayDeque&lt;&gt;();
    stack.push(start);
    <span class="algorithm-keyword">while</span> (!stack.isEmpty()) {
        <span class="algorithm-keyword">int</span> v = stack.peek();
        <span class="algorithm-keyword">if</span> (next[v] &lt; graph.get(v).size()) {
            stack.push(graph.get(v).get(next[v]++));
        } <span class="algorithm-keyword">else</span> {
            path[index--] = stack.pop();    <span class="algorithm-comment">// 막힌 곳부터 기록해야 경로가 이어짐</span>
        }
    }
    <span class="algorithm-keyword">return</span> index == -1 ? path : <span class="algorithm-keyword">null</span>;    <span class="algorithm-comment">// 차수 조건만 맞는 분리된 성분을 걸러냄</span>
}</code></pre>

---

## Undirected

무방향 그래프의 모든 간선을 정확히 한 번 사용하는 경로를 구한다.

`edges[i]={a,b}`. 같은 ID를 양쪽 인접 목록에 넣는다.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// 홀수 차수로 시작점과 가능 여부 결정</span>
<span class="algorithm-keyword">while</span> (스택에 정점이 있으면) {
    <span class="algorithm-comment">// 이미 사용한 간선 ID 건너뛰기</span>
    <span class="algorithm-keyword">if</span> (미사용 간선이 있으면) {
        <span class="algorithm-comment">// ID를 사용 처리 → 다음 정점 push</span>
    } <span class="algorithm-keyword">else</span> {
        <span class="algorithm-comment">// pop한 정점을 답의 뒤부터 기록</span>
    }
}
<span class="algorithm-keyword">return</span> 모든 간선을 썼는지 확인;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> Edge {
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span> to, id;
    Edge(<span class="algorithm-keyword">int</span> to, <span class="algorithm-keyword">int</span> id) {
        <span class="algorithm-keyword">this</span>.to = to;
        <span class="algorithm-keyword">this</span>.id = id;
    }
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span>[] undirectedEulerTrail(<span class="algorithm-keyword">int</span> n, <span class="algorithm-keyword">int</span>[][] edges) {
    <span class="algorithm-keyword">if</span> (n == 0) <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[0];
    List&lt;List&lt;Edge&gt;&gt; graph = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;(n);
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> v = 0; v &lt; n; v++) graph.add(<span class="algorithm-keyword">new</span> ArrayList&lt;&gt;());
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> id = 0; id &lt; edges.length; id++) {
        <span class="algorithm-keyword">int</span> a = edges[id][0], b = edges[id][1];
        graph.get(a).add(<span class="algorithm-keyword">new</span> Edge(b, id));
        graph.get(b).add(<span class="algorithm-keyword">new</span> Edge(a, id));
    }
    <span class="algorithm-keyword">int</span> start = 0, odd = 0;
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> v = 0; v &lt; n; v++) {
        <span class="algorithm-keyword">if</span> (!graph.get(v).isEmpty()) start = v;
        <span class="algorithm-keyword">if</span> (graph.get(v).size() % 2 == 1) odd++;
    }
    <span class="algorithm-keyword">if</span> (odd != 0 &amp;&amp; odd != 2) <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">null</span>;
    <span class="algorithm-keyword">if</span> (odd == 2) {
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> v = 0; v &lt; n; v++) {
            <span class="algorithm-keyword">if</span> (graph.get(v).size() % 2 == 1) { start = v; <span class="algorithm-keyword">break</span>; }
        }
    }

    <span class="algorithm-keyword">boolean</span>[] used = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">boolean</span>[edges.length];
    <span class="algorithm-keyword">int</span>[] next = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[n], path = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[edges.length + 1];
    <span class="algorithm-keyword">int</span> index = edges.length;
    Deque&lt;Integer&gt; stack = <span class="algorithm-keyword">new</span> ArrayDeque&lt;&gt;();
    stack.push(start);
    <span class="algorithm-keyword">while</span> (!stack.isEmpty()) {
        <span class="algorithm-keyword">int</span> v = stack.peek();
        <span class="algorithm-keyword">while</span> (next[v] &lt; graph.get(v).size()
                &amp;&amp; used[graph.get(v).get(next[v]).id]) next[v]++;
        <span class="algorithm-keyword">if</span> (next[v] == graph.get(v).size()) {
            path[index--] = stack.pop();
        } <span class="algorithm-keyword">else</span> {
            Edge edge = graph.get(v).get(next[v]++);
            used[edge.id] = <span class="algorithm-keyword">true</span>;    <span class="algorithm-comment">// 양방향 목록이 같은 간선 하나를 공유</span>
            stack.push(edge.to);
        }
    }
    <span class="algorithm-keyword">return</span> index == -1 ? path : <span class="algorithm-keyword">null</span>;
}</code></pre>

[[DFS]]
