---
tags:
  - dp
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# LIS

![LIS memory cue](../attachments/reference-lis.svg)

> [!reference] 개념과 사용 조건
> **목적** · 원래 순서를 유지하는 가장 긴 증가 부분 수열.
>
> **상태** · `tails[k]` = 길이 k+1인 수열의 최소 끝값. tails 자체는 실제 부분 수열이 아닐 수 있다.
>
> **변형** · Strict는 첫 `≥ value`, Non-decreasing은 첫 `> value` 위치를 바꾼다.
>
> **비용** · O(n log n) 시간, O(n) 공간. 빈 입력의 길이는 0.

---

## Strictly Increasing

같은 값을 이어 붙이지 않는 최장 증가 부분 수열의 길이를 구한다.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-keyword">for</span> (각 값 value) {
    <span class="algorithm-comment">// tails에서 첫 ≥ value 위치 찾기</span>
    <span class="algorithm-comment">// 그 위치를 value로 교체</span>
    <span class="algorithm-keyword">if</span> (끝에 추가했다면) {
        <span class="algorithm-comment">// 길이 증가</span>
    }
}
<span class="algorithm-keyword">return</span> length;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span> lisLength(<span class="algorithm-keyword">int</span>[] a) {
    <span class="algorithm-keyword">int</span>[] tails = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[a.length];
    <span class="algorithm-keyword">int</span> length = 0;

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> value : a) {
        <span class="algorithm-keyword">int</span> left = 0, right = length;
        <span class="algorithm-keyword">while</span> (left &lt; right) {
            <span class="algorithm-keyword">int</span> mid = left + (right - left) / 2;
            <span class="algorithm-keyword">if</span> (<mark class="algorithm-change">tails[mid] &gt;= value</mark>) {    <span class="algorithm-comment">// 같은 값은 길이를 늘리지 않음</span>
                right = mid;
            } <span class="algorithm-keyword">else</span> {
                left = mid + 1;
            }
        }
        tails[left] = value;
        <span class="algorithm-keyword">if</span> (left == length) {
            length++;
        }
    }

    <span class="algorithm-keyword">return</span> length;
}</code></pre>

---

## Non-decreasing

같은 값도 이어 붙일 수 있는 최장 비감소 부분 수열의 길이를 구한다.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-keyword">for</span> (각 값 value) {
    <span class="algorithm-comment">// tails에서 첫 &gt; value 위치 찾기</span>
    <span class="algorithm-comment">// 그 위치를 value로 교체</span>
    <span class="algorithm-keyword">if</span> (끝에 추가했다면) {
        <span class="algorithm-comment">// 길이 증가</span>
    }
}
<span class="algorithm-keyword">return</span> length;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span> nonDecreasingLength(<span class="algorithm-keyword">int</span>[] a) {
    <span class="algorithm-keyword">int</span>[] tails = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[a.length];
    <span class="algorithm-keyword">int</span> length = 0;

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> value : a) {
        <span class="algorithm-keyword">int</span> left = 0, right = length;
        <span class="algorithm-keyword">while</span> (left &lt; right) {
            <span class="algorithm-keyword">int</span> mid = left + (right - left) / 2;
            <span class="algorithm-keyword">if</span> (<mark class="algorithm-change">tails[mid] &gt; value</mark>) {    <span class="algorithm-comment">// 같은 값 뒤에 이어 붙일 수 있음</span>
                right = mid;
            } <span class="algorithm-keyword">else</span> {
                left = mid + 1;
            }
        }
        tails[left] = value;
        <span class="algorithm-keyword">if</span> (left == length) {
            length++;
        }
    }

    <span class="algorithm-keyword">return</span> length;
}</code></pre>

---

## Restore · Strictly Increasing

최장 증가 부분 수열 하나를 골라 실제 원소 배열로 복원한다.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-keyword">for</span> (각 인덱스 i) {
    <span class="algorithm-comment">// 첫 ≥ a[i] 위치 찾기</span>
    <span class="algorithm-comment">// 직전 길이의 끝 인덱스를 previous[i]에 기록</span>
    <span class="algorithm-comment">// 현재 길이의 끝 인덱스 교체</span>
}

<span class="algorithm-comment">// 가장 긴 수열의 끝에서 previous를 따라 역순 복원</span>
<span class="algorithm-keyword">return</span> result;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">import</span> java.util.*;

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span>[] restoreLis(<span class="algorithm-keyword">int</span>[] a) {
    <span class="algorithm-keyword">int</span>[] tailIndex = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[a.length], previous = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[a.length];
    Arrays.fill(previous, -1);
    <span class="algorithm-keyword">int</span> length = 0;

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = 0; i &lt; a.length; i++) {
        <span class="algorithm-keyword">int</span> left = 0, right = length;
        <span class="algorithm-keyword">while</span> (left &lt; right) {
            <span class="algorithm-keyword">int</span> mid = left + (right - left) / 2;
            <span class="algorithm-keyword">if</span> (a[tailIndex[mid]] &gt;= a[i]) {
                right = mid;
            } <span class="algorithm-keyword">else</span> {
                left = mid + 1;
            }
        }
        <span class="algorithm-keyword">if</span> (left &gt; 0) {
            previous[i] = tailIndex[left - 1];    <span class="algorithm-comment">// 교체되기 전 실제 선행 원소를 보관</span>
        }
        tailIndex[left] = i;
        <span class="algorithm-keyword">if</span> (left == length) {
            length++;
        }
    }

    <span class="algorithm-keyword">int</span>[] result = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[length];
    <span class="algorithm-keyword">int</span> index = length == 0 ? -1 : tailIndex[length - 1];
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = length - 1; i &gt;= 0; i--) {
        result[i] = a[index];
        index = previous[index];
    }
    <span class="algorithm-keyword">return</span> result;
}</code></pre>

[[Binary Search]]
