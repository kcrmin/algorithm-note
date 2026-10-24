---
tags:
  - search
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Meet in the Middle

![Meet in the Middle memory cue](../attachments/reference-meet-in-the-middle.svg)

> [!reference] 개념과 사용 조건
> **목적** · 합이 target인 부분집합 개수. 음수·중복 원소도 가능.
>
> **조건** · `int[] a`, n ≤ 40. 빈 집합 포함; 같은 값이어도 인덱스가 다르면 다른 선택.
>
> **비용** · 절반 크기 m ≈ n/2. 시간 O(m·2ᵐ), 공간 O(2ᵐ).

---

## Count Subset Sums

배열에서 합이 `target`인 부분집합의 개수를 센다.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// 양쪽 부분집합 합 생성 → 각각 정렬</span>
<span class="algorithm-keyword">while</span> (양쪽 후보가 남았으면) {
    <span class="algorithm-keyword">if</span> (합 &lt; target) {
        <span class="algorithm-comment">// 왼쪽 합을 키움</span>
    } <span class="algorithm-keyword">else</span> <span class="algorithm-keyword">if</span> (합 &gt; target) {
        <span class="algorithm-comment">// 오른쪽 합을 줄임</span>
    } <span class="algorithm-keyword">else</span> {
        <span class="algorithm-comment">// 같은 값의 개수끼리 곱해 answer에 추가</span>
    }
}
<span class="algorithm-keyword">return</span> answer;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">long</span> countSubsets(<span class="algorithm-keyword">int</span>[] a, <span class="algorithm-keyword">long</span> target) {
    <span class="algorithm-keyword">if</span> (a.length &gt; 40) <span class="algorithm-keyword">throw</span> <span class="algorithm-keyword">new</span> IllegalArgumentException();
    <span class="algorithm-keyword">int</span> mid = a.length / 2;
    <span class="algorithm-keyword">long</span>[] leftSums = halfSums(a, 0, mid);
    <span class="algorithm-keyword">long</span>[] rightSums = halfSums(a, mid, a.length);
    Arrays.sort(leftSums);
    Arrays.sort(rightSums);
    <span class="algorithm-keyword">int</span> left = 0, right = rightSums.length - 1;
    <span class="algorithm-keyword">long</span> answer = 0;

    <span class="algorithm-keyword">while</span> (left &lt; leftSums.length &amp;&amp; right &gt;= 0) {
        <span class="algorithm-keyword">long</span> sum = leftSums[left] + rightSums[right];
        <span class="algorithm-keyword">if</span> (sum &lt; target) {
            left++;
        } <span class="algorithm-keyword">else</span> <span class="algorithm-keyword">if</span> (sum &gt; target) {
            right--;
        } <span class="algorithm-keyword">else</span> {
            <span class="algorithm-keyword">long</span> leftSum = leftSums[left], rightSum = rightSums[right];
            <span class="algorithm-keyword">long</span> leftCount = 0, rightCount = 0;
            <span class="algorithm-keyword">while</span> (left &lt; leftSums.length &amp;&amp; leftSums[left] == leftSum) {
                left++;
                leftCount++;
            }
            <span class="algorithm-keyword">while</span> (right &gt;= 0 &amp;&amp; rightSums[right] == rightSum) {
                right--;
                rightCount++;
            }
            answer += leftCount * rightCount;    <span class="algorithm-comment">// 같은 합을 만드는 서로 다른 선택을 모두 셈</span>
        }
    }
    <span class="algorithm-keyword">return</span> answer;
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">long</span>[] halfSums(<span class="algorithm-keyword">int</span>[] a, <span class="algorithm-keyword">int</span> from, <span class="algorithm-keyword">int</span> to) {
    <span class="algorithm-keyword">long</span>[] sums = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[1 &lt;&lt; (to - from)];
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> mask = 1; mask &lt; sums.length; mask++) {
        <span class="algorithm-keyword">int</span> bit = Integer.numberOfTrailingZeros(mask);
        sums[mask] = sums[mask &amp; (mask - 1)] + a[from + bit];
    }
    <span class="algorithm-keyword">return</span> sums;
}</code></pre>

[[Bitmask]] · [[Two Pointers]]
