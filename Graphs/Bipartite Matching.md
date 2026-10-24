---
tags:
  - graphs
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Bipartite Matching

![Bipartite Matching memory cue](../attachments/reference-bipartite-matching.svg)

> [!reference] 개념과 사용 조건
> **목적** · 왼쪽·오른쪽 정점을 각각 한 번만 사용하는 최대 매칭.
>
> **입력** · `edges[i]={left,right}`. 양쪽 번호는 각자 `0..size-1`.
>
> **반환** · `matchRight[right]`는 매칭된 왼쪽 정점, 없으면 `-1`. `-1` 아닌 개수가 매칭 크기.
>
> **비용·조건** · Kuhn: `O(L×E)`. 증강 DFS는 재귀이며, 긴 증강 경로에서는 `StackOverflowError` 가능.

---

## Kuhn

각 정점을 한 번만 사용하는 왼쪽·오른쪽 정점 쌍을 최대한 많이 구한다.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-keyword">for</span> (각 왼쪽 정점) {
    <span class="algorithm-comment">// 이번 시도의 방문 기록 초기화</span>
    <span class="algorithm-comment">// augment 호출</span>
}

<span class="algorithm-keyword">boolean</span> augment(left) {
    <span class="algorithm-keyword">if</span> (이번 시도에서 방문했으면) <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">false</span>;
    <span class="algorithm-comment">// 방문 표시</span>
    <span class="algorithm-keyword">for</span> (연결된 각 right) {
        <span class="algorithm-keyword">if</span> (비어 있거나 기존 상대를 옮길 수 있으면) {
            <span class="algorithm-comment">// 매칭 교체</span>
            <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">true</span>;
        }
    }
    <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">false</span>;
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> List&lt;List&lt;Integer&gt;&gt; graph;
<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span>[] matchRight;
<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">boolean</span>[] visited;

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span>[] maximumMatching(<span class="algorithm-keyword">int</span> leftSize, <span class="algorithm-keyword">int</span> rightSize, <span class="algorithm-keyword">int</span>[][] edges) {
    graph = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;(leftSize);
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> vertex = 0; vertex &lt; leftSize; vertex++) graph.add(<span class="algorithm-keyword">new</span> ArrayList&lt;&gt;());
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span>[] row : edges) graph.get(row[0]).add(row[1]);
    matchRight = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[rightSize];
    Arrays.fill(matchRight, -1);
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> left = 0; left &lt; leftSize; left++) {
        visited = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">boolean</span>[leftSize];    <span class="algorithm-comment">// 매 증강 시도마다 새 경로를 허용</span>
        augment(left);
    }
    <span class="algorithm-keyword">return</span> matchRight;
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">boolean</span> augment(<span class="algorithm-keyword">int</span> left) {
    <span class="algorithm-keyword">if</span> (visited[left]) <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">false</span>;
    visited[left] = <span class="algorithm-keyword">true</span>;
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> right : graph.get(left)) {
        <span class="algorithm-keyword">if</span> (matchRight[right] == -1
                || augment(matchRight[right])) {
            matchRight[right] = left;    <span class="algorithm-comment">// 기존 상대를 옮길 수 있을 때만 자리 교체</span>
            <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">true</span>;
        }
    }
    <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">false</span>;
}</code></pre>

[[Bipartite Graph Check]] · [[Max Flow and Min Cut]]
