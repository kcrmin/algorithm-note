---
tags:
  - intervals
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Interval Merging

![Interval Merging memory cue](../attachments/reference-interval-merging.svg)

> [!reference] 개념과 사용 조건
> **목적** · 겹치거나 맞닿는 닫힌 구간을 합친다.
>
> **조건** · start ≤ end. `[1,3]`과 `[3,5]`는 합치고, `[1,3]`과 `[4,5]`는 분리.
>
> **비용** · 시간 O(n log n), 입력 복사·결과 공간 O(n).

---

## Template

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// 시작점 오름차순 정렬</span>
<span class="algorithm-keyword">for</span> (각 구간) {
    <span class="algorithm-keyword">if</span> (이전 구간과 떨어져 있으면) {
        <span class="algorithm-comment">// 새 구간 추가</span>
    } <span class="algorithm-keyword">else</span> {
        <span class="algorithm-comment">// 이전 구간의 끝을 더 멀리 확장</span>
    }
}
<span class="algorithm-keyword">return</span> merged;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> Interval {
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">long</span> start, end;

    Interval(<span class="algorithm-keyword">long</span> start, <span class="algorithm-keyword">long</span> end) {
        <span class="algorithm-keyword">this</span>.start = start;
        <span class="algorithm-keyword">this</span>.end = end;
    }
}

<span class="algorithm-keyword">static</span> List&lt;Interval&gt; mergeIntervals(Interval[] input) {
    Interval[] intervals = input.clone();
    Arrays.sort(intervals, Comparator.comparingLong(interval -&gt; interval.start));
    List&lt;Interval&gt; merged = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;();

    <span class="algorithm-keyword">for</span> (Interval interval : intervals) {
        <span class="algorithm-keyword">if</span> (merged.isEmpty() || merged.get(merged.size() - 1).end &lt; interval.start) {
            merged.add(interval);
        } <span class="algorithm-keyword">else</span> {
            Interval last = merged.get(merged.size() - 1);
            merged.set(merged.size() - 1,
                    <span class="algorithm-keyword">new</span> Interval(last.start, Math.max(last.end, interval.end)));
        }
    }
    <span class="algorithm-keyword">return</span> merged;
}</code></pre>

[[Interval Scheduling]]
