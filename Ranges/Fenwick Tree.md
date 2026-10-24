---
tags:
  - ranges
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Fenwick Tree

![Fenwick Tree memory cue](../attachments/reference-fenwick-tree.svg)

> [!reference] 개념과 사용 조건
> **목적** · 값이 바뀌는 배열의 누적합·구간 합.
>
> **범위** · 바깥 인덱스는 0-based. `add/set`은 `0..n-1`, `sum`은 `[left, right)`.
>
> **핵심** · 내부만 1-based. `i & -i`는 현재 블록의 길이.
>
> **비용** · 생성 O(n log n), 갱신·질의 O(log n). 합은 long 범위.

---

## Template

원소의 값을 더하거나 바꾸면서 누적합과 구간 합을 구한다.

**Structure**

<pre class="algorithm-code" data-language="text"><code>add(index, delta) {
    <span class="algorithm-keyword">while</span> (배열 안이면) {
        <span class="algorithm-comment">// 이 값을 포함하는 블록 갱신 → 상위 블록</span>
    }
}

prefix(end) {
    <span class="algorithm-keyword">while</span> (남은 prefix가 있으면) {
        <span class="algorithm-comment">// 끝 블록 더하기 → 그 블록 제거</span>
    }
}

sum(left, right) {
    <span class="algorithm-keyword">return</span> prefix(right) - prefix(left);
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> Fenwick {
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span> n;
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">long</span>[] tree;

    Fenwick(<span class="algorithm-keyword">long</span>[] values) {
        n = values.length;
        tree = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[n + 1];
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = 0; i &lt; n; i++) {
            add(i, values[i]);
        }
    }

    <span class="algorithm-keyword">void</span> add(<span class="algorithm-keyword">int</span> index, <span class="algorithm-keyword">long</span> delta) {
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = index + 1; i &lt;= n; i += i &amp; -i) {    <span class="algorithm-comment">// 0에서는 lowbit이 0이라 이동 불가</span>
            tree[i] += delta;
        }
    }

    <span class="algorithm-keyword">long</span> prefix(<span class="algorithm-keyword">int</span> end) {
        <span class="algorithm-keyword">long</span> answer = 0;
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = end; i &gt; 0; i -= i &amp; -i) {
            answer += tree[i];
        }
        <span class="algorithm-keyword">return</span> answer;
    }

    <span class="algorithm-keyword">long</span> sum(<span class="algorithm-keyword">int</span> left, <span class="algorithm-keyword">int</span> right) {
        <span class="algorithm-keyword">return</span> prefix(right) - prefix(left);
    }

    <span class="algorithm-keyword">void</span> set(<span class="algorithm-keyword">int</span> index, <span class="algorithm-keyword">long</span> value) {
        add(index, value - sum(index, index + 1));    <span class="algorithm-comment">// 저장값 자체가 아니라 변화량 반영</span>
    }
}</code></pre>

[[Prefix Sum]] · [[Segment Tree]]
