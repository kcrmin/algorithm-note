---
tags:
  - dp
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Bitmask DP

![Bitmask DP memory cue](../attachments/reference-bitmask-dp.svg)

> [!reference] 개념과 사용 조건
> **목적** · 0번에서 출발해 모든 정점을 한 번씩 방문하고 돌아오는 최소 비용: TSP.
>
> **상태** · `dp[mask][last]` = mask의 정점을 방문하고 last에서 끝나는 최소 비용.
>
> **조건** · `cost[u][v]`는 0 이상, 간선 없음은 -1. n ≤ 20이며 메모리 제한에 맞춰 사용.
>
> **비용** · O(n²·2ⁿ) 시간, O(n·2ⁿ) 공간. 경로 없음은 `Long.MAX_VALUE`.

---

## Traveling Salesman

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// 시작 정점만 방문한 비용 = 0</span>
<span class="algorithm-keyword">for</span> (각 방문 집합) {
    <span class="algorithm-keyword">for</span> (도달 가능한 마지막 정점) {
        <span class="algorithm-keyword">for</span> (아직 방문하지 않은 다음 정점) {
            <span class="algorithm-comment">// 정점 하나 추가 → 최소 비용 갱신</span>
        }
    }
}
<span class="algorithm-comment">// 모든 정점을 방문한 상태에서 0번으로 복귀</span>
<span class="algorithm-keyword">return</span> 최소 비용;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">import</span> java.util.*;

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">long</span> travelingSalesman(<span class="algorithm-keyword">int</span>[][] cost) {
    <span class="algorithm-keyword">int</span> n = cost.length;
    <span class="algorithm-keyword">if</span> (n &lt;= 1) {
        <span class="algorithm-keyword">return</span> 0;
    }
    <span class="algorithm-keyword">int</span> states = 1 &lt;&lt; n;
    <span class="algorithm-keyword">long</span>[][] dp = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[states][n];
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">long</span>[] row : dp) {
        Arrays.fill(row, Long.MAX_VALUE);
    }
    dp[1][0] = 0;

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> mask = 1; mask &lt; states; mask++) {
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> last = 0; last &lt; n; last++) {
            <span class="algorithm-keyword">if</span> (dp[mask][last] == Long.MAX_VALUE) {
                <span class="algorithm-keyword">continue</span>;    <span class="algorithm-comment">// 무한대에 간선 비용을 더하지 않음</span>
            }
            <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> next = 0; next &lt; n; next++) {
                <span class="algorithm-keyword">if</span> ((mask &amp; (1 &lt;&lt; next)) != 0 || cost[last][next] &lt; 0) {
                    <span class="algorithm-keyword">continue</span>;
                }
                <span class="algorithm-keyword">int</span> nextMask = mask | (1 &lt;&lt; next);
                <span class="algorithm-keyword">long</span> candidate = dp[mask][last] + cost[last][next];
                dp[nextMask][next] = Math.min(dp[nextMask][next], candidate);
            }
        }
    }

    <span class="algorithm-keyword">long</span> answer = Long.MAX_VALUE;
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> last = 1; last &lt; n; last++) {
        <span class="algorithm-keyword">if</span> (dp[states - 1][last] != Long.MAX_VALUE &amp;&amp; cost[last][0] &gt;= 0) {
            answer = Math.min(answer, dp[states - 1][last] + cost[last][0]);
        }
    }
    <span class="algorithm-keyword">return</span> answer;
}</code></pre>

[[Bitmask]] · [[Backtracking]]
