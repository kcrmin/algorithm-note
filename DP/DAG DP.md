---
tags:
  - dp
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# DAG DP

![DAG DP memory cue](../attachments/reference-dag-dp.svg)

> [!reference] 개념과 사용 조건
> **목적** · DAG의 시작점에서 각 정점까지 최장 거리 또는 경로 수.
>
> **조건** · 0-based 방향 그래프, 사이클 없음. start는 `0..n-1`. 사이클이 있으면 예외.
>
> **핵심** · 정점을 꺼낼 때 모든 선행 정점의 결과가 이미 반영되어 있다.
>
> **비용** · O(V + E) 시간·공간.

---

## Longest Path

`buildGraph(n, edges)`의 각 행은 `{from, to, weight}`. 음수 가중치 허용, 누적 비용은 long 범위. 도달 불가는 `Long.MIN_VALUE`.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// 진입 차수 계산, 0인 정점들을 큐에 삽입</span>
<span class="algorithm-comment">// start 거리 = 0, 나머지는 도달 불가</span>
<span class="algorithm-keyword">while</span> (큐가 비어 있지 않으면) {
    <span class="algorithm-comment">// 선행 계산이 끝난 정점 꺼내기</span>
    <span class="algorithm-keyword">for</span> (각 나가는 간선) {
        <span class="algorithm-keyword">if</span> (현재 정점에 도달 가능하면) {
            <span class="algorithm-comment">// 최장 거리 갱신</span>
        }
        <span class="algorithm-comment">// 다음 정점의 진입 차수 감소, 0이면 큐에 삽입</span>
    }
}
<span class="algorithm-comment">// 전부 처리하지 못했으면 사이클</span>
<span class="algorithm-keyword">return</span> dist;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">import</span> java.util.*;

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> Edge {
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span> to, weight;

    Edge(<span class="algorithm-keyword">int</span> to, <span class="algorithm-keyword">int</span> weight) {
        <span class="algorithm-keyword">this</span>.to = to;
        <span class="algorithm-keyword">this</span>.weight = weight;
    }
}

<span class="algorithm-keyword">static</span> List&lt;List&lt;Edge&gt;&gt; buildGraph(<span class="algorithm-keyword">int</span> n, <span class="algorithm-keyword">int</span>[][] edges) {
    List&lt;List&lt;Edge&gt;&gt; graph = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;(n);
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> v = 0; v &lt; n; v++) {
        graph.add(<span class="algorithm-keyword">new</span> ArrayList&lt;&gt;());
    }
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span>[] edge : edges) {
        graph.get(edge[0]).add(<span class="algorithm-keyword">new</span> Edge(edge[1], edge[2]));
    }
    <span class="algorithm-keyword">return</span> graph;
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">long</span>[] longestPaths(List&lt;List&lt;Edge&gt;&gt; graph, <span class="algorithm-keyword">int</span> start) {
    <span class="algorithm-keyword">int</span> n = graph.size();
    <span class="algorithm-keyword">int</span>[] indegree = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[n];
    <span class="algorithm-keyword">for</span> (List&lt;Edge&gt; edges : graph) {
        <span class="algorithm-keyword">for</span> (Edge edge : edges) {
            indegree[edge.to]++;
        }
    }
    ArrayDeque&lt;Integer&gt; queue = <span class="algorithm-keyword">new</span> ArrayDeque&lt;&gt;();
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> v = 0; v &lt; n; v++) {
        <span class="algorithm-keyword">if</span> (indegree[v] == 0) {
            queue.offer(v);
        }
    }
    <span class="algorithm-keyword">long</span>[] dist = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[n];
    Arrays.fill(dist, Long.MIN_VALUE);
    dist[start] = 0;
    <span class="algorithm-keyword">int</span> processed = 0;

    <span class="algorithm-keyword">while</span> (!queue.isEmpty()) {
        <span class="algorithm-keyword">int</span> v = queue.poll();
        processed++;
        <span class="algorithm-keyword">for</span> (Edge edge : graph.get(v)) {
            <span class="algorithm-keyword">if</span> (dist[v] != Long.MIN_VALUE) {    <span class="algorithm-comment">// 시작점에서 닿지 않는 정점은 비용을 전달하지 않음</span>
                dist[edge.to] = Math.max(dist[edge.to], dist[v] + edge.weight);
            }
            <span class="algorithm-keyword">if</span> (--indegree[edge.to] == 0) {
                queue.offer(edge.to);
            }
        }
    }

    <span class="algorithm-keyword">if</span> (processed != n) {
        <span class="algorithm-keyword">throw</span> <span class="algorithm-keyword">new</span> IllegalArgumentException("Graph has a cycle");
    }
    <span class="algorithm-keyword">return</span> dist;
}</code></pre>

