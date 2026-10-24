---
tags:
  - trees
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# LCA and Binary Lifting

![LCA and Binary Lifting memory cue](../attachments/reference-lca-and-binary-lifting.svg)

> [!reference] 개념과 사용 조건
> **목적** · 최소 공통 조상, k번째 조상, 두 정점 사이 간선 수.
>
> **조건** · n ≥ 1인 연결된 무방향 트리. `edges[i]={a,b}`.
>
> **사용** · `LCA t = new LCA(n, edges, root);` → `query(a,b)`, `ancestor(vertex,k)`, `distance(a,b)`.
>
> **비용·반환** · 전처리 `O(V log V)`, 질의 `O(log V)`. 존재하지 않는 k번째 조상은 `-1`.

---

## Template

최소 공통 조상, k번째 조상, 두 정점 사이의 간선 수를 구한다.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// BFS로 깊이와 바로 위 부모 만들기</span>
<span class="algorithm-keyword">for</span> (각 점프 단계 k) {
    <span class="algorithm-comment">// 2^(k-1)번 위의 조상에서 다시 2^(k-1)번 이동</span>
}

<span class="algorithm-keyword">int</span> query(a, b) {
    <span class="algorithm-comment">// 깊은 쪽을 끌어올려 깊이 맞추기</span>
    <span class="algorithm-keyword">if</span> (같은 정점이면) <span class="algorithm-keyword">return</span> a;
    <span class="algorithm-keyword">for</span> (큰 점프부터) {
        <span class="algorithm-keyword">if</span> (점프한 조상이 서로 다르면) { <span class="algorithm-comment">/* 둘 다 위로 이동 */</span> }
    }
    <span class="algorithm-keyword">return</span> 바로 위 부모;
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> LCA {
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span>[][] up;
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span>[] depth;
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span> log;

    LCA(<span class="algorithm-keyword">int</span> n, <span class="algorithm-keyword">int</span>[][] edges, <span class="algorithm-keyword">int</span> root) {
        <span class="algorithm-keyword">int</span> levels = 1;
        <span class="algorithm-keyword">while</span> ((1L &lt;&lt; levels) &lt;= n) levels++;
        log = levels;
        up = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[log][n];
        depth = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[n];
        Arrays.fill(depth, -1);
        List&lt;List&lt;Integer&gt;&gt; graph = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;(n);
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> vertex = 0; vertex &lt; n; vertex++) graph.add(<span class="algorithm-keyword">new</span> ArrayList&lt;&gt;());
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span>[] row : edges) {
            graph.get(row[0]).add(row[1]);
            graph.get(row[1]).add(row[0]);
        }
        Queue&lt;Integer&gt; queue = <span class="algorithm-keyword">new</span> ArrayDeque&lt;&gt;();
        up[0][root] = root;    <span class="algorithm-comment">// 루트 위로 올라도 배열 범위를 벗어나지 않음</span>
        depth[root] = 0;
        queue.offer(root);
        <span class="algorithm-keyword">while</span> (!queue.isEmpty()) {
            <span class="algorithm-keyword">int</span> vertex = queue.poll();
            <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> to : graph.get(vertex)) {
                <span class="algorithm-keyword">if</span> (depth[to] != -1) <span class="algorithm-keyword">continue</span>;
                depth[to] = depth[vertex] + 1;
                up[0][to] = vertex;
                queue.offer(to);
            }
        }
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> k = 1; k &lt; log; k++) {
            <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> vertex = 0; vertex &lt; n; vertex++) {
                up[k][vertex] = up[k - 1][up[k - 1][vertex]];
            }
        }
    }

    <span class="algorithm-keyword">int</span> ancestor(<span class="algorithm-keyword">int</span> vertex, <span class="algorithm-keyword">int</span> distance) {
        <span class="algorithm-keyword">if</span> (distance &lt; 0 || distance &gt; depth[vertex]) <span class="algorithm-keyword">return</span> -1;
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> k = 0; k &lt; log; k++) {
            <span class="algorithm-keyword">if</span> ((distance &amp; (1 &lt;&lt; k)) != 0) vertex = up[k][vertex];
        }
        <span class="algorithm-keyword">return</span> vertex;
    }

    <span class="algorithm-keyword">int</span> query(<span class="algorithm-keyword">int</span> a, <span class="algorithm-keyword">int</span> b) {
        <span class="algorithm-keyword">if</span> (depth[a] &lt; depth[b]) {
            <span class="algorithm-keyword">int</span> temp = a;
            a = b;
            b = temp;
        }
        a = ancestor(a, depth[a] - depth[b]);
        <span class="algorithm-keyword">if</span> (a == b) <span class="algorithm-keyword">return</span> a;
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> k = log - 1; k &gt;= 0; k--) {
            <span class="algorithm-keyword">if</span> (up[k][a] != up[k][b]) {    <span class="algorithm-comment">// 조상이 달라야 LCA 아래에 머무름</span>
                a = up[k][a];
                b = up[k][b];
            }
        }
        <span class="algorithm-keyword">return</span> up[0][a];
    }

    <span class="algorithm-keyword">int</span> distance(<span class="algorithm-keyword">int</span> a, <span class="algorithm-keyword">int</span> b) {
        <span class="algorithm-keyword">int</span> common = query(a, b);
        <span class="algorithm-keyword">return</span> (depth[a] - depth[common]) + (depth[b] - depth[common]);
    }
}</code></pre>

[[Rooted Tree Traversal]]
