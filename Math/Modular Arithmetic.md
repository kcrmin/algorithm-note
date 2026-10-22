---
tags:
  - math
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Modular Arithmetic

![Modular Arithmetic memory cue](../attachments/reference-modular-arithmetic.svg)

> [!reference] 개념과 사용 조건
> **목적** · 큰 지수의 거듭제곱과 소수 모듈러에서 나눗셈 처리.
>
> **범위** · 2 ≤ mod ≤ Integer.MAX_VALUE, exponent ≥ 0. 나머지끼리의 곱은 long 안에 들어간다.
>
> **Inverse** · prime은 소수이고 a가 prime의 배수가 아닐 때만 a^(prime−2)가 역원이다.
>
> **비용** · 거듭제곱 O(log exponent), 공간 O(1).

---

## Power and Prime Inverse

**Structure**

<pre class="algorithm-code" data-language="text"><code>powMod(base, exponent, mod) {
    <span class="algorithm-comment">// base를 0..mod−1로 정규화</span>
    <span class="algorithm-keyword">while</span> (exponent &gt; 0) {
        <span class="algorithm-keyword">if</span> (현재 비트가 1이면) { <span class="algorithm-comment">/* 결과에 base 곱하기 */</span> }
        <span class="algorithm-comment">// base 제곱 → 다음 비트로 이동</span>
    }
    <span class="algorithm-keyword">return</span> result;
}

inversePrime(a, prime) {
    <span class="algorithm-keyword">return</span> powMod(a, prime − 2, prime);
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">long</span> powMod(<span class="algorithm-keyword">long</span> base, <span class="algorithm-keyword">long</span> exponent, <span class="algorithm-keyword">int</span> mod) {
    base %= mod;
    <span class="algorithm-keyword">if</span> (base &lt; 0) base += mod;    <span class="algorithm-comment">// Java의 음수 나머지를 양수 범위로 복원</span>
    <span class="algorithm-keyword">long</span> result = 1;

    <span class="algorithm-keyword">while</span> (exponent &gt; 0) {
        <span class="algorithm-keyword">if</span> ((exponent &amp; 1) != 0) {
            result = result * base % mod;
        }
        base = base * base % mod;
        exponent &gt;&gt;= 1;
    }
    <span class="algorithm-keyword">return</span> result;
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">long</span> inversePrime(<span class="algorithm-keyword">long</span> a, <span class="algorithm-keyword">int</span> prime) {
    <span class="algorithm-keyword">if</span> (a % prime == 0) <span class="algorithm-keyword">throw</span> <span class="algorithm-keyword">new</span> IllegalArgumentException("inverse does not exist");
    <span class="algorithm-keyword">return</span> powMod(a, prime - 2L, prime);    <span class="algorithm-comment">// 소수 모듈러에서만 Fermat 적용</span>
}</code></pre>
