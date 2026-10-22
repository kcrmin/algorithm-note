---
tags:
  - graphs
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# SCC

![SCC memory cue](../attachments/reference-scc.svg)

> [!reference] 개념과 사용 조건
> **목적** · 방향 그래프에서 서로 왕복 가능한 정점들을 하나로 묶는다.
>
> **사용** · `SCC s = new SCC(n, edges);` → `s.component[v]`, `s.count`, `s.condensation()`.
>
> **구조** · 서로 다른 SCC 사이의 간선만 남기면 DAG. 이 구현의 번호는 위상 순서.
>
> **비용·조건** · `O(V+E)`. `edges[i]={from,to}`. 두 DFS 모두 명시적 스택으로 처리.

---

## Kosaraju

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-keyword">void</span> finishDfs(start) {
    <span class="algorithm-comment">// 시작점을 방문 처리하고 push</span>
    <span class="algorithm-keyword">while</span> (스택이 비어 있지 않으면) {
        <span class="algorithm-keyword">if</span> (맨 위 정점에 남은 간선이 있으면) {
            <span class="algorithm-comment">// 간선 커서 이동 → 미방문 이웃 push</span>
        } <span class="algorithm-keyword">else</span> {
            <span class="algorithm-comment">// pop → 완료 순서에 기록</span>
        }
    }
}

<span class="algorithm-keyword">void</span> assignDfs(start, id) {
    <span class="algorithm-keyword">while</span> (스택이 비어 있지 않으면) {
        <span class="algorithm-comment">// pop → 역방향의 미배정 이웃을 같은 SCC로 지정하고 push</span>
    }
}

<span class="algorithm-comment">// 원본 DFS를 모두 마친 뒤</span>
<span class="algorithm-keyword">for</span> (order의 뒤에서부터) {
    <span class="algorithm-keyword">if</span> (아직 미배정이면) { <span class="algorithm-comment">/* 역방향 DFS로 새 SCC 구성 */</span> }
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> SCC {
    <span class="algorithm-keyword">final</span> List&lt;List&lt;Integer&gt;&gt; graph = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;();
    <span class="algorithm-keyword">final</span> List&lt;List&lt;Integer&gt;&gt; reverse = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;();
    <span class="algorithm-keyword">final</span> List&lt;Integer&gt; order = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;();
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">boolean</span>[] visited;
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span>[] component, next;
    <span class="algorithm-keyword">int</span> count;

    SCC(<span class="algorithm-keyword">int</span> n, <span class="algorithm-keyword">int</span>[][] edges) {
        visited = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">boolean</span>[n];
        component = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[n];
        next = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[n];
        Arrays.fill(component, -1);
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> v = 0; v &lt; n; v++) {
            graph.add(<span class="algorithm-keyword">new</span> ArrayList&lt;&gt;());
            reverse.add(<span class="algorithm-keyword">new</span> ArrayList&lt;&gt;());
        }
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span>[] row : edges) {
            graph.get(row[0]).add(row[1]);
            reverse.get(row[1]).add(row[0]);
        }
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> v = 0; v &lt; n; v++) {
            <span class="algorithm-keyword">if</span> (!visited[v]) finishDfs(v);
        }
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = order.size() - 1; i &gt;= 0; i--) {
            <span class="algorithm-keyword">int</span> v = order.get(i);
            <span class="algorithm-keyword">if</span> (component[v] != -1) <span class="algorithm-keyword">continue</span>;
            assignDfs(v, count++);
        }
    }

    <span class="algorithm-keyword">void</span> finishDfs(<span class="algorithm-keyword">int</span> start) {
        Deque&lt;Integer&gt; stack = <span class="algorithm-keyword">new</span> ArrayDeque&lt;&gt;();
        visited[start] = <span class="algorithm-keyword">true</span>;
        stack.push(start);
        <span class="algorithm-keyword">while</span> (!stack.isEmpty()) {
            <span class="algorithm-keyword">int</span> v = stack.peek();
            <span class="algorithm-keyword">if</span> (next[v] &lt; graph.get(v).size()) {
                <span class="algorithm-keyword">int</span> to = graph.get(v).get(next[v]++);    <span class="algorithm-comment">// 자식에서 돌아오면 다음 간선부터 재개</span>
                <span class="algorithm-keyword">if</span> (!visited[to]) {
                    visited[to] = <span class="algorithm-keyword">true</span>;
                    stack.push(to);
                }
            } <span class="algorithm-keyword">else</span> {
                order.add(stack.pop());    <span class="algorithm-comment">// 이웃 탐색이 전부 끝난 시점에 기록</span>
            }
        }
    }

    <span class="algorithm-keyword">void</span> assignDfs(<span class="algorithm-keyword">int</span> start, <span class="algorithm-keyword">int</span> id) {
        Deque&lt;Integer&gt; stack = <span class="algorithm-keyword">new</span> ArrayDeque&lt;&gt;();
        component[start] = id;
        stack.push(start);
        <span class="algorithm-keyword">while</span> (!stack.isEmpty()) {
            <span class="algorithm-keyword">int</span> v = stack.pop();
            <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> to : reverse.get(v)) {
                <span class="algorithm-keyword">if</span> (component[to] != -1) <span class="algorithm-keyword">continue</span>;
                component[to] = id;
                stack.push(to);
            }
        }
    }

    List&lt;List&lt;Integer&gt;&gt; condensation() {
        List&lt;List&lt;Integer&gt;&gt; dag = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;(count);
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> id = 0; id &lt; count; id++) dag.add(<span class="algorithm-keyword">new</span> ArrayList&lt;&gt;());
        Set&lt;Long&gt; used = <span class="algorithm-keyword">new</span> HashSet&lt;&gt;();
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> v = 0; v &lt; graph.size(); v++) {
            <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> to : graph.get(v)) {
                <span class="algorithm-keyword">int</span> a = component[v], b = component[to];
                <span class="algorithm-keyword">if</span> (a == b) <span class="algorithm-keyword">continue</span>;
                <span class="algorithm-keyword">long</span> key = ((<span class="algorithm-keyword">long</span>) a &lt;&lt; 32) | (b &amp; 0xffffffffL);
                <span class="algorithm-keyword">if</span> (used.add(key)) dag.get(a).add(b);
            }
        }
        <span class="algorithm-keyword">return</span> dag;
    }
}</code></pre>

[[2-SAT]] · [[DAG DP]]
