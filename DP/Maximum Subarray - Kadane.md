---
tags:
  - dp
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Maximum Subarray - Kadane

![Maximum Subarray - Kadane memory cue](../attachments/reference-maximum-subarray-kadane.svg)

> [!reference] 개념과 사용 조건
> **목적** · 연속 부분 배열의 최대 합. 음수를 포함해도 된다.
>
> **상태** · `ending` = 현재 원소에서 끝나는 최대 합, `best` = 전체 최대 합.
>
> **조건** · 입력은 비어 있지 않다. 적어도 한 원소를 선택한다.
>
> **비용** · O(n) 시간, O(1) 공간. 합은 long 범위.

---

## Template

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// 첫 원소로 ending과 best 초기화</span>
<span class="algorithm-keyword">for</span> (두 번째 원소부터) {
    <span class="algorithm-comment">// ending = 새로 시작 / 이전 구간에 연결 중 큰 값</span>
    <span class="algorithm-comment">// best 갱신</span>
}
<span class="algorithm-keyword">return</span> best;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">long</span> maxSubarray(<span class="algorithm-keyword">int</span>[] a) {
    <span class="algorithm-keyword">long</span> ending = a[0], best = a[0];    <span class="algorithm-comment">// 0으로 시작하면 모두 음수일 때 빈 구간을 선택</span>

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = 1; i &lt; a.length; i++) {
        ending = Math.max(a[i], ending + a[i]);
        best = Math.max(best, ending);
    }

    <span class="algorithm-keyword">return</span> best;
}</code></pre>

[[Prefix Sum]] · [[Sliding Window]]
