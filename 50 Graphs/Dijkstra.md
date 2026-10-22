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
> **입력** · `graph[v]`는 `{to, weight}` 목록. 각 리스트는 미리 생성하고, 정점 번호는 배열 인덱스로 사용.
>
> **자료형** · 간선 가중치는 `int`, 누적 거리는 `long`. 힙에는 `{정점, 누적 거리}`를 저장.
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

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">long</span>[] dijkstra(List&lt;<span class="algorithm-keyword">int</span>[]&gt;[] graph, <span class="algorithm-keyword">int</span> start) {
    <span class="algorithm-keyword">long</span>[] dist = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[graph.length];
    Arrays.fill(dist, Long.MAX_VALUE);

    PriorityQueue&lt;<span class="algorithm-keyword">long</span>[]&gt; pq = <span class="algorithm-keyword">new</span> PriorityQueue&lt;&gt;(
            Comparator.comparingLong(a -&gt; a[1]));
    dist[start] = 0;
    pq.offer(<span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[]{start, 0});

    <span class="algorithm-keyword">while</span> (!pq.isEmpty()) {
        <span class="algorithm-keyword">long</span>[] cur = pq.poll();
        <span class="algorithm-keyword">int</span> v = (<span class="algorithm-keyword">int</span>) cur[0];
        <span class="algorithm-keyword">long</span> cost = cur[1];

        <span class="algorithm-keyword">if</span> (cost != dist[v]) {    <span class="algorithm-comment">// 갱신 전 후보도 힙에 남아 있음</span>
            <span class="algorithm-keyword">continue</span>;
        }

        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span>[] edge : graph[v]) {
            <span class="algorithm-keyword">int</span> to = edge[0];
            <span class="algorithm-keyword">long</span> next = cost + edge[1];

            <span class="algorithm-keyword">if</span> (next &lt; dist[to]) {    <span class="algorithm-comment">// 같은 거리도 넣으면 0 가중치 사이클에서 반복</span>
                dist[to] = next;
                pq.offer(<span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[]{to, next});
            }
        }
    }

    <span class="algorithm-keyword">return</span> dist;
}</code></pre>

#Graphs
