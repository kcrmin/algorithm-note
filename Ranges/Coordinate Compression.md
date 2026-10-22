---
tags:
  - ranges
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Coordinate Compression

![Coordinate Compression memory cue](../attachments/reference-coordinate-compression.svg)

> [!reference] 개념과 사용 조건
> **목적** · 큰 값·음수를 순서를 유지하는 작은 인덱스로 바꾼다.
>
> **반환** · 같은 값은 같은 번호. 압축 결과는 0부터 시작.
>
> **조건** · 값 사이의 거리·간격은 보존하지 않는다. 원래 배열은 변경하지 않는다.
>
> **비용** · O(n log n) 시간, O(n) 공간.

---

## Template

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// 복사 → 정렬</span>
<span class="algorithm-keyword">for</span> (정렬된 각 값) {
    <span class="algorithm-keyword">if</span> (직전의 고유값과 다르면) {
        <span class="algorithm-comment">// 고유값 목록에 추가</span>
    }
}
<span class="algorithm-keyword">for</span> (원래 배열의 각 값) {
    <span class="algorithm-comment">// 고유값 목록에서 위치 검색</span>
}
<span class="algorithm-keyword">return</span> rank;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">import</span> java.util.*;

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span>[] compress(<span class="algorithm-keyword">int</span>[] a) {
    <span class="algorithm-keyword">int</span>[] sorted = a.clone();
    Arrays.sort(sorted);
    <span class="algorithm-keyword">int</span> size = 0;

    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> value : sorted) {
        <span class="algorithm-keyword">if</span> (size == 0 || sorted[size - 1] != value) {
            sorted[size++] = value;
        }
    }

    <span class="algorithm-keyword">int</span>[] rank = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[a.length];
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = 0; i &lt; a.length; i++) {
        rank[i] = Arrays.binarySearch(sorted, 0, size, a[i]);    <span class="algorithm-comment">// 뒤의 중복 잔여값은 제외</span>
    }

    <span class="algorithm-keyword">return</span> rank;
}</code></pre>

[[Fenwick Tree]] · [[Segment Tree]]