---

## Path Count

`buildDirectedGraph(n, edges)`의 각 행은 `{from, to}`. `mod ≥ 2`. 평행 간선은 서로 다른 경로로 센다.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// 진입 차수 계산, 0인 정점들을 큐에 삽입</span>
<span class="algorithm-comment">// start의 경로 수 = 1, 나머지 = 0</span>
<span class="algorithm-keyword">while</span> (큐가 비어 있지 않으면) {
    <span class="algorithm-comment">// 선행 계산이 끝난 정점 꺼내기</span>
    <span class="algorithm-keyword">for</span> (각 나가는 간선) {
        <span class="algorithm-comment">// 현재 경로 수를 다음 정점에 더하기</span>
        <span class="algorithm-comment">// 다음 정점의 진입 차수 감소, 0이면 큐에 삽입</span>
    }
}
<span class="algorithm-comment">// 전부 처리하지 못했으면 사이클</span>
<span class="algorithm-keyword">return</span> count;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">import</span> java.util.*;

<span class="algorithm-keyword">static</span> List&lt;List&lt;Integer&gt;&gt; buildDirectedGraph(<span class="algorithm-keyword">int</span> n, <span class="algorithm-keyword">int</span>[][] edges) {
    List&lt;List&lt;Integer&gt;&gt; graph = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;(n);
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> v = 0; v &lt; n; v++) {
        graph.add(<span class="algorithm-keyword">new</span> ArrayList&lt;&gt;());
    }
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span>[] edge : edges) {
        graph.get(edge[0]).add(edge[1]);
    }
    <span class="algorithm-keyword">return</span> graph;
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">long</span>[] countDagPaths(List&lt;List&lt;Integer&gt;&gt; graph, <span class="algorithm-keyword">int</span> start, <span class="algorithm-keyword">int</span> mod) {
    <span class="algorithm-keyword">int</span> n = graph.size();
    <span class="algorithm-keyword">int</span>[] indegree = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[n];
    <span class="algorithm-keyword">for</span> (List&lt;Integer&gt; edges : graph) {
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> to : edges) {
            indegree[to]++;
        }
    }
    ArrayDeque&lt;Integer&gt; queue = <span class="algorithm-keyword">new</span> ArrayDeque&lt;&gt;();
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> v = 0; v &lt; n; v++) {
        <span class="algorithm-keyword">if</span> (indegree[v] == 0) {
            queue.offer(v);
        }
    }
    <span class="algorithm-keyword">long</span>[] count = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[n];
    count[start] = 1;    <span class="algorithm-comment">// 출발점에 도착하는 빈 경로 한 가지</span>
    <span class="algorithm-keyword">int</span> processed = 0;

    <span class="algorithm-keyword">while</span> (!queue.isEmpty()) {
        <span class="algorithm-keyword">int</span> v = queue.poll();
        processed++;
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> to : graph.get(v)) {
            count[to] = (count[to] + count[v]) % mod;
            <span class="algorithm-keyword">if</span> (--indegree[to] == 0) {
                queue.offer(to);
            }
        }
    }

    <span class="algorithm-keyword">if</span> (processed != n) {
        <span class="algorithm-keyword">throw</span> <span class="algorithm-keyword">new</span> IllegalArgumentException("Graph has a cycle");
    }
    <span class="algorithm-keyword">return</span> count;
}</code></pre>

[[Topological Sort]]
