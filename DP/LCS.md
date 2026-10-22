---
tags:
  - dp
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# LCS

![LCS memory cue](../attachments/reference-lcs.svg)

> [!reference] 개념과 사용 조건
> **목적** · 두 문자열에서 순서를 유지하는 최장 공통 부분 수열.
>
> **상태** · `dp[i][j]` = 앞 i글자와 앞 j글자의 LCS 길이. 연속할 필요는 없다.
>
> **반환** · LCS 한 개. 여러 정답 중 하나를 반환하며, 공통 문자가 없으면 빈 문자열.
>
> **비용** · O(nm) 시간·공간. Java char 단위 비교.

---

## Length and Restore

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-keyword">for</span> (첫 문자열의 각 prefix) {
    <span class="algorithm-keyword">for</span> (둘째 문자열의 각 prefix) {
        <span class="algorithm-keyword">if</span> (마지막 문자가 같으면) {
            <span class="algorithm-comment">// 두 문자를 쓰기: 대각선 + 1</span>
        } <span class="algorithm-keyword">else</span> {
            <span class="algorithm-comment">// 어느 쪽 한 문자를 버릴지 선택</span>
        }
    }
}

<span class="algorithm-keyword">while</span> (두 prefix가 모두 남으면) {
    <span class="algorithm-comment">// 같은 문자면 기록하고 대각선, 아니면 더 큰 상태로 이동</span>
}
<span class="algorithm-keyword">return</span> 뒤집은 결과;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> String lcs(String a, String b) {
    <span class="algorithm-keyword">int</span> n = a.length(), m = b.length();
    <span class="algorithm-keyword">int</span>[][] dp = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[n + 1][m + 1];

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = 1; i &lt;= n; i++) {
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> j = 1; j &lt;= m; j++) {
            <span class="algorithm-keyword">if</span> (a.charAt(i - 1) == b.charAt(j - 1)) {
                dp[i][j] = dp[i - 1][j - 1] + 1;
            } <span class="algorithm-keyword">else</span> {
                dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
    }

    StringBuilder result = <span class="algorithm-keyword">new</span> StringBuilder();
    <span class="algorithm-keyword">int</span> i = n, j = m;
    <span class="algorithm-keyword">while</span> (i &gt; 0 &amp;&amp; j &gt; 0) {
        <span class="algorithm-keyword">if</span> (a.charAt(i - 1) == b.charAt(j - 1)) {
            result.append(a.charAt(i - 1));
            i--;
            j--;
        } <span class="algorithm-keyword">else</span> <span class="algorithm-keyword">if</span> (dp[i - 1][j] &gt;= dp[i][j - 1]) {
            i--;
        } <span class="algorithm-keyword">else</span> {
            j--;
        }
    }

    <span class="algorithm-keyword">return</span> result.reverse().toString();    <span class="algorithm-comment">// 끝 문자부터 모았기 때문</span>
}</code></pre>

[[Edit Distance]]
