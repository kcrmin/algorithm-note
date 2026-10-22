---
tags:
  - search
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Two Pointers

![Two Pointers memory cue](../attachments/reference-two-pointers.svg)

> [!reference] 개념과 사용 조건
> **목적** · 서로 다른 두 원소의 합이 `target`인지 검사.
>
> **조건** · `a`는 오름차순 정렬. 정렬 비용을 제외하면 시간 O(n), 공간 O(1).
>
> **판단** · 합이 작으면 작은 값을 키우고, 합이 크면 큰 값을 줄인다.

---

## Pair Sum

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-keyword">while</span> (left &lt; right) {
    <span class="algorithm-comment">// 두 값의 합 계산</span>
    <span class="algorithm-keyword">if</span> (합 == target) <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">true</span>;
    <span class="algorithm-keyword">if</span> (합 &lt; target) {
        <span class="algorithm-comment">// left를 다음으로</span>
    } <span class="algorithm-keyword">else</span> {
        <span class="algorithm-comment">// right를 이전으로</span>
    }
}
<span class="algorithm-keyword">return</span> <span class="algorithm-keyword">false</span>;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">boolean</span> hasPair(<span class="algorithm-keyword">int</span>[] a, <span class="algorithm-keyword">long</span> target) {
    <span class="algorithm-keyword">int</span> left = 0, right = a.length - 1;

    <span class="algorithm-keyword">while</span> (left &lt; right) {
        <span class="algorithm-keyword">long</span> sum = (<span class="algorithm-keyword">long</span>) a[left] + a[right];    <span class="algorithm-comment">// 더하기 전에 long으로 승격</span>
        <span class="algorithm-keyword">if</span> (sum == target) <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">true</span>;

        <span class="algorithm-keyword">if</span> (sum &lt; target) {
            left++;
        } <span class="algorithm-keyword">else</span> {
            right--;
        }
    }
    <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">false</span>;
}</code></pre>

[[Binary Search]] · [[Sliding Window]]
