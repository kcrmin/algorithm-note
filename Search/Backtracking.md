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
> **상태** · `depth`는 고른 개수, `start`는 다음 후보의 시작 인덱스.
>
> **비용** · 생성한 결과 수 × 결과 길이만큼 시간·출력 공간이 필요.

---

## Permutation · Basic

서로 다른 값의 입력에서 모든 원소를 한 번씩 사용하는 순열을 구한다.

**Structure**

<pre class="algorithm-code" data-language="text"><code>dfs(depth) {
    <span class="algorithm-keyword">if</span> (depth == 원소 수) {
        <span class="algorithm-comment">// 결과 저장</span>
        <span class="algorithm-keyword">return</span>;
    }

    <span class="algorithm-keyword">for</span> (각 인덱스 i) {
        <span class="algorithm-keyword">if</span> (이미 사용 중이면) {
            <span class="algorithm-keyword">continue</span>;
        }

        <span class="algorithm-comment">// choose: 사용 표시 + 경로에 추가</span>
        dfs(depth + 1);
        <span class="algorithm-comment">// undo: 경로에서 제거 + 사용 해제</span>
    }
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span>[] a;
<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">boolean</span>[] used;
<span class="algorithm-keyword">static</span> List&lt;Integer&gt; path;
<span class="algorithm-keyword">static</span> List&lt;List&lt;Integer&gt;&gt; result;

<span class="algorithm-keyword">static</span> List&lt;List&lt;Integer&gt;&gt; permutations(<span class="algorithm-keyword">int</span>[] values) {
    a = values;
    used = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">boolean</span>[a.length];
    path = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;();
    result = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;();
    dfs(0);
    <span class="algorithm-keyword">return</span> result;
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">void</span> dfs(<span class="algorithm-keyword">int</span> depth) {
    <span class="algorithm-keyword">if</span> (depth == a.length) {
        result.add(<span class="algorithm-keyword">new</span> ArrayList&lt;&gt;(path));    <span class="algorithm-comment">// 이후 path가 바뀌므로 복사해서 저장</span>
        <span class="algorithm-keyword">return</span>;
    }

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = 0; i &lt; a.length; i++) {
        <span class="algorithm-keyword">if</span> (used[i]) {
            <span class="algorithm-keyword">continue</span>;
        }

        used[i] = <span class="algorithm-keyword">true</span>;
        path.add(a[i]);
        dfs(depth + 1);
        path.remove(path.size() - 1);
        used[i] = <span class="algorithm-keyword">false</span>;
    }
}</code></pre>

---

## Permutation · Duplicate Values

같은 값이 있어도, 값의 나열이 같은 순열은 한 번만 구한다.

**Structure**

<pre class="algorithm-code" data-language="text"><code>dfs(depth) {
    <span class="algorithm-keyword">if</span> (depth == 원소 수) {
        <span class="algorithm-comment">// 결과 저장</span>
        <span class="algorithm-keyword">return</span>;
    }

    <span class="algorithm-keyword">for</span> (각 값) {
        <span class="algorithm-keyword">if</span> (남은 개수가 0이면) {
            <span class="algorithm-keyword">continue</span>;
        }

        <span class="algorithm-comment">// choose: 개수 1 감소 + 경로에 추가</span>
        dfs(depth + 1);
        <span class="algorithm-comment">// undo: 경로에서 제거 + 개수 복구</span>
    }
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span> n;
<span class="algorithm-keyword">static</span> Map&lt;Integer, Integer&gt; count;
<span class="algorithm-keyword">static</span> List&lt;Integer&gt; path;
<span class="algorithm-keyword">static</span> List&lt;List&lt;Integer&gt;&gt; result;

<span class="algorithm-keyword">static</span> List&lt;List&lt;Integer&gt;&gt; uniquePermutations(<span class="algorithm-keyword">int</span>[] values) {
    n = values.length;
    count = <span class="algorithm-keyword">new</span> HashMap&lt;&gt;();
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> value : values) {
        count.put(value, count.getOrDefault(value, 0) + 1);
    }

    path = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;();
    result = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;();
    dfs(0);
    <span class="algorithm-keyword">return</span> result;
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">void</span> dfs(<span class="algorithm-keyword">int</span> depth) {
    <span class="algorithm-keyword">if</span> (depth == n) {
        result.add(<span class="algorithm-keyword">new</span> ArrayList&lt;&gt;(path));    <span class="algorithm-comment">// 이후 path가 바뀌므로 복사해서 저장</span>
        <span class="algorithm-keyword">return</span>;
    }

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> value : count.keySet()) {    <span class="algorithm-comment">// 같은 숫자는 후보 하나로만 등장</span>
        <span class="algorithm-keyword">int</span> remaining = count.get(value);
        <span class="algorithm-keyword">if</span> (remaining == 0) {
            <span class="algorithm-keyword">continue</span>;
        }

        count.put(value, remaining - 1);
        path.add(value);
        dfs(depth + 1);
        path.remove(path.size() - 1);
        count.put(value, remaining);
    }
}</code></pre>

---

## Combination · Basic

서로 다른 값의 입력에서 원소 k개를 고르는 조합을 구한다.

**Structure**

<pre class="algorithm-code" data-language="text"><code>dfs(depth, start) {
    <span class="algorithm-keyword">if</span> (depth == k) {
        <span class="algorithm-comment">// 결과 저장</span>
        <span class="algorithm-keyword">return</span>;
    }

    <span class="algorithm-keyword">for</span> (start부터의 인덱스 i) {
        <span class="algorithm-comment">// choose: 경로에 추가</span>
        dfs(depth + 1, i + 1);
        <span class="algorithm-comment">// undo: 경로에서 제거</span>
    }
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span>[] a;
<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span> k;
<span class="algorithm-keyword">static</span> List&lt;Integer&gt; path;
<span class="algorithm-keyword">static</span> List&lt;List&lt;Integer&gt;&gt; result;

<span class="algorithm-keyword">static</span> List&lt;List&lt;Integer&gt;&gt; combinations(<span class="algorithm-keyword">int</span>[] values, <span class="algorithm-keyword">int</span> target) {
    <span class="algorithm-keyword">if</span> (target &lt; 0 || target &gt; values.length) {
        <span class="algorithm-keyword">throw</span> <span class="algorithm-keyword">new</span> IllegalArgumentException();
    }

    a = values;
    k = target;
    path = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;();
    result = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;();
    dfs(0, 0);
    <span class="algorithm-keyword">return</span> result;
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">void</span> dfs(<span class="algorithm-keyword">int</span> depth, <span class="algorithm-keyword">int</span> start) {
    <span class="algorithm-keyword">if</span> (depth == k) {
        result.add(<span class="algorithm-keyword">new</span> ArrayList&lt;&gt;(path));    <span class="algorithm-comment">// 이후 path가 바뀌므로 복사해서 저장</span>
        <span class="algorithm-keyword">return</span>;
    }

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = start; i &lt; a.length; i++) {
        path.add(a[i]);
        dfs(depth + 1, i + 1);    <span class="algorithm-comment">// 뒤쪽 인덱스만 골라 순서만 다른 중복을 막음</span>
        path.remove(path.size() - 1);
    }
}</code></pre>

[[Bitmask]] · [[Next Permutation]]
