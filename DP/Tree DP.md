---
tags:
  - dp
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Tree DP

![Tree DP memory cue](../attachments/reference-tree-dp.svg)

> [!reference] 개념과 사용 조건
> **목적** · 서로 인접한 정점을 함께 고르지 않는 최대 가중치 독립 집합.
>
> **상태** · `take[vertex]` = vertex 선택, `skip[vertex]` = vertex 미선택일 때 서브트리의 최대 합.
>
> **조건** · 연결된 무방향 트리, 0-based. 음수 가중치 허용, 아무 정점도 안 골라도 된다. 가중치 합은 long 범위.
>
> **비용** · O(n) 시간·공간. 반복 순회로 깊은 트리의 재귀 stack 초과를 피한다.

---

## Maximum Weight Independent Set

서로 인접한 정점을 함께 고르지 않으면서 얻을 수 있는 최대 가중치 합을 구한다.

`buildTree(n, edges)`의 각 행은 `{from, to}`. weight 길이는 정점 수와 같다.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// root부터 부모와 방문 순서 기록</span>
<span class="algorithm-keyword">for</span> (방문 순서를 뒤집어서 각 정점 vertex) {
    <span class="algorithm-comment">// take[vertex]는 자기 가중치로 시작</span>
    <span class="algorithm-keyword">for</span> (각 자식) {
        <span class="algorithm-comment">// vertex를 고르면 자식은 skip</span>
        <span class="algorithm-comment">// vertex를 안 고르면 자식은 take / skip 중 큰 값</span>
    }
}
<span class="algorithm-keyword">return</span> root를 선택 / 미선택 중 큰 값;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">import</span> java.util.*;

<span class="algorithm-keyword">static</span> List&lt;List&lt;Integer&gt;&gt; buildTree(<span class="algorithm-keyword">int</span> n, <span class="algorithm-keyword">int</span>[][] edges) {
    List&lt;List&lt;Integer&gt;&gt; graph = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;(n);
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> vertex = 0; vertex &lt; n; vertex++) {
        graph.add(<span class="algorithm-keyword">new</span> ArrayList&lt;&gt;());
    }
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span>[] edge : edges) {
        graph.get(edge[0]).add(edge[1]);
        graph.get(edge[1]).add(edge[0]);    <span class="algorithm-comment">// 무방향 트리라 양쪽에서 접근</span>
    }
    <span class="algorithm-keyword">return</span> graph;
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">long</span> maxIndependentSet(List&lt;List&lt;Integer&gt;&gt; graph, <span class="algorithm-keyword">long</span>[] weight, <span class="algorithm-keyword">int</span> root) {
    <span class="algorithm-keyword">int</span> n = graph.size();
    <span class="algorithm-keyword">if</span> (n == 0) {
        <span class="algorithm-keyword">return</span> 0;
    }
    <span class="algorithm-keyword">int</span>[] parent = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[n], order = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[n];
    Arrays.fill(parent, -1);
    parent[root] = root;
    order[0] = root;
    <span class="algorithm-keyword">int</span> count = 1;

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = 0; i &lt; count; i++) {
        <span class="algorithm-keyword">int</span> vertex = order[i];
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> to : graph.get(vertex)) {
            <span class="algorithm-keyword">if</span> (to != parent[vertex]) {
                parent[to] = vertex;
                order[count++] = to;
            }
        }
    }

    <span class="algorithm-keyword">long</span>[] take = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[n], skip = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[n];
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = n - 1; i &gt;= 0; i--) {    <span class="algorithm-comment">// 부모보다 뒤에 방문한 자식을 먼저 계산</span>
        <span class="algorithm-keyword">int</span> vertex = order[i];
        take[vertex] = weight[vertex];

        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> to : graph.get(vertex)) {
            <span class="algorithm-keyword">if</span> (to != parent[vertex]) {
                take[vertex] += skip[to];
                skip[vertex] += Math.max(take[to], skip[to]);
            }
        }
    }

    <span class="algorithm-keyword">return</span> Math.max(take[root], skip[root]);
}</code></pre>

[[Rooted Tree Traversal]]
