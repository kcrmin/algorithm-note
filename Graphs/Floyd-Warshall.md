---
tags:
  - graphs
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Floyd-Warshall

![Floyd-Warshall memory cue](../attachments/reference-floyd-warshall.svg)

> [!reference] 개념과 사용 조건
> **목적** · 모든 정점 쌍의 최단 거리. 음수 간선은 허용, 음수 사이클은 제외.
>
> **입력** · 방향 간선 `new Edge(from, to, weight)`. 무방향이면 역방향 간선도 추가.
>
> **반환** · 도달 불가는 `Long.MAX_VALUE / 4`. 경로 비용의 절댓값은 이 값 미만.
>
> **비용** · 시간 `O(V³)`, 공간 `O(V²)`.

---

## Template

모든 정점 쌍 사이의 최단 거리를 구한다.

`List<Edge> edges = new ArrayList<>();` 후 `edges.add(new Edge(from, to, weight));`.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// 자기 거리 = 0, 나머지 = ∞, 직접 간선 반영</span>
<span class="algorithm-keyword">for</span> (각 via) {
    <span class="algorithm-keyword">for</span> (각 from) {
        <span class="algorithm-keyword">for</span> (각 to) {
            <span class="algorithm-keyword">if</span> (양쪽 경로가 있으면) {
                <span class="algorithm-comment">// 직접 거리와 via 경유 거리 중 최소</span>
            }
        }
    }
}
<span class="algorithm-keyword">return</span> dist;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> Edge {
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span> from, to, weight;
    Edge(<span class="algorithm-keyword">int</span> from, <span class="algorithm-keyword">int</span> to, <span class="algorithm-keyword">int</span> weight) {
        <span class="algorithm-keyword">this</span>.from = from;
        <span class="algorithm-keyword">this</span>.to = to;
        <span class="algorithm-keyword">this</span>.weight = weight;
    }
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">long</span>[][] floydWarshall(<span class="algorithm-keyword">int</span> n, List&lt;Edge&gt; edges) {
    <span class="algorithm-keyword">long</span> inf = Long.MAX_VALUE / 4;
    <span class="algorithm-keyword">long</span>[][] dist = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[n][n];
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> vertex = 0; vertex &lt; n; vertex++) {
        Arrays.fill(dist[vertex], inf);
        dist[vertex][vertex] = 0;
    }
    <span class="algorithm-keyword">for</span> (Edge edge : edges) {
        dist[edge.from][edge.to] = Math.min(dist[edge.from][edge.to], edge.weight);
    }

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> via = 0; via &lt; n; via++) {    <span class="algorithm-comment">// 허용할 중간 정점을 바깥에서 하나씩 늘림</span>
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> from = 0; from &lt; n; from++) {
            <span class="algorithm-keyword">if</span> (dist[from][via] == inf) <span class="algorithm-keyword">continue</span>;
            <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> to = 0; to &lt; n; to++) {
                <span class="algorithm-keyword">if</span> (dist[via][to] == inf) <span class="algorithm-keyword">continue</span>;    <span class="algorithm-comment">// 도달 불가 값에 음수를 더하지 않음</span>
                dist[from][to] = Math.min(dist[from][to],
                        dist[from][via] + dist[via][to]);
            }
        }
    }
    <span class="algorithm-keyword">return</span> dist;
}</code></pre>

[[Dijkstra]] · [[Bellman-Ford]]
