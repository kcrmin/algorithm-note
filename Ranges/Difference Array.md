---
tags:
  - ranges
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Difference Array

![Difference Array memory cue](../attachments/reference-difference-array.svg)

> [!reference] 개념과 사용 조건
> **목적** · 여러 구간에 값을 더한 뒤 최종 배열을 한 번 구한다.
>
> **범위** · `[left, right)`에 delta 추가. `0 ≤ left ≤ right ≤ n`.
>
> **조건** · 초기 배열은 0. 갱신 중간의 질의는 처리하지 않는다. 누적값은 long 범위.
>
> **비용** · q개 갱신과 복원에 O(n + q).

---

## Template

0으로 시작한 배열에 모든 구간 덧셈을 반영한 최종 배열을 구한다.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-keyword">for</span> (각 구간 갱신) {
    <span class="algorithm-comment">// left에 +delta, right에 -delta</span>
}

<span class="algorithm-keyword">for</span> (왼쪽부터 각 위치) {
    <span class="algorithm-comment">// 차이를 sum에 누적해 원래 값 복원</span>
}
<span class="algorithm-keyword">return</span> result;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">import</span> java.util.*;

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> Update {
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span> left, right;
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">long</span> delta;

    Update(<span class="algorithm-keyword">int</span> left, <span class="algorithm-keyword">int</span> right, <span class="algorithm-keyword">long</span> delta) {
        <span class="algorithm-keyword">this</span>.left = left;
        <span class="algorithm-keyword">this</span>.right = right;
        <span class="algorithm-keyword">this</span>.delta = delta;
    }
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">long</span>[] applyUpdates(<span class="algorithm-keyword">int</span> n, List&lt;Update&gt; updates) {
    <span class="algorithm-keyword">long</span>[] diff = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[n + 1];    <span class="algorithm-comment">// right=n인 종료 경계도 기록</span>

    <span class="algorithm-keyword">for</span> (Update update : updates) {
        diff[update.left] += update.delta;
        diff[update.right] -= update.delta;
    }

    <span class="algorithm-keyword">long</span>[] result = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[n];
    <span class="algorithm-keyword">long</span> sum = 0;
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = 0; i &lt; n; i++) {
        sum += diff[i];
        result[i] = sum;
    }

    <span class="algorithm-keyword">return</span> result;
}</code></pre>

[[Prefix Sum]] · [[Lazy Segment Tree]]
