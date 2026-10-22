---
tags:
  - graphs
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Union-Find

![Union-Find memory cue](../attachments/reference-union-find.svg)

> [!reference] 개념과 사용 조건
> **목적** · 집합 합치기와 같은 집합인지 확인. 정점 `0..n-1`.
>
> **판정** · `find(a) == find(b)`. `union`은 실제로 합쳤을 때만 `true`.
>
> **크기** · 집합 크기는 `size[find(v)]`.
>
> **비용** · 경로 압축 + 크기 기준 합치기: 연산당 상각 `O(α(n))`.

---

## Template

`UnionFind uf = new UnionFind(n);`으로 생성.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-keyword">int</span> find(v) {
    <span class="algorithm-keyword">while</span> (자기 자신이 대표가 아니면) {
        <span class="algorithm-comment">// 부모 경로 압축 → 위로 이동</span>
    }
    <span class="algorithm-keyword">return</span> v;
}

<span class="algorithm-keyword">boolean</span> union(a, b) {
    <span class="algorithm-comment">// 두 대표 찾기</span>
    <span class="algorithm-keyword">if</span> (이미 같은 대표면) <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">false</span>;
    <span class="algorithm-comment">// 작은 집합을 큰 집합에 붙이기</span>
    <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">true</span>;
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> UnionFind {
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span>[] parent, size;

    UnionFind(<span class="algorithm-keyword">int</span> n) {
        parent = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[n];
        size = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[n];
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> v = 0; v &lt; n; v++) {
            parent[v] = v;
            size[v] = 1;
        }
    }

    <span class="algorithm-keyword">int</span> find(<span class="algorithm-keyword">int</span> v) {
        <span class="algorithm-keyword">while</span> (v != parent[v]) {
            parent[v] = parent[parent[v]];    <span class="algorithm-comment">// 지나가는 경로를 짧게 만들어 다음 조회를 줄임</span>
            v = parent[v];
        }
        <span class="algorithm-keyword">return</span> v;
    }

    <span class="algorithm-keyword">boolean</span> union(<span class="algorithm-keyword">int</span> a, <span class="algorithm-keyword">int</span> b) {
        a = find(a);
        b = find(b);
        <span class="algorithm-keyword">if</span> (a == b) <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">false</span>;
        <span class="algorithm-keyword">if</span> (size[a] &lt; size[b]) {
            <span class="algorithm-keyword">int</span> temp = a;
            a = b;
            b = temp;
        }
        parent[b] = a;    <span class="algorithm-comment">// 작은 트리를 큰 트리 아래에 붙여 높이 억제</span>
        size[a] += size[b];
        <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">true</span>;
    }
}</code></pre>

[[Kruskal]]
