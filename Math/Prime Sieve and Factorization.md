---
tags:
  - math
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Prime Sieve and Factorization

![Prime Sieve and Factorization memory cue](../attachments/reference-prime-sieve-and-factorization.svg)

> [!reference] 개념과 사용 조건
> **목적** · 0..n의 소수 판정 또는 여러 수의 소인수분해를 전처리.
>
> **조건** · n ≥ 0. SPF 분해는 1 ≤ x ≤ n. 1의 소인수 목록은 비어 있다.
>
> **비용** · 체 O(n log log n), 공간 O(n). SPF 분해 한 번은 O(log x).

---

## Prime Sieve

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// 처음에는 모두 소수 후보, 0과 1 제외</span>
<span class="algorithm-keyword">for</span> (p가 √n 이하인 동안) {
    <span class="algorithm-keyword">if</span> (p가 소수이면) {
        <span class="algorithm-keyword">for</span> (p²부터 p의 배수) { <span class="algorithm-comment">/* 소수 아님 표시 */</span> }
    }
}
<span class="algorithm-keyword">return</span> 소수 여부 배열;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">boolean</span>[] primeSieve(<span class="algorithm-keyword">int</span> n) {
    <span class="algorithm-keyword">boolean</span>[] prime = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">boolean</span>[n + 1];
    Arrays.fill(prime, <span class="algorithm-keyword">true</span>);
    prime[0] = <span class="algorithm-keyword">false</span>;
    <span class="algorithm-keyword">if</span> (n &gt;= 1) prime[1] = <span class="algorithm-keyword">false</span>;

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> p = 2; p &lt;= n / p; p++) {
        <span class="algorithm-keyword">if</span> (!prime[p]) <span class="algorithm-keyword">continue</span>;
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">long</span> multiple = (<span class="algorithm-keyword">long</span>) p * p; multiple &lt;= n; multiple += p) {
            prime[(<span class="algorithm-keyword">int</span>) multiple] = <span class="algorithm-keyword">false</span>;    <span class="algorithm-comment">// p² 미만의 배수는 더 작은 소수에서 처리됨</span>
        }
    }
    <span class="algorithm-keyword">return</span> prime;
}</code></pre>

---

## SPF and Factorization

smallestPrimeFactors(n)으로 한 번 만든 배열을 factorize(x, spf)에 재사용한다.

**Structure**

<pre class="algorithm-code" data-language="text"><code>smallestPrimeFactors(n) {
    <span class="algorithm-keyword">for</span> (각 p) {
        <span class="algorithm-keyword">if</span> (최소 소인수가 아직 없으면) {
            <span class="algorithm-comment">// p 자신은 소수</span>
            <span class="algorithm-keyword">for</span> (p²부터 배수) {
                <span class="algorithm-keyword">if</span> (아직 비어 있으면) { <span class="algorithm-comment">/* 최소 소인수 = p */</span> }
            }
        }
    }
    <span class="algorithm-keyword">return</span> spf;
}

factorize(x, spf) {
    <span class="algorithm-keyword">while</span> (x &gt; 1) {
        <span class="algorithm-comment">// spf[x]로 나눌 수 있는 만큼 나눔 → 소수와 지수 기록</span>
    }
    <span class="algorithm-keyword">return</span> 소인수 목록;
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> Factor {
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span> prime, exponent;

    Factor(<span class="algorithm-keyword">int</span> prime, <span class="algorithm-keyword">int</span> exponent) {
        <span class="algorithm-keyword">this</span>.prime = prime;
        <span class="algorithm-keyword">this</span>.exponent = exponent;
    }
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span>[] smallestPrimeFactors(<span class="algorithm-keyword">int</span> n) {
    <span class="algorithm-keyword">int</span>[] spf = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[n + 1];
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> p = 2; p &lt;= n; p++) {
        <span class="algorithm-keyword">if</span> (spf[p] != 0) <span class="algorithm-keyword">continue</span>;
        spf[p] = p;
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">long</span> multiple = (<span class="algorithm-keyword">long</span>) p * p; multiple &lt;= n; multiple += p) {
            <span class="algorithm-keyword">int</span> value = (<span class="algorithm-keyword">int</span>) multiple;
            <span class="algorithm-keyword">if</span> (spf[value] == 0) spf[value] = p;    <span class="algorithm-comment">// 처음 기록한 소수가 가장 작음</span>
        }
    }
    <span class="algorithm-keyword">return</span> spf;
}

<span class="algorithm-keyword">static</span> List&lt;Factor&gt; factorize(<span class="algorithm-keyword">int</span> x, <span class="algorithm-keyword">int</span>[] spf) {
    List&lt;Factor&gt; factors = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;();
    <span class="algorithm-keyword">while</span> (x &gt; 1) {
        <span class="algorithm-keyword">int</span> p = spf[x], count = 0;
        <span class="algorithm-keyword">while</span> (x % p == 0) {
            x /= p;
            count++;
        }
        factors.add(<span class="algorithm-keyword">new</span> Factor(p, count));
    }
    <span class="algorithm-keyword">return</span> factors;
}</code></pre>
