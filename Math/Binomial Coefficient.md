---
tags:
  - math
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Binomial Coefficient

![Binomial Coefficient memory cue](../attachments/reference-binomial-coefficient.svg)

> [!reference] 개념과 사용 조건
> **목적** · 순서를 구분하지 않고 n개 중 k개를 고르는 경우의 수.
>
> **Pascal** · 임의의 2 ≤ mod ≤ Integer.MAX_VALUE. 질의 한 번 O(nk), 공간 O(k).
>
> **Factorial** · prime은 소수, 전처리 한도 n < prime. 전처리 O(n + log prime), 질의 O(1).

---

## Pascal

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// C(0, 0) = 1</span>
<span class="algorithm-keyword">for</span> (행을 1부터 n까지) {
    <span class="algorithm-keyword">for</span> (열을 큰 쪽부터 1까지) {
        <span class="algorithm-comment">// 이전 행의 같은 열 + 이전 열</span>
    }
}
<span class="algorithm-keyword">return</span> C(n, k);</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">long</span> combinationPascal(<span class="algorithm-keyword">int</span> n, <span class="algorithm-keyword">int</span> k, <span class="algorithm-keyword">int</span> mod) {
    <span class="algorithm-keyword">if</span> (k &lt; 0 || k &gt; n) <span class="algorithm-keyword">return</span> 0;
    k = Math.min(k, n - k);
    <span class="algorithm-keyword">long</span>[] dp = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[k + 1];
    dp[0] = 1;

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> row = 1; row &lt;= n; row++) {
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> col = Math.min(row, k); col &gt;= 1; col--) {
            dp[col] = (dp[col] + dp[col - 1]) % mod;    <span class="algorithm-comment">// 역순이어야 이전 행의 값 사용</span>
        }
    }
    <span class="algorithm-keyword">return</span> dp[k];
}</code></pre>

---

## Factorial and Inverse Factorial

new Combinations(limit, prime)으로 만들고 choose(n, k)를 호출한다. 질의는 0 ≤ n ≤ limit.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-keyword">class</span> Combinations {
    constructor(limit, prime) {
        <span class="algorithm-comment">// factorial을 앞에서부터 계산</span>
        <span class="algorithm-comment">// 마지막 factorial의 역원 계산</span>
        <span class="algorithm-comment">// inverse factorial을 뒤에서부터 계산</span>
    }

    choose(n, k) {
        <span class="algorithm-keyword">if</span> (k가 범위 밖이면) <span class="algorithm-keyword">return</span> 0;
        <span class="algorithm-keyword">return</span> fact[n] × invFact[k] × invFact[n−k];
    }

    powMod(base, exponent) { <span class="algorithm-comment">/* 제곱하며 지수를 절반으로 */</span> }
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> Combinations {
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span> prime;
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">long</span>[] fact, invFact;

    Combinations(<span class="algorithm-keyword">int</span> limit, <span class="algorithm-keyword">int</span> prime) {
        <span class="algorithm-keyword">if</span> (limit &lt; 0 || limit &gt;= prime) <span class="algorithm-keyword">throw</span> <span class="algorithm-keyword">new</span> IllegalArgumentException("require 0 &lt;= limit &lt; prime");
        <span class="algorithm-keyword">this</span>.prime = prime;
        fact = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[limit + 1];
        invFact = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[limit + 1];
        fact[0] = 1;

        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = 1; i &lt;= limit; i++) {
            fact[i] = fact[i - 1] * i % prime;
        }
        invFact[limit] = powMod(fact[limit], prime - 2L);
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = limit; i &gt;= 1; i--) {
            invFact[i - 1] = invFact[i] * i % prime;
        }
    }

    <span class="algorithm-keyword">long</span> choose(<span class="algorithm-keyword">int</span> n, <span class="algorithm-keyword">int</span> k) {
        <span class="algorithm-keyword">if</span> (k &lt; 0 || k &gt; n) <span class="algorithm-keyword">return</span> 0;
        <span class="algorithm-keyword">return</span> fact[n] * invFact[k] % prime * invFact[n - k] % prime;
    }

    <span class="algorithm-keyword">long</span> powMod(<span class="algorithm-keyword">long</span> base, <span class="algorithm-keyword">long</span> exponent) {
        <span class="algorithm-keyword">long</span> result = 1;
        <span class="algorithm-keyword">while</span> (exponent &gt; 0) {
            <span class="algorithm-keyword">if</span> ((exponent &amp; 1) != 0) result = result * base % prime;
            base = base * base % prime;
            exponent &gt;&gt;= 1;
        }
        <span class="algorithm-keyword">return</span> result;
    }
}</code></pre>

[[Modular Arithmetic]]
