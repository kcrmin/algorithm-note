---
tags:
  - math
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# GCD and LCM

![GCD and LCM memory cue](../attachments/reference-gcd-and-lcm.svg)

> [!reference] 개념과 사용 조건
> **목적** · 최대공약수와 최소공배수.
>
> **조건** · a, b는 0 이상의 long. gcd(0, 0) = 0, 한쪽이 0이면 lcm = 0.
>
> **비용** · GCD O(log min(a, b)), 공간 O(1). LCM이 long을 넘으면 예외를 낸다.

---

## Template

두 수 a, b의 최대공약수와 최소공배수를 구한다.

**Structure**

<pre class="algorithm-code" data-language="text"><code>gcd(a, b) {
    <span class="algorithm-keyword">while</span> (b가 0이 아니면) {
        <span class="algorithm-comment">// 나머지 계산 → (b, 나머지)로 이동</span>
    }
    <span class="algorithm-keyword">return</span> a;
}

lcm(a, b) {
    <span class="algorithm-keyword">if</span> (어느 한쪽이 0이면) <span class="algorithm-keyword">return</span> 0;
    <span class="algorithm-keyword">return</span> a를 gcd로 나눈 뒤 b를 곱한 값;
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">long</span> gcd(<span class="algorithm-keyword">long</span> a, <span class="algorithm-keyword">long</span> b) {
    <span class="algorithm-keyword">while</span> (b != 0) {
        <span class="algorithm-keyword">long</span> remainder = a % b;
        a = b;
        b = remainder;
    }
    <span class="algorithm-keyword">return</span> a;
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">long</span> lcm(<span class="algorithm-keyword">long</span> a, <span class="algorithm-keyword">long</span> b) {
    <span class="algorithm-keyword">if</span> (a == 0 || b == 0) <span class="algorithm-keyword">return</span> 0;
    <span class="algorithm-keyword">return</span> Math.multiplyExact(a / gcd(a, b), b);    <span class="algorithm-comment">// 먼저 나눠 중간 곱의 overflow 방지</span>
}</code></pre>
