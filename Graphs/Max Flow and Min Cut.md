---
tags:
  - graphs
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Max Flow and Min Cut

![Max Flow and Min Cut memory cue](../attachments/reference-max-flow-and-min-cut.svg)

> [!reference] 개념과 사용 조건
> **목적** · 용량 제한 안에서 source→sink 최대 유량과 최소 절단.
>
> **입력** · `Dinic flow = new Dinic(n);` 후 `flow.addEdge(from, to, capacity)`. 용량 ≥ 0, source ≠ sink.
>
> **반환** · `maxFlow`가 잔여 용량을 변경한다. 완료 후 `sourceSide(source)`의 true/false 경계가 최소 절단.
>
> **비용·조건** · 일반 그래프 `O(V²E)`. 총유량은 `long` 범위. push는 재귀이며, 깊은 레벨 경로에서는 `StackOverflowError` 가능.

---

## Dinic

`long answer = flow.maxFlow(source, sink);` 후 `boolean[] side = flow.sourceSide(source);`. 원본 간선 중 `side[from] && !side[to]`인 간선이 절단을 이룬다.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-keyword">boolean</span> buildLevel(source, sink) {
    <span class="algorithm-comment">// 잔여 용량 &gt; 0인 간선으로 BFS</span>
}

<span class="algorithm-keyword">long</span> push(v, sink, limit) {
    <span class="algorithm-keyword">if</span> (sink에 도착했으면) <span class="algorithm-keyword">return</span> limit;
    <span class="algorithm-keyword">for</span> (아직 확인할 간선) {
        <span class="algorithm-keyword">if</span> (잔여 용량이 없거나 다음 레벨이 아니면) <span class="algorithm-keyword">continue</span>;
        <span class="algorithm-comment">// 재귀로 보낼 양 결정</span>
        <span class="algorithm-keyword">if</span> (보냈으면) { <span class="algorithm-comment">/* 정방향 감소, 역방향 증가 */</span> <span class="algorithm-keyword">return</span> 보낸 양; }
    }
    <span class="algorithm-keyword">return</span> 0;
}

