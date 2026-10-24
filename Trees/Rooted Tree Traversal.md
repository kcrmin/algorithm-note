---
tags:
  - trees
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Rooted Tree Traversal

![Rooted Tree Traversal memory cue](../attachments/reference-rooted-tree-traversal.svg)

> [!reference] 개념과 사용 조건
> **목적** · 루트를 기준으로 부모·깊이·처리 순서·서브트리 크기를 만든다.
>
> **조건** · n ≥ 1인 연결된 무방향 트리. `edges[i]={a,b}`, root는 `0..n-1`.
>
> **사용** · `RootedTree t = new RootedTree(n, edges, root);`. 루트의 parent는 `-1`, depth는 `0`.
>
> **비용** · 시간·공간 `O(V)`. order는 부모가 자식보다 앞에 있는 순서.

---

## Template

루트를 기준으로 각 정점의 부모·깊이·처리 순서·서브트리 크기를 구한다.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// root의 부모 = -1, 깊이 = 0</span>
<span class="algorithm-keyword">while</span> (큐가 비어 있지 않으면) {
    <span class="algorithm-comment">// 꺼낸 정점을 order에 기록</span>
    <span class="algorithm-keyword">for</span> (미방문 이웃) {
        <span class="algorithm-comment">// 부모·깊이 설정 → 큐에 삽입</span>
    }
}

<span class="algorithm-keyword">for</span> (order의 뒤에서 루트 직전까지) {
    <span class="algorithm-comment">// 부모의 size에 내 size 더하기</span>
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> RootedTree {
    <span class="algorithm-keyword">final</span> List&lt;List&lt;Integer&gt;&gt; graph = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;();
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span>[] parent, depth, order, size;

    RootedTree(<span class="algorithm-keyword">int</span> n, <span class="algorithm-keyword">int</span>[][] edges, <span class="algorithm-keyword">int</span> root) {
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> vertex = 0; vertex &lt; n; vertex++) graph.add(<span class="algorithm-keyword">new</span> ArrayList&lt;&gt;());
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span>[] row : edges) {
            graph.get(row[0]).add(row[1]);
            graph.get(row[1]).add(row[0]);
        }
        parent = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[n];
        depth = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[n];
        order = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[n];
        size = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[n];
        Arrays.fill(parent, -2);
        Arrays.fill(size, 1);
        Queue&lt;Integer&gt; queue = <span class="algorithm-keyword">new</span> ArrayDeque&lt;&gt;();
        parent[root] = -1;
        queue.offer(root);
        <span class="algorithm-keyword">int</span> count = 0;
        <span class="algorithm-keyword">while</span> (!queue.isEmpty()) {
            <span class="algorithm-keyword">int</span> vertex = queue.poll();
            order[count++] = vertex;
            <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> to : graph.get(vertex)) {
                <span class="algorithm-keyword">if</span> (parent[to] != -2) <span class="algorithm-keyword">continue</span>;
                parent[to] = vertex;
                depth[to] = depth[vertex] + 1;
                queue.offer(to);
            }
        }
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = n - 1; i &gt; 0; i--) {    <span class="algorithm-comment">// 자식의 결과를 부모보다 먼저 완성</span>
            <span class="algorithm-keyword">int</span> vertex = order[i];
            size[parent[vertex]] += size[vertex];
        }
    }
}</code></pre>

[[Tree DP]] · [[LCA and Binary Lifting]]
