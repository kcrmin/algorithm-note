---
tags:
  - dp
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Interval DP

![Interval DP memory cue](../attachments/reference-interval-dp.svg)

> [!reference] 개념과 사용 조건
> **목적** · 짧은 구간의 결과로 큰 구간을 계산. 분할점 선택 / 양끝 선택.
>
> **상태** · `dp[left][right]`는 `[left, right)`의 결과. 무엇을 저장하는지는 변형마다 다르다.
>
> **조건** · 누적합·DP값·전이식의 덧셈·뺄셈이 long 범위에 들어와야 한다.
>
> **비용** · 분할점 선택 O(n³), 양끝 선택 O(n²). 공간은 모두 O(n²).

---

## Minimum Merge Cost

`dp[left][right]` = `[left, right)`를 하나로 합치는 최소 비용. size는 0 이상이며, 합치는 비용은 두 묶음의 크기 합. 길이 0·1의 비용은 0.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// prefix sum 생성, 길이 0·1의 비용은 0</span>
<span class="algorithm-keyword">for</span> (구간 길이를 2부터 늘리며) {
    <span class="algorithm-keyword">for</span> (각 시작점) {
        <span class="algorithm-keyword">for</span> (구간을 둘로 나누는 각 위치) {
            <span class="algorithm-comment">// 왼쪽 최소 비용 + 오른쪽 최소 비용 + 구간 전체 합</span>
        }
    }
}
<span class="algorithm-keyword">return</span> 전체 구간의 비용;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">long</span> minMergeCost(<span class="algorithm-keyword">int</span>[] size) {
    <span class="algorithm-keyword">int</span> n = size.length;
    <span class="algorithm-keyword">long</span>[] prefix = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[n + 1];
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = 0; i &lt; n; i++) {
        prefix[i + 1] = prefix[i] + size[i];
    }
    <span class="algorithm-keyword">long</span>[][] dp = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[n + 1][n + 1];

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> length = 2; length &lt;= n; length++) {    <span class="algorithm-comment">// 필요한 두 부분 구간을 먼저 계산</span>
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> left = 0; left + length &lt;= n; left++) {
            <span class="algorithm-keyword">int</span> right = left + length;
            dp[left][right] = Long.MAX_VALUE;

            <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> cut = left + 1; cut &lt; right; cut++) {
                <span class="algorithm-keyword">long</span> candidate = dp[left][cut] + dp[cut][right]
                        + prefix[right] - prefix[left];
                dp[left][right] = Math.min(dp[left][right], candidate);
            }
        }
    }

    <span class="algorithm-keyword">return</span> dp[0][n];
}</code></pre>

---

## Optimal End Picking

`dp[left][right]` = **현재 차례의 사람 점수 − 상대 점수**의 최댓값. 두 사람이 번갈아 양끝 중 하나를 가져가며, 모두 가져갈 때까지 각자 최선으로 선택한다.

음수도 허용. 빈 구간의 점수 차는 0, 한 원소만 남으면 그 값. 반환은 **선공 총점 − 후공 총점**이다.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// 빈 구간의 점수 차 = 0</span>
<span class="algorithm-keyword">for</span> (구간 길이를 1부터 늘리며) {
    <span class="algorithm-keyword">for</span> (각 시작점) {
        <span class="algorithm-comment">// left 선택: 왼쪽 값 − 남은 구간에서 상대가 얻는 점수 차</span>
        <span class="algorithm-comment">// right 선택: 오른쪽 값 − 남은 구간에서 상대가 얻는 점수 차</span>
        <span class="algorithm-comment">// 둘 중 큰 점수 차 선택</span>
    }
}
<span class="algorithm-keyword">return</span> 전체 구간의 점수 차;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">long</span> optimalScoreDifference(<span class="algorithm-keyword">int</span>[] values) {
    <span class="algorithm-keyword">int</span> n = values.length;
    <span class="algorithm-keyword">long</span>[][] dp = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[n + 1][n + 1];

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> length = 1; length &lt;= n; length++) {
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> left = 0; left + length &lt;= n; left++) {
            <span class="algorithm-keyword">int</span> right = left + length;
            <span class="algorithm-keyword">long</span> takeLeft = values[left] - dp[left + 1][right];    <span class="algorithm-comment">// 다음 차례는 상대이므로 남은 구간의 이득을 뺌</span>
            <span class="algorithm-keyword">long</span> takeRight = values[right - 1] - dp[left][right - 1];
            dp[left][right] = Math.max(takeLeft, takeRight);
        }
    }

    <span class="algorithm-keyword">return</span> dp[0][n];
}</code></pre>

[[Prefix Sum]]
