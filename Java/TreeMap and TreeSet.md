---
tags:
  - java
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# TreeMap and TreeSet

![TreeMap and TreeSet memory cue](../attachments/reference-treemap-and-treeset.svg)

> [!reference] 개념과 사용 조건
> **목적** · 값의 순서와 이웃 조회가 필요한 집합·중복 집합.
>
> **경계** · lower < x, floor ≤ x, ceiling ≥ x, higher > x. 해당 값이 없으면 null.
>
> **비용** · 삽입·삭제·이웃 조회 O(log n). TreeSet은 같은 값을 하나만 보관.

---

## Ordered Set

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// 값 삽입</span>
<span class="algorithm-comment">// x 이하의 최댓값 / x 이상의 최솟값 조회</span></code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code>NavigableSet&lt;Integer&gt; set = <span class="algorithm-keyword">new</span> TreeSet&lt;&gt;();
set.add(10);
set.add(20);
Integer floor = set.floor(15);
Integer ceiling = set.ceiling(15);
Integer lower = set.lower(10);
set.remove(10);</code></pre>

---

## Multiset

**Structure**

<pre class="algorithm-code" data-language="text"><code>add(value) {
    <span class="algorithm-comment">// 빈도 증가</span>
}
remove(value) {
    <span class="algorithm-comment">// 빈도 감소, 0이면 키 제거</span>
}
min() / max() {
    <span class="algorithm-comment">// 첫 키 / 마지막 키</span>
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> Multiset {
    <span class="algorithm-keyword">final</span> TreeMap&lt;Integer, Integer&gt; count = <span class="algorithm-keyword">new</span> TreeMap&lt;&gt;();

    <span class="algorithm-keyword">void</span> add(<span class="algorithm-keyword">int</span> value) {
        count.merge(value, 1, Integer::sum);
    }

    <span class="algorithm-keyword">boolean</span> remove(<span class="algorithm-keyword">int</span> value) {
        Integer frequency = count.get(value);
        <span class="algorithm-keyword">if</span> (frequency == <span class="algorithm-keyword">null</span>) <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">false</span>;
        <span class="algorithm-keyword">if</span> (frequency == 1) count.remove(value);
        <span class="algorithm-keyword">else</span> count.put(value, frequency - 1);
        <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">true</span>;
    }

    Integer min() {
        <span class="algorithm-keyword">return</span> count.isEmpty() ? <span class="algorithm-keyword">null</span> : count.firstKey();
    }

    Integer max() {
        <span class="algorithm-keyword">return</span> count.isEmpty() ? <span class="algorithm-keyword">null</span> : count.lastKey();
    }

    Integer floor(<span class="algorithm-keyword">int</span> value) {
        <span class="algorithm-keyword">return</span> count.floorKey(value);
    }

    Integer ceiling(<span class="algorithm-keyword">int</span> value) {
        <span class="algorithm-keyword">return</span> count.ceilingKey(value);
    }
}</code></pre>

[[HashMap and HashSet]] · [[PriorityQueue]]
