---
tags:
  - graphs
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# 2-SAT

![2-SAT memory cue](../attachments/reference-2-sat.svg)

> [!reference] 개념과 사용 조건
> **목적** · 두 literal의 OR로 된 조건들을 모두 만족하는 참·거짓 배정.
>
> **표현** · 변수 `v`는 참 `2*v`, 거짓 `2*v+1`. 반대 literal은 `^1`.
>
> **사용** · `new TwoSat(n)` → `addClause(a, aTrue, b, bTrue)` → `solve()`. 불가능하면 `null`.
>
> **비용·조건** · 변수 n, 절 m일 때 `O(n+m)`. 두 SCC 탐색 모두 명시적 스택 사용.

---

## Template

두 literal의 OR로 된 조건들을 모두 만족하는 참·거짓 배정을 구한다.

`(x0 OR NOT x1)`은 `addClause(0, true, 1, false)`. `x0`을 참으로 고정하려면 같은 literal을 두 번 넣는다.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-keyword">void</span> addClause(a, b) {
    <span class="algorithm-comment">// NOT a → b, NOT b → a</span>
}

<span class="algorithm-keyword">boolean</span>[] solve() {
    <span class="algorithm-comment">// 원본을 스택 DFS: 간선 커서 보존, pop할 때 완료 순서 기록</span>
    <span class="algorithm-comment">// 완료 순서의 역순으로 역방향 스택 DFS: SCC 배정</span>
    <span class="algorithm-keyword">for</span> (각 변수) {
        <span class="algorithm-keyword">if</span> (참과 거짓이 같은 SCC면) <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">null</span>;
        <span class="algorithm-comment">// SCC의 위상 순서로 참/거짓 선택</span>
    }
    <span class="algorithm-keyword">return</span> result;
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> TwoSat {
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span> n;
    <span class="algorithm-keyword">final</span> List&lt;List&lt;Integer&gt;&gt; graph = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;();
    <span class="algorithm-keyword">final</span> List&lt;List&lt;Integer&gt;&gt; reverse = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;();

    TwoSat(<span class="algorithm-keyword">int</span> n) {
        <span class="algorithm-keyword">this</span>.n = n;
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = 0; i &lt; 2 * n; i++) {
            graph.add(<span class="algorithm-keyword">new</span> ArrayList&lt;&gt;());
            reverse.add(<span class="algorithm-keyword">new</span> ArrayList&lt;&gt;());
        }
    }

    <span class="algorithm-keyword">void</span> imply(<span class="algorithm-keyword">int</span> from, <span class="algorithm-keyword">int</span> to) {
        graph.get(from).add(to);
        reverse.get(to).add(from);
    }

    <span class="algorithm-keyword">void</span> addClause(<span class="algorithm-keyword">int</span> a, <span class="algorithm-keyword">boolean</span> aTrue, <span class="algorithm-keyword">int</span> b, <span class="algorithm-keyword">boolean</span> bTrue) {
        <span class="algorithm-keyword">int</span> x = 2 * a + (aTrue ? 0 : 1);
        <span class="algorithm-keyword">int</span> y = 2 * b + (bTrue ? 0 : 1);
        imply(x ^ 1, y);    <span class="algorithm-comment">// (x OR y)는 NOT x → y, NOT y → x</span>
        imply(y ^ 1, x);
    }

    <span class="algorithm-keyword">boolean</span>[] solve() {
        <span class="algorithm-keyword">boolean</span>[] visited = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">boolean</span>[2 * n];
        List&lt;Integer&gt; order = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;();
        <span class="algorithm-keyword">int</span>[] next = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[2 * n];
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> vertex = 0; vertex &lt; 2 * n; vertex++) {
            <span class="algorithm-keyword">if</span> (!visited[vertex]) finishDfs(vertex, visited, order, next);
        }
        <span class="algorithm-keyword">int</span>[] component = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[2 * n];
        Arrays.fill(component, -1);
        <span class="algorithm-keyword">int</span> componentId = 0;
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = order.size() - 1; i &gt;= 0; i--) {
            <span class="algorithm-keyword">int</span> vertex = order.get(i);
            <span class="algorithm-keyword">if</span> (component[vertex] == -1) assignDfs(vertex, componentId++, component);
        }
        <span class="algorithm-keyword">boolean</span>[] result = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">boolean</span>[n];
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> v = 0; v &lt; n; v++) {
            <span class="algorithm-keyword">if</span> (component[2 * v] == component[2 * v + 1]) <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">null</span>;
            result[v] = component[2 * v] &gt; component[2 * v + 1];    <span class="algorithm-comment">// 이 Kosaraju 번호는 source → sink</span>
        }
        <span class="algorithm-keyword">return</span> result;
    }

    <span class="algorithm-keyword">void</span> finishDfs(<span class="algorithm-keyword">int</span> start, <span class="algorithm-keyword">boolean</span>[] visited, List&lt;Integer&gt; order, <span class="algorithm-keyword">int</span>[] next) {
        Deque&lt;Integer&gt; stack = <span class="algorithm-keyword">new</span> ArrayDeque&lt;&gt;();
        visited[start] = <span class="algorithm-keyword">true</span>;
        stack.push(start);
        <span class="algorithm-keyword">while</span> (!stack.isEmpty()) {
            <span class="algorithm-keyword">int</span> vertex = stack.peek();
            <span class="algorithm-keyword">if</span> (next[vertex] &lt; graph.get(vertex).size()) {
                <span class="algorithm-keyword">int</span> to = graph.get(vertex).get(next[vertex]++);    <span class="algorithm-comment">// 자식 탐색 뒤에도 간선 커서를 보존</span>
                <span class="algorithm-keyword">if</span> (!visited[to]) {
                    visited[to] = <span class="algorithm-keyword">true</span>;
                    stack.push(to);
                }
            } <span class="algorithm-keyword">else</span> {
                order.add(stack.pop());
            }
        }
    }

    <span class="algorithm-keyword">void</span> assignDfs(<span class="algorithm-keyword">int</span> start, <span class="algorithm-keyword">int</span> componentId, <span class="algorithm-keyword">int</span>[] component) {
        Deque&lt;Integer&gt; stack = <span class="algorithm-keyword">new</span> ArrayDeque&lt;&gt;();
        component[start] = componentId;
        stack.push(start);
        <span class="algorithm-keyword">while</span> (!stack.isEmpty()) {
            <span class="algorithm-keyword">int</span> vertex = stack.pop();
            <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> to : reverse.get(vertex)) {
                <span class="algorithm-keyword">if</span> (component[to] != -1) <span class="algorithm-keyword">continue</span>;
                component[to] = componentId;
                stack.push(to);
            }
        }
    }
}</code></pre>

[[SCC]]
