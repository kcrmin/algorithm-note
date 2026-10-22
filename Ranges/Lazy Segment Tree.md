---
tags:
  - ranges
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Lazy Segment Tree

![Lazy Segment Tree memory cue](../attachments/reference-lazy-segment-tree.svg)

> [!reference] 개념과 사용 조건
> **목적** · 구간 전체에 더하기와 구간 합 질의를 함께 처리.
>
> **범위** · `add(left, right, delta)`와 `sum(left, right)` 모두 `[left, right)`.
>
> **핵심** · `tree[node]`는 갱신된 합. `lazy[node]`는 자식에게 아직 전달하지 않은 덧셈.
>
> **비용** · 생성 O(n), 갱신·질의 O(log n). 모든 합·delta × 구간 길이는 long 범위.

---

## Range Add / Range Sum

**Structure**

<pre class="algorithm-code" data-language="text"><code>apply(node, length, delta) {
    <span class="algorithm-comment">// 합에 delta × 길이 반영, 미룬 덧셈 누적</span>
}

add(node, segment, query) {
    <span class="algorithm-keyword">if</span> (겹치지 않으면) <span class="algorithm-keyword">return</span>;
    <span class="algorithm-keyword">if</span> (전부 포함되면) { apply(...); <span class="algorithm-keyword">return</span>; }
    <span class="algorithm-comment">// push → 양쪽 갱신 → pull</span>
}

sum(node, segment, query) {
    <span class="algorithm-keyword">if</span> (겹치지 않으면) <span class="algorithm-keyword">return</span> 0;
    <span class="algorithm-keyword">if</span> (전부 포함되면) <span class="algorithm-keyword">return</span> 현재 합;
    <span class="algorithm-comment">// push → 양쪽 질의 합산</span>
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> LazySegmentTree {
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span> n;
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">long</span>[] tree, lazy;

    LazySegmentTree(<span class="algorithm-keyword">long</span>[] values) {
        n = values.length;
        tree = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[Math.max(1, n * 4)];
        lazy = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[tree.length];
        <span class="algorithm-keyword">if</span> (n &gt; 0) {
            build(1, 0, n, values);
        }
    }

    <span class="algorithm-keyword">void</span> build(<span class="algorithm-keyword">int</span> node, <span class="algorithm-keyword">int</span> left, <span class="algorithm-keyword">int</span> right, <span class="algorithm-keyword">long</span>[] values) {
        <span class="algorithm-keyword">if</span> (right - left == 1) {
            tree[node] = values[left];
            <span class="algorithm-keyword">return</span>;
        }
        <span class="algorithm-keyword">int</span> mid = left + (right - left) / 2;
        build(node * 2, left, mid, values);
        build(node * 2 + 1, mid, right, values);
        tree[node] = tree[node * 2] + tree[node * 2 + 1];
    }

    <span class="algorithm-keyword">void</span> apply(<span class="algorithm-keyword">int</span> node, <span class="algorithm-keyword">int</span> length, <span class="algorithm-keyword">long</span> delta) {
        tree[node] += delta * length;    <span class="algorithm-comment">// 원소 하나가 아니라 구간 전체의 합</span>
        lazy[node] += delta;
    }

    <span class="algorithm-keyword">void</span> push(<span class="algorithm-keyword">int</span> node, <span class="algorithm-keyword">int</span> left, <span class="algorithm-keyword">int</span> right) {
        <span class="algorithm-keyword">if</span> (lazy[node] == 0 || right - left == 1) {
            <span class="algorithm-keyword">return</span>;
        }
        <span class="algorithm-keyword">int</span> mid = left + (right - left) / 2;
        apply(node * 2, mid - left, lazy[node]);
        apply(node * 2 + 1, right - mid, lazy[node]);
        lazy[node] = 0;    <span class="algorithm-comment">// 다시 내려보내면 같은 갱신이 중복 적용</span>
    }

    <span class="algorithm-keyword">void</span> add(<span class="algorithm-keyword">int</span> left, <span class="algorithm-keyword">int</span> right, <span class="algorithm-keyword">long</span> delta) {
        <span class="algorithm-keyword">if</span> (left &lt; right) {
            add(1, 0, n, left, right, delta);
        }
    }

    <span class="algorithm-keyword">void</span> add(<span class="algorithm-keyword">int</span> node, <span class="algorithm-keyword">int</span> left, <span class="algorithm-keyword">int</span> right, <span class="algorithm-keyword">int</span> from, <span class="algorithm-keyword">int</span> to, <span class="algorithm-keyword">long</span> delta) {
        <span class="algorithm-keyword">if</span> (to &lt;= left || right &lt;= from) {
            <span class="algorithm-keyword">return</span>;
        }
        <span class="algorithm-keyword">if</span> (from &lt;= left &amp;&amp; right &lt;= to) {
            apply(node, right - left, delta);
            <span class="algorithm-keyword">return</span>;
        }
        push(node, left, right);
        <span class="algorithm-keyword">int</span> mid = left + (right - left) / 2;
        add(node * 2, left, mid, from, to, delta);
        add(node * 2 + 1, mid, right, from, to, delta);
        tree[node] = tree[node * 2] + tree[node * 2 + 1];
    }

    <span class="algorithm-keyword">long</span> sum(<span class="algorithm-keyword">int</span> left, <span class="algorithm-keyword">int</span> right) {
        <span class="algorithm-keyword">return</span> left == right ? 0 : sum(1, 0, n, left, right);
    }

    <span class="algorithm-keyword">long</span> sum(<span class="algorithm-keyword">int</span> node, <span class="algorithm-keyword">int</span> left, <span class="algorithm-keyword">int</span> right, <span class="algorithm-keyword">int</span> from, <span class="algorithm-keyword">int</span> to) {
        <span class="algorithm-keyword">if</span> (to &lt;= left || right &lt;= from) {
            <span class="algorithm-keyword">return</span> 0;
        }
        <span class="algorithm-keyword">if</span> (from &lt;= left &amp;&amp; right &lt;= to) {
            <span class="algorithm-keyword">return</span> tree[node];
        }
        push(node, left, right);
        <span class="algorithm-keyword">int</span> mid = left + (right - left) / 2;
        <span class="algorithm-keyword">return</span> sum(node * 2, left, mid, from, to)
                + sum(node * 2 + 1, mid, right, from, to);
    }
}</code></pre>

[[Segment Tree]] · [[Difference Array]]
