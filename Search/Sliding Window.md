---
tags:
  - search
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Sliding Window

![Sliding Window memory cue](../attachments/reference-sliding-window.svg)

> [!reference] 개념과 사용 조건
> **목적** · 연속 구간의 상태를 유지하며 양쪽 경계를 한 방향으로 이동.
>
> **핵심** · 최장 유효 구간은 위반을 없앤 뒤 기록. 최단 충족 구간은 조건이 깨지기 전에 기록.
>
> **범위** · 아래 세 변형 모두 시간 O(n). 합은 long, 빈도는 HashMap으로 유지.

---

## Fixed Size

음수가 있어도 사용 가능. 길이가 정확히 `k`인 구간만 비교한다.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-keyword">for</span> (right를 처음부터 끝까지) {
    <span class="algorithm-comment">// expand</span>
    <span class="algorithm-keyword">if</span> (길이가 k를 넘으면) {
        <span class="algorithm-comment">// 가장 왼쪽 원소 제거</span>
    }
    <span class="algorithm-keyword">if</span> (길이가 k이면) {
        <span class="algorithm-comment">// answer</span>
    }
}
<span class="algorithm-keyword">return</span> answer;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">long</span> maxFixedSum(<span class="algorithm-keyword">int</span>[] a, <span class="algorithm-keyword">int</span> k) {
    <span class="algorithm-keyword">if</span> (k &lt; 1 || k &gt; a.length) <span class="algorithm-keyword">throw</span> <span class="algorithm-keyword">new</span> IllegalArgumentException();
    <span class="algorithm-keyword">long</span> sum = 0, answer = Long.MIN_VALUE;    <span class="algorithm-comment">// 음수만 있어도 빈 구간을 고르지 않음</span>

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> right = 0; right &lt; a.length; right++) {
        sum += a[right];
        <span class="algorithm-keyword">if</span> (right &gt;= k) sum -= a[right - k];
        <span class="algorithm-keyword">if</span> (right &gt;= k - 1) answer = Math.max(answer, sum);
    }
    <span class="algorithm-keyword">return</span> answer;
}</code></pre>

---

## Longest Valid Window

서로 다른 값이 최대 `k`종류인 최장 구간. `k = 0`이면 0.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-keyword">for</span> (right를 처음부터 끝까지) {
    <span class="algorithm-comment">// expand: 빈도 추가</span>
    <span class="algorithm-keyword">while</span> (조건을 위반하면) {
        <span class="algorithm-comment">// shrink: 왼쪽 빈도 제거</span>
    }
    <span class="algorithm-comment">// answer: 유효해진 구간 길이 기록</span>
}
<span class="algorithm-keyword">return</span> answer;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span> longestAtMostKDistinct(<span class="algorithm-keyword">int</span>[] a, <span class="algorithm-keyword">int</span> k) {
    <span class="algorithm-keyword">if</span> (k &lt; 0) <span class="algorithm-keyword">throw</span> <span class="algorithm-keyword">new</span> IllegalArgumentException();
    Map&lt;Integer, Integer&gt; count = <span class="algorithm-keyword">new</span> HashMap&lt;&gt;();
    <span class="algorithm-keyword">int</span> left = 0, answer = 0;

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> right = 0; right &lt; a.length; right++) {
        count.merge(a[right], 1, Integer::sum);

        <mark class="algorithm-change"><span class="algorithm-keyword">while</span></mark><mark class="algorithm-change"> (count.size() &gt; k)</mark> {
            <span class="algorithm-keyword">int</span> value = a[left++];
            <span class="algorithm-keyword">int</span> remaining = count.get(value) - 1;
            <span class="algorithm-keyword">if</span> (remaining == 0) count.remove(value);    <span class="algorithm-comment">// 0개인 값은 종류 수에서 제외</span>
            <span class="algorithm-keyword">else</span> count.put(value, remaining);
        }
        answer = Math.max(answer, right - left + 1);
    }
    <span class="algorithm-keyword">return</span> answer;
}</code></pre>

---

## Shortest Satisfying Window

**모든 원소 ≥ 0, target > 0.** 답이 없으면 0. 음수가 있으면 이 축소 규칙은 성립하지 않는다.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-keyword">for</span> (right를 처음부터 끝까지) {
    <span class="algorithm-comment">// expand: 합에 추가</span>
    <span class="algorithm-keyword">while</span> (합이 target 이상이면) {
        <span class="algorithm-comment">// answer: 줄이기 전에 현재 길이 기록</span>
        <span class="algorithm-comment">// shrink: 왼쪽을 제거하고 더 짧은 답 시도</span>
    }
}
<span class="algorithm-keyword">return</span> answer;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span> shortestAtLeast(<span class="algorithm-keyword">int</span>[] a, <span class="algorithm-keyword">long</span> target) {
    <span class="algorithm-keyword">if</span> (target &lt;= 0) <span class="algorithm-keyword">throw</span> <span class="algorithm-keyword">new</span> IllegalArgumentException();
    <span class="algorithm-keyword">int</span> left = 0, answer = Integer.MAX_VALUE;
    <span class="algorithm-keyword">long</span> sum = 0;

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> right = 0; right &lt; a.length; right++) {
        sum += a[right];

        <mark class="algorithm-change"><span class="algorithm-keyword">while</span></mark><mark class="algorithm-change"> (sum &gt;= target)</mark> {
            <mark class="algorithm-change">answer = Math.min(answer, right - left + 1);</mark>    <span class="algorithm-comment">// 제거하면 조건이 깨질 수 있음</span>
            sum -= a[left++];
        }
    }
    <span class="algorithm-keyword">return</span> answer == Integer.MAX_VALUE ? 0 : answer;
}</code></pre>

[[Two Pointers]] · [[Prefix Sum with Hash Map]] · [[Monotonic Deque]]
