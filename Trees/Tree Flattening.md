---
tags:
  - trees
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Tree Flattening

![Tree Flattening memory cue](../attachments/reference-tree-flattening.svg)

> [!reference] 개념과 사용 조건
> **목적** · 트리의 서브트리를 배열의 연속 구간으로 바꾼다.
>
> **조건** · n ≥ 1인 연결된 무방향 트리. `edges[i]={a,b}`, 정점 `0..n-1`.
>
> **사용** · `new TreeFlattening(n, edges, root)`. v의 서브트리는 `[tin[v], tout[v])`, 크기는 `tout[v]-tin[v]`.
>
> **연결·비용** · `flat[tin[v]]=value[v]`로 값 배치 후 구간 자료구조 사용. 시간·공간 `O(V)`.

---

## Entry / Exit

각 서브트리를 배열의 연속 구간으로 바꾸고 조상 관계를 판정한다.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-keyword">while</span> (스택에 frame이 있으면) {
    <span class="algorithm-comment">// frame 꺼내기</span>
    <span class="algorithm-keyword">if</span> (종료 frame이면) {
        <span class="algorithm-comment">// tout = 현재 timer</span>
        <span class="algorithm-keyword">continue</span>;
    }
    <span class="algorithm-comment">// tin과 order 기록 → timer 증가</span>
    <span class="algorithm-comment">// 내 종료 frame을 먼저 넣기</span>
    <span class="algorithm-keyword">for</span> (자식들을 역순으로) {
        <span class="algorithm-comment">// 자식 진입 frame 넣기</span>
    }
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> Frame {
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span> vertex, parent;
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">boolean</span> exit;
    Frame(<span class="algorithm-keyword">int</span> vertex, <span class="algorithm-keyword">int</span> parent, <span class="algorithm-keyword">boolean</span> exit) {
        <span class="algorithm-keyword">this</span>.vertex = vertex;
        <span class="algorithm-keyword">this</span>.parent = parent;
        <span class="algorithm-keyword">this</span>.exit = exit;
    }
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> TreeFlattening {
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span>[] tin, tout, order;

    TreeFlattening(<span class="algorithm-keyword">int</span> n, <span class="algorithm-keyword">int</span>[][] edges, <span class="algorithm-keyword">int</span> root) {
        List&lt;List&lt;Integer&gt;&gt; graph = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;(n);
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> v = 0; v &lt; n; v++) graph.add(<span class="algorithm-keyword">new</span> ArrayList&lt;&gt;());
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span>[] row : edges) {
            graph.get(row[0]).add(row[1]);
            graph.get(row[1]).add(row[0]);
        }
        tin = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[n];
        tout = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[n];
        order = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[n];
        <span class="algorithm-keyword">int</span> timer = 0;
        Deque&lt;Frame&gt; stack = <span class="algorithm-keyword">new</span> ArrayDeque&lt;&gt;();
        stack.push(<span class="algorithm-keyword">new</span> Frame(root, -1, <span class="algorithm-keyword">false</span>));
        <span class="algorithm-keyword">while</span> (!stack.isEmpty()) {
            Frame cur = stack.pop();
            <span class="algorithm-keyword">int</span> v = cur.vertex;
            <span class="algorithm-keyword">if</span> (cur.exit) {
                tout[v] = timer;    <span class="algorithm-comment">// 자식 전체가 끝난 직후의 위치: 오른쪽 끝 제외</span>
                <span class="algorithm-keyword">continue</span>;
            }
            tin[v] = timer;
            order[timer++] = v;
            stack.push(<span class="algorithm-keyword">new</span> Frame(v, cur.parent, <span class="algorithm-keyword">true</span>));
            List&lt;Integer&gt; children = graph.get(v);
            <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = children.size() - 1; i &gt;= 0; i--) {
                <span class="algorithm-keyword">int</span> to = children.get(i);
                <span class="algorithm-keyword">if</span> (to != cur.parent) stack.push(<span class="algorithm-keyword">new</span> Frame(to, v, <span class="algorithm-keyword">false</span>));
            }
        }
    }

    <span class="algorithm-keyword">boolean</span> isAncestor(<span class="algorithm-keyword">int</span> a, <span class="algorithm-keyword">int</span> b) {
        <span class="algorithm-keyword">return</span> tin[a] &lt;= tin[b] &amp;&amp; tin[b] &lt; tout[a];
    }
}</code></pre>

[[Fenwick Tree]] · [[Segment Tree]] · [[Rooted Tree Traversal]]
