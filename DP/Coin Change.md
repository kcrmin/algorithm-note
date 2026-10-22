---
tags:
  - dp
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Coin Change

![Coin Change memory cue](../attachments/reference-coin-change.svg)

> [!reference] 개념과 사용 조건
> **목적** · 동전을 무제한 사용해 정확히 amount를 만든다.
>
> **조건** · coins는 중복 없는 양의 정수, `amount ≥ 0`. 경우의 수는 `mod ≥ 2`로 나눈다.
>
> **초기값** · 합 0의 최소 개수는 0, 합 0을 만드는 경우의 수는 1.
>
> **비용** · O(coins.length × amount) 시간, O(amount) 공간.

---

## Minimum Count

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// dp[0] = 0, 나머지는 불가능</span>
<span class="algorithm-keyword">for</span> (작은 합부터) {
    <span class="algorithm-keyword">for</span> (쓸 수 있는 각 동전) {
        <span class="algorithm-comment">// 이전 합의 최소 개수 + 1로 갱신</span>
    }
}
<span class="algorithm-keyword">return</span> 최소 개수 또는 -1;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">import</span> java.util.*;

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span> minCoins(<span class="algorithm-keyword">int</span>[] coins, <span class="algorithm-keyword">int</span> amount) {
    <span class="algorithm-keyword">int</span> impossible = amount + 1;
    <span class="algorithm-keyword">int</span>[] dp = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[amount + 1];
    Arrays.fill(dp, impossible);
    dp[0] = 0;

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> sum = 1; sum &lt;= amount; sum++) {
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> coin : coins) {
            <span class="algorithm-keyword">if</span> (coin &lt;= sum) {
                dp[sum] = Math.min(dp[sum], dp[sum - coin] + 1);
            }
        }
    }

    <span class="algorithm-keyword">return</span> dp[amount] == impossible ? -1 : dp[amount];
}</code></pre>

---

## Ordered Count · 1+2 ≠ 2+1

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// dp[0] = 1</span>
<span class="algorithm-keyword">for</span> (작은 합부터) {
    <span class="algorithm-keyword">for</span> (마지막에 붙일 각 동전) {
        <span class="algorithm-comment">// 그 동전을 붙이기 전 합의 경우의 수 누적</span>
    }
}
<span class="algorithm-keyword">return</span> dp[amount];</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">long</span> countOrdered(<span class="algorithm-keyword">int</span>[] coins, <span class="algorithm-keyword">int</span> amount, <span class="algorithm-keyword">int</span> mod) {
    <span class="algorithm-keyword">long</span>[] dp = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[amount + 1];
    dp[0] = 1;

    <mark class="algorithm-change"><span class="algorithm-keyword">for</span></mark><mark class="algorithm-change"> (</mark><mark class="algorithm-change"><span class="algorithm-keyword">int</span></mark><mark class="algorithm-change"> sum = 1; sum &lt;= amount; sum++)</mark> {
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> coin : coins) {    <span class="algorithm-comment">// 마지막 동전이 다르면 다른 순서로 센다</span>
            <span class="algorithm-keyword">if</span> (coin &lt;= sum) {
                dp[sum] = (dp[sum] + dp[sum - coin]) % mod;
            }
        }
    }

    <span class="algorithm-keyword">return</span> dp[amount];
}</code></pre>

---

## Unordered Count · 1+2 = 2+1

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// dp[0] = 1</span>
<span class="algorithm-keyword">for</span> (동전 종류를 하나씩 허용하며) {
    <span class="algorithm-keyword">for</span> (작은 합부터) {
        <span class="algorithm-comment">// 이번 동전을 추가하는 경우 누적</span>
    }
}
<span class="algorithm-keyword">return</span> dp[amount];</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">long</span> countUnordered(<span class="algorithm-keyword">int</span>[] coins, <span class="algorithm-keyword">int</span> amount, <span class="algorithm-keyword">int</span> mod) {
    <span class="algorithm-keyword">long</span>[] dp = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[amount + 1];
    dp[0] = 1;

    <mark class="algorithm-change"><span class="algorithm-keyword">for</span></mark><mark class="algorithm-change"> (</mark><mark class="algorithm-change"><span class="algorithm-keyword">int</span></mark><mark class="algorithm-change"> coin : coins)</mark> {    <span class="algorithm-comment">// 종류를 고정된 순서로 도입해 순서 중복 제거</span>
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> sum = coin; sum &lt;= amount; sum++) {
            dp[sum] = (dp[sum] + dp[sum - coin]) % mod;
        }
    }

    <span class="algorithm-keyword">return</span> dp[amount];
}</code></pre>

[[Knapsack DP]]
