---
tags:
  - dp
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Edit Distance

![Edit Distance memory cue](../attachments/reference-edit-distance.svg)

> [!reference] 개념과 사용 조건
> **목적** · 삽입·삭제·교체로 a를 b로 바꾸는 최소 연산 수.
>
> **상태** · `dp[i][j]` = a의 앞 i글자를 b의 앞 j글자로 바꾸는 최소 비용.
>
> **조건** · 각 연산 비용은 1. 같은 문자 유지 비용은 0. Java char 단위.
>
> **비용** · O(nm) 시간·공간.

---

## Template

문자열 a를 b로 바꾸는 데 필요한 삽입·삭제·교체의 최소 횟수를 구한다.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// 빈 문자열로 바꾸는 비용 = 삭제 횟수</span>
<span class="algorithm-comment">// 빈 문자열에서 만드는 비용 = 삽입 횟수</span>
<span class="algorithm-keyword">for</span> (각 prefix 쌍) {
    <span class="algorithm-comment">// 삭제 / 삽입 / 교체·유지 중 최소 비용</span>
}
<span class="algorithm-keyword">return</span> dp[n][m];</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span> editDistance(String a, String b) {
    <span class="algorithm-keyword">int</span> n = a.length(), m = b.length();
    <span class="algorithm-keyword">int</span>[][] dp = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[n + 1][m + 1];

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = 0; i &lt;= n; i++) {
        dp[i][0] = i;
    }
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> j = 0; j &lt;= m; j++) {
        dp[0][j] = j;
    }

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = 1; i &lt;= n; i++) {
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> j = 1; j &lt;= m; j++) {
            <span class="algorithm-keyword">int</span> change = a.charAt(i - 1) == b.charAt(j - 1) ? 0 : 1;
            <span class="algorithm-keyword">int</span> delete = dp[i - 1][j] + 1;
            <span class="algorithm-keyword">int</span> insert = dp[i][j - 1] + 1;
            <span class="algorithm-keyword">int</span> replace = dp[i - 1][j - 1] + change;
            dp[i][j] = Math.min(replace, Math.min(delete, insert));
        }
    }

    <span class="algorithm-keyword">return</span> dp[n][m];
}</code></pre>

[[LCS]]
