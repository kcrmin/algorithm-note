---
tags:
  - data-structures
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Monotonic Deque

![Monotonic Deque memory cue](../attachments/reference-monotonic-deque.svg)

> [!reference] 개념과 사용 조건
> **목적** · 길이 k인 모든 연속 구간의 최댓값·최솟값.
>
> **조건** · 1 ≤ k ≤ n. 값 대신 인덱스를 넣어 만료 여부도 판단.
>
> **비용** · 시간 O(n), deque 공간 O(k), 결과 공간 O(n).

---

## Maximum

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-keyword">for</span> (right를 처음부터 끝까지) {
    <span class="algorithm-comment">// 앞에서 만료된 인덱스 제거</span>
    <span class="algorithm-comment">// 뒤에서 현재 값보다 불리한 후보 제거</span>
    <span class="algorithm-comment">// 현재 인덱스 추가</span>
    <span class="algorithm-keyword">if</span> (구간 길이가 k이면) {
        <span class="algorithm-comment">// 맨 앞의 값 기록</span>
    }
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span>[] windowMax(<span class="algorithm-keyword">int</span>[] a, <span class="algorithm-keyword">int</span> k) {
    <span class="algorithm-keyword">if</span> (k &lt; 1 || k &gt; a.length) <span class="algorithm-keyword">throw</span> <span class="algorithm-keyword">new</span> IllegalArgumentException();
    <span class="algorithm-keyword">int</span>[] answer = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[a.length - k + 1];
    Deque&lt;Integer&gt; deque = <span class="algorithm-keyword">new</span> ArrayDeque&lt;&gt;();

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> right = 0; right &lt; a.length; right++) {
        <span class="algorithm-keyword">while</span> (!deque.isEmpty() &amp;&amp; deque.peekFirst() &lt;= right - k) {
            deque.pollFirst();
        }
        <span class="algorithm-keyword">while</span> (!deque.isEmpty() &amp;&amp; <mark class="algorithm-change">a[deque.peekLast()] &lt;= a[right]</mark>) {
            deque.pollLast();    <span class="algorithm-comment">// 같은 값이면 더 늦게 만료되는 현재 인덱스를 남김</span>
        }
        deque.offerLast(right);
        <span class="algorithm-keyword">if</span> (right &gt;= k - 1) answer[right - k + 1] = a[deque.peekFirst()];
    }
    <span class="algorithm-keyword">return</span> answer;
}</code></pre>

---

## Minimum

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-keyword">for</span> (right를 처음부터 끝까지) {
    <span class="algorithm-comment">// 앞에서 만료된 인덱스 제거</span>
    <span class="algorithm-comment">// 뒤에서 현재 값보다 불리한 후보 제거</span>
    <span class="algorithm-comment">// 현재 인덱스 추가</span>
    <span class="algorithm-keyword">if</span> (구간 길이가 k이면) {
        <span class="algorithm-comment">// 맨 앞의 값 기록</span>
    }
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span>[] windowMin(<span class="algorithm-keyword">int</span>[] a, <span class="algorithm-keyword">int</span> k) {
    <span class="algorithm-keyword">if</span> (k &lt; 1 || k &gt; a.length) <span class="algorithm-keyword">throw</span> <span class="algorithm-keyword">new</span> IllegalArgumentException();
    <span class="algorithm-keyword">int</span>[] answer = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[a.length - k + 1];
    Deque&lt;Integer&gt; deque = <span class="algorithm-keyword">new</span> ArrayDeque&lt;&gt;();

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> right = 0; right &lt; a.length; right++) {
        <span class="algorithm-keyword">while</span> (!deque.isEmpty() &amp;&amp; deque.peekFirst() &lt;= right - k) {
            deque.pollFirst();
        }
        <span class="algorithm-keyword">while</span> (!deque.isEmpty() &amp;&amp; <mark class="algorithm-change">a[deque.peekLast()] &gt;= a[right]</mark>) {
            deque.pollLast();    <span class="algorithm-comment">// 같은 값이면 더 늦게 만료되는 현재 인덱스를 남김</span>
        }
        deque.offerLast(right);
        <span class="algorithm-keyword">if</span> (right &gt;= k - 1) answer[right - k + 1] = a[deque.peekFirst()];
    }
    <span class="algorithm-keyword">return</span> answer;
}</code></pre>

[[Sliding Window]] · [[Monotonic Stack]]
