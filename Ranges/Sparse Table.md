---
tags:
  - ranges
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Sparse Table

![Sparse Table memory cue](../attachments/reference-sparse-table.svg)

> [!reference] 개념과 사용 조건
> **목적** · 갱신 없는 배열의 구간 최솟값을 O(1)에 질의.
>
> **범위** · 비어 있지 않은 `[left, right)`. 생성은 빈 배열도 허용하지만 질의는 불가.
>
> **조건** · 겹친 원소를 두 번 써도 같은 min/max/gcd에 적합. sum에는 이 질의식을 쓰면 안 된다.
>
> **비용** · 전처리·공간 O(n log n), 질의 O(1).

---

## Range Minimum

값이 바뀌지 않는 배열의 비어 있지 않은 `[left, right)` 구간에서 최솟값을 구한다.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-keyword">for</span> (블록 길이를 2배씩 늘리며) {
    <span class="algorithm-comment">// 앞 절반과 뒤 절반의 최솟값 결합</span>
}

min(left, right) {
    <span class="algorithm-comment">// 구간 길이 이하의 가장 큰 2의 거듭제곱 선택</span>
    <span class="algorithm-keyword">return</span> 양끝 두 블록의 최솟값;
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> SparseTable {
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span>[] log;
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span>[][] table;

    SparseTable(<span class="algorithm-keyword">int</span>[] values) {
        <span class="algorithm-keyword">int</span> n = values.length;
        log = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[n + 1];
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = 2; i &lt;= n; i++) {
            log[i] = log[i / 2] + 1;
        }
        table = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[log[n] + 1][n];
        table[0] = values.clone();

        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> k = 1; k &lt; table.length; k++) {
            <span class="algorithm-keyword">int</span> half = 1 &lt;&lt; (k - 1), length = 1 &lt;&lt; k;
            <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> left = 0; left + length &lt;= n; left++) {
                table[k][left] = Math.min(table[k - 1][left], table[k - 1][left + half]);
            }
        }
    }

    <span class="algorithm-keyword">int</span> min(<span class="algorithm-keyword">int</span> left, <span class="algorithm-keyword">int</span> right) {
        <span class="algorithm-keyword">int</span> k = log[right - left], length = 1 &lt;&lt; k;
        <span class="algorithm-keyword">return</span> Math.min(table[k][left], table[k][right - length]);    <span class="algorithm-comment">// 겹침은 min 결과에 영향 없음</span>
    }
}</code></pre>

[[Segment Tree]]
