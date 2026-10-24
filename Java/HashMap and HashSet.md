---
tags:
  - java
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# HashMap and HashSet

![HashMap and HashSet memory cue](../attachments/reference-hashmap-and-hashset.svg)

> [!reference] 개념과 사용 조건
> **목적** · 값으로 기록을 조회하고 중복·빈도를 관리.
>
> **선택** · 값 범위가 작고 연속이면 빈도 배열, 크거나 흩어져 있으면 HashMap.
>
> **비용** · HashMap/HashSet 조회·갱신은 평균 O(1). 순서는 보장하지 않는다.

---

## Frequency Map

값별 개수를 세고, 특정 값 하나를 제거한다.

**Structure**

<pre class="algorithm-code" data-language="text"><code>count(values) {
    <span class="algorithm-comment">// 각 값의 빈도 증가</span>
}
removeOne(count, value) {
    <span class="algorithm-comment">// 1 감소, 0이면 키 제거</span>
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> Map&lt;Integer, Integer&gt; frequencies(<span class="algorithm-keyword">int</span>[] values) {
    Map&lt;Integer, Integer&gt; count = <span class="algorithm-keyword">new</span> HashMap&lt;&gt;();
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> value : values) count.merge(value, 1, Integer::sum);
    <span class="algorithm-keyword">return</span> count;
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">boolean</span> removeOne(Map&lt;Integer, Integer&gt; count, <span class="algorithm-keyword">int</span> value) {
    <span class="algorithm-keyword">int</span> current = count.getOrDefault(value, 0);
    <span class="algorithm-keyword">if</span> (current == 0) <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">false</span>;
    <span class="algorithm-keyword">if</span> (current == 1) count.remove(value);    <span class="algorithm-comment">// size()를 실제 종류 수로 유지</span>
    <span class="algorithm-keyword">else</span> count.put(value, current - 1);
    <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">true</span>;
}</code></pre>

---

## Set

값의 첫 등장·중복·존재 여부를 판정한다.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// add의 반환값으로 처음 등장했는지 판정</span>
<span class="algorithm-comment">// contains로 존재 확인</span></code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code>Set&lt;Integer&gt; seen = <span class="algorithm-keyword">new</span> HashSet&lt;&gt;();
<span class="algorithm-keyword">boolean</span> first = seen.add(7);
<span class="algorithm-keyword">boolean</span> duplicate = !seen.add(7);
<span class="algorithm-keyword">boolean</span> exists = seen.contains(7);
seen.remove(7);</code></pre>

---

## Frequency Array

값을 인덱스로 삼아 각 값의 등장 횟수를 센다.

모든 값은 `0..maxValue`. 시간 O(n + maxValue), 공간 O(maxValue).

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// 값 자체를 인덱스로 사용해 빈도 증가</span></code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span>[] frequenciesInRange(<span class="algorithm-keyword">int</span>[] values, <span class="algorithm-keyword">int</span> maxValue) {
    <span class="algorithm-keyword">int</span>[] count = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[maxValue + 1];
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> value : values) count[value]++;
    <span class="algorithm-keyword">return</span> count;
}</code></pre>

[[Sliding Window]] · [[Prefix Sum with Hash Map]]
