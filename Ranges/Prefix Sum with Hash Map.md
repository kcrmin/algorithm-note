---
tags:
  - ranges
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Prefix Sum with Hash Map

![Prefix Sum with Hash Map memory cue](../attachments/reference-prefix-sum-with-hash-map.svg)

> [!reference] 개념과 사용 조건
> **목적** · 합이 `target`인 연속 부분 배열의 개수·존재 여부. 음수도 허용.
>
> **핵심** · 현재 누적합 − 이전 누적합 = target. 시작 전 누적합은 0.
>
> **조건** · 합과 `prefix - target`이 long 범위에 들어와야 한다.
>
> **비용** · 평균 O(n) 시간, O(n) 공간.

---

## Count

합이 target인 비어 있지 않은 연속 부분 배열의 개수를 구한다.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// 빈 prefix의 빈도 = 1</span>
<span class="algorithm-keyword">for</span> (각 값) {
    <span class="algorithm-comment">// 현재 prefix 갱신</span>
    <span class="algorithm-comment">// answer += prefix - target의 기존 빈도</span>
    <span class="algorithm-comment">// 현재 prefix 빈도 증가</span>
}
<span class="algorithm-keyword">return</span> answer;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">import</span> java.util.*;

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">long</span> countSubarrays(<span class="algorithm-keyword">int</span>[] a, <span class="algorithm-keyword">long</span> target) {
    Map&lt;Long, Long&gt; count = <span class="algorithm-keyword">new</span> HashMap&lt;&gt;();
    count.put(0L, 1L);    <span class="algorithm-comment">// 0번에서 시작하는 구간도 세기 위해</span>
    <span class="algorithm-keyword">long</span> prefix = 0, answer = 0;

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> value : a) {
        prefix += value;
        answer += count.getOrDefault(prefix - target, 0L);
        count.merge(prefix, 1L, Long::sum);    <span class="algorithm-comment">// 먼저 저장하면 target=0에서 빈 구간을 셈</span>
    }

    <span class="algorithm-keyword">return</span> answer;
}</code></pre>

---

## Existence

합이 target인 비어 있지 않은 연속 부분 배열이 존재하는지 확인한다.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// 빈 prefix 저장</span>
<span class="algorithm-keyword">for</span> (각 값) {
    <span class="algorithm-comment">// 현재 prefix 갱신</span>
    <span class="algorithm-keyword">if</span> (prefix - target이 이전에 나왔으면) {
        <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">true</span>;
    }
    <span class="algorithm-comment">// 현재 prefix 저장</span>
}
<span class="algorithm-keyword">return</span> <span class="algorithm-keyword">false</span>;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">import</span> java.util.*;

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">boolean</span> hasSubarray(<span class="algorithm-keyword">int</span>[] a, <span class="algorithm-keyword">long</span> target) {
    Set&lt;Long&gt; seen = <span class="algorithm-keyword">new</span> HashSet&lt;&gt;();
    seen.add(0L);
    <span class="algorithm-keyword">long</span> prefix = 0;

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> value : a) {
        prefix += value;
        <span class="algorithm-keyword">if</span> (seen.contains(prefix - target)) {
            <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">true</span>;
        }
        seen.add(prefix);    <span class="algorithm-comment">// 현재 위치 자신과 짝지어 빈 구간을 만들지 않음</span>
    }

    <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">false</span>;
}</code></pre>

[[Prefix Sum]] · [[HashMap and HashSet]]
