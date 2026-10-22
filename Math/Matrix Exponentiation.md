---
tags:
  - math
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Matrix Exponentiation

![Matrix Exponentiation memory cue](../attachments/reference-matrix-exponentiation.svg)

> [!reference] 개념과 사용 조건
> **목적** · 같은 선형 전이를 매우 많이 반복할 때 전이 행렬을 거듭제곱.
>
> **조건** · 정사각 행렬, exponent ≥ 0, 2 ≤ mod ≤ Integer.MAX_VALUE.
>
> **반환·비용** · A^exponent mod mod. 지수 0이면 단위행렬. d×d 행렬은 O(d³ log exponent).

---

## Template

행렬 곱의 입력은 0..mod−1로 정규화된 값. matrixPower는 입력을 복사하면서 정규화한다.

**Structure**

<pre class="algorithm-code" data-language="text"><code>multiply(a, b) {
    <span class="algorithm-keyword">for</span> (출발 행 i) {
        <span class="algorithm-keyword">for</span> (중간 인덱스 k) {
            <span class="algorithm-keyword">for</span> (도착 열 j) { <span class="algorithm-comment">/* a[i][k] × b[k][j] 누적 */</span> }
        }
    }
    <span class="algorithm-keyword">return</span> 결과 행렬;
}

matrixPower(a, exponent) {
    <span class="algorithm-comment">// 결과 = 단위행렬</span>
    <span class="algorithm-keyword">while</span> (exponent &gt; 0) {
        <span class="algorithm-keyword">if</span> (현재 비트가 1이면) { <span class="algorithm-comment">/* 결과 × a */</span> }
        <span class="algorithm-comment">// a 제곱 → 다음 비트</span>
    }
    <span class="algorithm-keyword">return</span> 결과;
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">long</span>[][] multiply(<span class="algorithm-keyword">long</span>[][] a, <span class="algorithm-keyword">long</span>[][] b, <span class="algorithm-keyword">int</span> mod) {
    <span class="algorithm-keyword">int</span> n = a.length;
    <span class="algorithm-keyword">long</span>[][] result = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[n][n];
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = 0; i &lt; n; i++) {
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> k = 0; k &lt; n; k++) {
            <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> j = 0; j &lt; n; j++) {
                result[i][j] = (result[i][j] + a[i][k] * b[k][j]) % mod;
            }
        }
    }
    <span class="algorithm-keyword">return</span> result;
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">long</span>[][] matrixPower(<span class="algorithm-keyword">long</span>[][] matrix, <span class="algorithm-keyword">long</span> exponent, <span class="algorithm-keyword">int</span> mod) {
    <span class="algorithm-keyword">int</span> n = matrix.length;
    <span class="algorithm-keyword">long</span>[][] result = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[n][n], base = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[n][n];
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = 0; i &lt; n; i++) {
        result[i][i] = 1;    <span class="algorithm-comment">// 지수 0에도 행렬 곱의 항등원 반환</span>
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> j = 0; j &lt; n; j++) {
            base[i][j] = Math.floorMod(matrix[i][j], (<span class="algorithm-keyword">long</span>) mod);
        }
    }

    <span class="algorithm-keyword">while</span> (exponent &gt; 0) {
        <span class="algorithm-keyword">if</span> ((exponent &amp; 1) != 0) result = multiply(result, base, mod);
        base = multiply(base, base, mod);
        exponent &gt;&gt;= 1;
    }
    <span class="algorithm-keyword">return</span> result;
}</code></pre>

[[Modular Arithmetic]]
