---
tags:
  - search
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Backtracking

![Backtracking memory cue](../attachments/reference-backtracking.svg)

> [!reference] 개념과 사용 조건
> **목적** · 조건에 맞는 선택을 모두 생성. 순열은 순서, 조합은 원소 구성을 구분.
>
> **조건** · 입력 복사본을 정렬해 같은 깊이의 중복 선택을 건너뛴다.
>
> **비용** · 생성한 결과 수 × 결과 길이만큼 시간·출력 공간이 필요.

---

## Permutation

**Structure**

<pre class="algorithm-code" data-language="text"><code>search(현재 선택) {
    <span class="algorithm-keyword">if</span> (모든 자리를 채웠으면) {
        <span class="algorithm-comment">// 현재 선택의 복사본 저장</span>
        <span class="algorithm-keyword">return</span>;
    }
    <span class="algorithm-keyword">for</span> (각 후보) {
        <span class="algorithm-keyword">if</span> (사용 중이거나 같은 깊이의 중복이면) <span class="algorithm-keyword">continue</span>;
        <span class="algorithm-comment">// choose</span>
        search(다음 자리);
        <span class="algorithm-comment">// undo</span>
    }
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> List&lt;List&lt;Integer&gt;&gt; permutations(<span class="algorithm-keyword">int</span>[] values) {
    <span class="algorithm-keyword">int</span>[] a = values.clone();
    Arrays.sort(a);
    List&lt;List&lt;Integer&gt;&gt; result = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;();
    permute(a, <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">boolean</span>[a.length], <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;(), result);
    <span class="algorithm-keyword">return</span> result;
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">void</span> permute(<span class="algorithm-keyword">int</span>[] a, <span class="algorithm-keyword">boolean</span>[] used, List&lt;Integer&gt; path,
                    List&lt;List&lt;Integer&gt;&gt; result) {
    <span class="algorithm-keyword">if</span> (path.size() == a.length) {
        result.add(<span class="algorithm-keyword">new</span> ArrayList&lt;&gt;(path));    <span class="algorithm-comment">// path는 다음 탐색에서 다시 바뀜</span>
        <span class="algorithm-keyword">return</span>;
    }

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = 0; i &lt; a.length; i++) {
        <span class="algorithm-keyword">if</span> (used[i]) <span class="algorithm-keyword">continue</span>;
        <span class="algorithm-keyword">if</span> (i &gt; 0 &amp;&amp; a[i] == a[i - 1] &amp;&amp; !used[i - 1]) <span class="algorithm-keyword">continue</span>;    <span class="algorithm-comment">// 같은 값은 앞 복사본부터 선택</span>

        used[i] = <span class="algorithm-keyword">true</span>;
        path.add(a[i]);
        permute(a, used, path, result);
        path.remove(path.size() - 1);
        used[i] = <span class="algorithm-keyword">false</span>;
    }
}</code></pre>

---

## Combination

**Structure**

<pre class="algorithm-code" data-language="text"><code>search(start, 현재 선택) {
    <span class="algorithm-keyword">if</span> (k개를 골랐으면) {
        <span class="algorithm-comment">// 복사본 저장</span>
        <span class="algorithm-keyword">return</span>;
    }
    <span class="algorithm-keyword">for</span> (start 이후의 후보) {
        <span class="algorithm-keyword">if</span> (같은 깊이의 중복이면) <span class="algorithm-keyword">continue</span>;
        <span class="algorithm-comment">// choose</span>
        search(선택한 인덱스 다음, 현재 선택);
        <span class="algorithm-comment">// undo</span>
    }
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> List&lt;List&lt;Integer&gt;&gt; combinations(<span class="algorithm-keyword">int</span>[] values, <span class="algorithm-keyword">int</span> k) {
    <span class="algorithm-keyword">if</span> (k &lt; 0 || k &gt; values.length) <span class="algorithm-keyword">throw</span> <span class="algorithm-keyword">new</span> IllegalArgumentException();
    <span class="algorithm-keyword">int</span>[] a = values.clone();
    Arrays.sort(a);
    List&lt;List&lt;Integer&gt;&gt; result = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;();
    combine(a, k, 0, <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;(), result);
    <span class="algorithm-keyword">return</span> result;
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">void</span> combine(<span class="algorithm-keyword">int</span>[] a, <span class="algorithm-keyword">int</span> k, <span class="algorithm-keyword">int</span> start, List&lt;Integer&gt; path,
                    List&lt;List&lt;Integer&gt;&gt; result) {
    <span class="algorithm-keyword">if</span> (path.size() == k) {
        result.add(<span class="algorithm-keyword">new</span> ArrayList&lt;&gt;(path));
        <span class="algorithm-keyword">return</span>;
    }

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = start; i &lt; a.length; i++) {
        <span class="algorithm-keyword">if</span> (i &gt; start &amp;&amp; a[i] == a[i - 1]) <span class="algorithm-keyword">continue</span>;    <span class="algorithm-comment">// 같은 깊이에서 같은 값은 한 번만</span>

        path.add(a[i]);
        combine(a, k, i + 1, path, result);    <span class="algorithm-comment">// 이미 고른 위치를 재사용하지 않음</span>
        path.remove(path.size() - 1);
    }
}</code></pre>

[[Bitmask]] · [[Next Permutation]]
