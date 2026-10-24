---
tags:
  - strings
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Rolling Hash

![Rolling Hash memory cue](../attachments/reference-rolling-hash.svg)

> [!reference] 개념과 사용 조건
> **목적** · 부분 문자열의 해시를 O(1)에 구한다.
>
> **구간** · 0-based [left, right). 문자는 Java char 단위.
>
> **조건** · 같은 해시는 같다는 증명이 아니다. 충돌을 허용할 수 없다면 실제 문자열도 확인한다.
>
> **비용** · 전처리 O(n), 질의 O(1), 공간 O(n).

---

## Template

문자열을 전처리해 부분 문자열의 해시를 구하고, 두 구간의 길이와 해시가 같은지 확인한다.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-keyword">class</span> RollingHash {
    constructor(text) {
        <span class="algorithm-keyword">for</span> (각 문자) {
            <span class="algorithm-comment">// 이전 해시 × BASE + 현재 문자</span>
            <span class="algorithm-comment">// BASE의 거듭제곱 저장</span>
        }
    }

    hash(left, right) {
        <span class="algorithm-comment">// 오른쪽 누적 해시 − 왼쪽 누적 해시 × BASE^(구간 길이)</span>
        <span class="algorithm-keyword">return</span> 음수가 되지 않게 보정한 나머지;
    }

    same(a, b) { <span class="algorithm-comment">/* 길이와 해시 모두 비교 */</span> }
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> RollingHash {
    <span class="algorithm-keyword">static</span> <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">long</span> MOD = 1_000_000_007L, BASE = 131L;
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">long</span>[] prefix, power;

    RollingHash(String text) {
        <span class="algorithm-keyword">int</span> n = text.length();
        prefix = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[n + 1];
        power = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[n + 1];
        power[0] = 1;

        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = 0; i &lt; n; i++) {
            prefix[i + 1] = (prefix[i] * BASE + text.charAt(i) + 1) % MOD;
            power[i + 1] = power[i] * BASE % MOD;
        }
    }

    <span class="algorithm-keyword">long</span> hash(<span class="algorithm-keyword">int</span> left, <span class="algorithm-keyword">int</span> right) {
        <span class="algorithm-keyword">long</span> removed = prefix[left] * power[right - left] % MOD;
        <span class="algorithm-keyword">return</span> (prefix[right] - removed + MOD) % MOD;    <span class="algorithm-comment">// Java의 음수 나머지 방지</span>
    }

    <span class="algorithm-keyword">boolean</span> same(<span class="algorithm-keyword">int</span> left1, <span class="algorithm-keyword">int</span> right1, <span class="algorithm-keyword">int</span> left2, <span class="algorithm-keyword">int</span> right2) {
        <span class="algorithm-keyword">return</span> right1 - left1 == right2 - left2
                &amp;&amp; hash(left1, right1) == hash(left2, right2);
    }
}</code></pre>
