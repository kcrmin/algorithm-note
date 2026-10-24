---
tags:
  - data-structures
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Monotonic Stack

![Monotonic Stack memory cue](../attachments/reference-monotonic-stack.svg)

> [!reference] 개념과 사용 조건
> **목적** · 각 위치에서 가장 가까운 더 큰 값·작은 값의 인덱스.
>
> **반환** · 답이 없으면 -1. 값이 아닌 인덱스를 스택에 저장.
>
> **비용** · 각 인덱스는 한 번 들어가고 한 번 빠짐. 시간 O(n), 공간 O(n).

---

## Next Greater

오른쪽의 첫 엄격히 큰 값의 **인덱스**.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-keyword">for</span> (오른쪽에서 왼쪽으로) {
    <span class="algorithm-keyword">while</span> (top이 현재 원소의 답이 될 수 없으면) {
        <span class="algorithm-comment">// pop</span>
    }
    <span class="algorithm-comment">// result[i]: 남은 top, 없으면 -1</span>
    <span class="algorithm-comment">// 현재 인덱스 push</span>
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span>[] nextGreater(<span class="algorithm-keyword">int</span>[] a) {
    <span class="algorithm-keyword">int</span>[] result = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[a.length];
    Deque&lt;Integer&gt; stack = <span class="algorithm-keyword">new</span> ArrayDeque&lt;&gt;();

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = a.length - 1; i &gt;= 0; i--) {
        <span class="algorithm-keyword">while</span> (!stack.isEmpty() &amp;&amp; <mark class="algorithm-change">a[stack.peek()] &lt;= a[i]</mark>) {
            stack.pop();    <span class="algorithm-comment">// 같은 값도 제거해야 엄격한 대소 비교가 됨</span>
        }
        result[i] = stack.isEmpty() ? -1 : stack.peek();
        stack.push(i);
    }
    <span class="algorithm-keyword">return</span> result;
}</code></pre>

---

## Next Smaller

오른쪽의 첫 엄격히 작은 값의 **인덱스**.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-keyword">for</span> (오른쪽에서 왼쪽으로) {
    <span class="algorithm-keyword">while</span> (top이 현재 원소의 답이 될 수 없으면) {
        <span class="algorithm-comment">// pop</span>
    }
    <span class="algorithm-comment">// result[i]: 남은 top, 없으면 -1</span>
    <span class="algorithm-comment">// 현재 인덱스 push</span>
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span>[] nextSmaller(<span class="algorithm-keyword">int</span>[] a) {
    <span class="algorithm-keyword">int</span>[] result = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[a.length];
    Deque&lt;Integer&gt; stack = <span class="algorithm-keyword">new</span> ArrayDeque&lt;&gt;();

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = a.length - 1; i &gt;= 0; i--) {
        <span class="algorithm-keyword">while</span> (!stack.isEmpty() &amp;&amp; <mark class="algorithm-change">a[stack.peek()] &gt;= a[i]</mark>) {
            stack.pop();    <span class="algorithm-comment">// 같은 값도 제거해야 엄격한 대소 비교가 됨</span>
        }
        result[i] = stack.isEmpty() ? -1 : stack.peek();
        stack.push(i);
    }
    <span class="algorithm-keyword">return</span> result;
}</code></pre>

---

## Previous Greater

왼쪽의 첫 엄격히 큰 값의 **인덱스**.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-keyword">for</span> (왼쪽에서 오른쪽으로) {
    <span class="algorithm-keyword">while</span> (top이 현재 원소의 답이 될 수 없으면) {
        <span class="algorithm-comment">// pop</span>
    }
    <span class="algorithm-comment">// result[i]: 남은 top, 없으면 -1</span>
    <span class="algorithm-comment">// 현재 인덱스 push</span>
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span>[] previousGreater(<span class="algorithm-keyword">int</span>[] a) {
    <span class="algorithm-keyword">int</span>[] result = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[a.length];
    Deque&lt;Integer&gt; stack = <span class="algorithm-keyword">new</span> ArrayDeque&lt;&gt;();

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = 0; i &lt; a.length; i++) {
        <span class="algorithm-keyword">while</span> (!stack.isEmpty() &amp;&amp; <mark class="algorithm-change">a[stack.peek()] &lt;= a[i]</mark>) {
            stack.pop();    <span class="algorithm-comment">// 같은 값도 제거해야 엄격한 대소 비교가 됨</span>
        }
        result[i] = stack.isEmpty() ? -1 : stack.peek();
        stack.push(i);
    }
    <span class="algorithm-keyword">return</span> result;
}</code></pre>

---

## Previous Smaller

왼쪽의 첫 엄격히 작은 값의 **인덱스**.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-keyword">for</span> (왼쪽에서 오른쪽으로) {
    <span class="algorithm-keyword">while</span> (top이 현재 원소의 답이 될 수 없으면) {
        <span class="algorithm-comment">// pop</span>
    }
    <span class="algorithm-comment">// result[i]: 남은 top, 없으면 -1</span>
    <span class="algorithm-comment">// 현재 인덱스 push</span>
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span>[] previousSmaller(<span class="algorithm-keyword">int</span>[] a) {
    <span class="algorithm-keyword">int</span>[] result = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[a.length];
    Deque&lt;Integer&gt; stack = <span class="algorithm-keyword">new</span> ArrayDeque&lt;&gt;();

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = 0; i &lt; a.length; i++) {
        <span class="algorithm-keyword">while</span> (!stack.isEmpty() &amp;&amp; <mark class="algorithm-change">a[stack.peek()] &gt;= a[i]</mark>) {
            stack.pop();    <span class="algorithm-comment">// 같은 값도 제거해야 엄격한 대소 비교가 됨</span>
        }
        result[i] = stack.isEmpty() ? -1 : stack.peek();
        stack.push(i);
    }
    <span class="algorithm-keyword">return</span> result;
}</code></pre>

[[Monotonic Deque]] · [[ArrayDeque]]