<span class="algorithm-keyword">while</span> (sink까지 레벨이 만들어지면) {
    <span class="algorithm-comment">// 간선 탐색 위치 초기화</span>
    <span class="algorithm-keyword">while</span> (더 보낼 수 있으면) { <span class="algorithm-comment">/* 총유량에 추가 */</span> }
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> Edge {
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span> to, reverse;
    <span class="algorithm-keyword">long</span> capacity;

    Edge(<span class="algorithm-keyword">int</span> to, <span class="algorithm-keyword">int</span> reverse, <span class="algorithm-keyword">long</span> capacity) {
        <span class="algorithm-keyword">this</span>.to = to;
        <span class="algorithm-keyword">this</span>.reverse = reverse;
        <span class="algorithm-keyword">this</span>.capacity = capacity;
    }
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> Dinic {
    <span class="algorithm-keyword">final</span> List&lt;List&lt;Edge&gt;&gt; graph = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;();
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span>[] level, next;

    Dinic(<span class="algorithm-keyword">int</span> n) {
        level = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[n];
        next = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[n];
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> v = 0; v &lt; n; v++) graph.add(<span class="algorithm-keyword">new</span> ArrayList&lt;&gt;());
    }

    <span class="algorithm-keyword">void</span> addEdge(<span class="algorithm-keyword">int</span> from, <span class="algorithm-keyword">int</span> to, <span class="algorithm-keyword">long</span> capacity) {
        <span class="algorithm-keyword">if</span> (from == to) <span class="algorithm-keyword">return</span>;    <span class="algorithm-comment">// 자기 루프는 s→t 유량을 늘리지 않음</span>
        Edge forward = <span class="algorithm-keyword">new</span> Edge(to, graph.get(to).size(), capacity);
        Edge backward = <span class="algorithm-keyword">new</span> Edge(from, graph.get(from).size(), 0);
        graph.get(from).add(forward);
        graph.get(to).add(backward);
    }

    <span class="algorithm-keyword">boolean</span> buildLevel(<span class="algorithm-keyword">int</span> source, <span class="algorithm-keyword">int</span> sink) {
        Arrays.fill(level, -1);
        Queue&lt;Integer&gt; queue = <span class="algorithm-keyword">new</span> ArrayDeque&lt;&gt;();
        level[source] = 0;
        queue.offer(source);
        <span class="algorithm-keyword">while</span> (!queue.isEmpty()) {
            <span class="algorithm-keyword">int</span> v = queue.poll();
            <span class="algorithm-keyword">for</span> (Edge edge : graph.get(v)) {
                <span class="algorithm-keyword">if</span> (edge.capacity &lt;= 0 || level[edge.to] != -1) <span class="algorithm-keyword">continue</span>;
                level[edge.to] = level[v] + 1;
                queue.offer(edge.to);
            }
        }
        <span class="algorithm-keyword">return</span> level[sink] != -1;
    }

    <span class="algorithm-keyword">long</span> push(<span class="algorithm-keyword">int</span> v, <span class="algorithm-keyword">int</span> sink, <span class="algorithm-keyword">long</span> limit) {
        <span class="algorithm-keyword">if</span> (v == sink) <span class="algorithm-keyword">return</span> limit;
        <span class="algorithm-keyword">for</span> (; next[v] &lt; graph.get(v).size(); next[v]++) {
            Edge edge = graph.get(v).get(next[v]);
            <span class="algorithm-keyword">if</span> (edge.capacity &lt;= 0 || level[edge.to] != level[v] + 1) <span class="algorithm-keyword">continue</span>;
            <span class="algorithm-keyword">long</span> sent = push(edge.to, sink, Math.min(limit, edge.capacity));
            <span class="algorithm-keyword">if</span> (sent == 0) <span class="algorithm-keyword">continue</span>;
            edge.capacity -= sent;
            graph.get(edge.to).get(edge.reverse).capacity += sent;    <span class="algorithm-comment">// 나중에 선택을 되돌릴 통로</span>
            <span class="algorithm-keyword">return</span> sent;
        }
        <span class="algorithm-keyword">return</span> 0;
    }

    <span class="algorithm-keyword">long</span> maxFlow(<span class="algorithm-keyword">int</span> source, <span class="algorithm-keyword">int</span> sink) {
        <span class="algorithm-keyword">if</span> (source == sink) <span class="algorithm-keyword">throw</span> <span class="algorithm-keyword">new</span> IllegalArgumentException("source == sink");
        <span class="algorithm-keyword">long</span> total = 0;
        <span class="algorithm-keyword">while</span> (buildLevel(source, sink)) {
            Arrays.fill(next, 0);    <span class="algorithm-comment">// 새 레벨 그래프에서는 간선을 다시 확인</span>
            <span class="algorithm-keyword">long</span> sent;
            <span class="algorithm-keyword">while</span> ((sent = push(source, sink, Long.MAX_VALUE)) &gt; 0) {
                total += sent;
            }
        }
        <span class="algorithm-keyword">return</span> total;
    }

    <span class="algorithm-keyword">boolean</span>[] sourceSide(<span class="algorithm-keyword">int</span> source) {
        <span class="algorithm-keyword">boolean</span>[] reached = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">boolean</span>[graph.size()];
        Queue&lt;Integer&gt; queue = <span class="algorithm-keyword">new</span> ArrayDeque&lt;&gt;();
        reached[source] = <span class="algorithm-keyword">true</span>;
        queue.offer(source);
        <span class="algorithm-keyword">while</span> (!queue.isEmpty()) {
            <span class="algorithm-keyword">int</span> v = queue.poll();
            <span class="algorithm-keyword">for</span> (Edge edge : graph.get(v)) {
                <span class="algorithm-keyword">if</span> (edge.capacity &lt;= 0 || reached[edge.to]) <span class="algorithm-keyword">continue</span>;
                reached[edge.to] = <span class="algorithm-keyword">true</span>;
                queue.offer(edge.to);
            }
        }
        <span class="algorithm-keyword">return</span> reached;
    }
}</code></pre>

[[Bipartite Matching]] · [[BFS]]
