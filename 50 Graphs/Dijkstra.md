---
cssclasses: [algorithm-reference-sample]
code-styler-ignore: true
type: template
tags: ["Graphs"]
category: shortest-paths
categories: [shortest-paths]
---

# Dijkstra

![Dijkstra의 Pop Skip Relax 흐름](../attachments/dijkstra-flow.svg)

> [!reference] 개념과 사용 조건
> **목적** · 한 시작점에서 모든 정점까지의 최단 거리.
>
> **조건** · 가중치 ≥ 0. 옛 후보를 제외하고 가장 가까운 정점을 꺼내면, 그 거리가 확정된다.
>
> **Edge → graph[v]** · `v → to` 간선 하나의 비용 `weight`. 정점 번호로 접근하며 각 리스트는 미리 생성.
>
> **State → pq** · `start → vertex`까지 도착한 후보의 누적 거리 `distance`.
>
> **반환** · `dist[v]`는 최단 거리. 도달 불가는 `Long.MAX_VALUE`.

---

## Template

**Structure**

<pre class="algorithm-code"><code><span class="algorithm-comment">// 시작 거리 = 0, 나머지 = ∞</span>
<span class="algorithm-comment">// 시작 후보를 힙에 삽입</span>

<span class="algorithm-keyword">while</span> (힙에 후보가 있으면) {
    <span class="algorithm-comment">// pop: 누적 거리가 가장 작은 후보</span>

    <span class="algorithm-keyword">if</span> (현재 기록과 다른 옛 후보면) {
        <span class="algorithm-keyword">continue</span>;
    }

    <span class="algorithm-keyword">for</span> (현재 정점에서 나가는 각 간선) {
        <span class="algorithm-comment">// next = 현재 거리 + 간선 가중치</span>

        <span class="algorithm-keyword">if</span> (next가 기존 거리보다 짧으면) {
            <span class="algorithm-comment">// 거리 갱신 → 새 후보 삽입</span>
        }
    }
}

<span class="algorithm-keyword">return</span> dist;</code></pre>

**Java**

<pre class="algorithm-code"><code><span class="algorithm-keyword">import</span> java.util.*;

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> Edge {
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span> to, weight;

    Edge(<span class="algorithm-keyword">int</span> to, <span class="algorithm-keyword">int</span> weight) {
        <span class="algorithm-keyword">this</span>.to = to;
        <span class="algorithm-keyword">this</span>.weight = weight;
    }
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> State {
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span> vertex;
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">long</span> distance;

    State(<span class="algorithm-keyword">int</span> vertex, <span class="algorithm-keyword">long</span> distance) {
        <span class="algorithm-keyword">this</span>.vertex = vertex;
        <span class="algorithm-keyword">this</span>.distance = distance;
    }
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">long</span>[] dijkstra(List&lt;Edge&gt;[] graph, <span class="algorithm-keyword">int</span> start) {
    <span class="algorithm-keyword">long</span>[] dist = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[graph.length];
    Arrays.fill(dist, Long.MAX_VALUE);

    PriorityQueue&lt;State&gt; pq = <span class="algorithm-keyword">new</span> PriorityQueue&lt;&gt;(
            Comparator.comparingLong(state -&gt; state.distance));
    dist[start] = 0;
    pq.offer(<span class="algorithm-keyword">new</span> State(start, 0));

    <span class="algorithm-keyword">while</span> (!pq.isEmpty()) {
        State cur = pq.poll();
        <span class="algorithm-keyword">int</span> v = cur.vertex;

        <span class="algorithm-keyword">if</span> (cur.distance != dist[v]) {    <span class="algorithm-comment">// 갱신 전 후보도 힙에 남아 있음</span>
            <span class="algorithm-keyword">continue</span>;
        }

        <span class="algorithm-keyword">for</span> (Edge edge : graph[v]) {
            <span class="algorithm-keyword">int</span> to = edge.to;
            <span class="algorithm-keyword">long</span> next = cur.distance + edge.weight;

            <span class="algorithm-keyword">if</span> (next &lt; dist[to]) {    <span class="algorithm-comment">// 같은 거리도 넣으면 0 가중치 사이클에서 반복</span>
                dist[to] = next;
                pq.offer(<span class="algorithm-keyword">new</span> State(to, next));
            }
        }
    }

    <span class="algorithm-keyword">return</span> dist;
}</code></pre>

---

## Graph setup

정점은 `0..n-1`, 입력 `edges[i]`는 `{from, to, weight}`, `start`는 시작 정점.

<pre class="algorithm-code"><code>@SuppressWarnings("unchecked")
List&lt;Edge&gt;[] graph = (List&lt;Edge&gt;[]) <span class="algorithm-keyword">new</span> List&lt;?&gt;[n];
<span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> v = 0; v &lt; n; v++) {
    graph[v] = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;();
}

<span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span>[] row : edges) {
    <span class="algorithm-keyword">int</span> from = row[0], to = row[1], weight = row[2];

    graph[from].add(<span class="algorithm-keyword">new</span> Edge(to, weight));
    <span class="algorithm-comment">// <mark class="algorithm-change">graph[to].add(new Edge(from, weight));</mark>    // 무방향이면 주석 해제</span>
}

<span class="algorithm-keyword">long</span>[] dist = dijkstra(graph, start);</code></pre>

#Graphs
