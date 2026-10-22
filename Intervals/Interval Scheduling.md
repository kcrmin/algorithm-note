---
tags:
  - intervals
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Interval Scheduling

![Interval Scheduling memory cue](../attachments/reference-interval-scheduling.svg)

> [!reference] 개념과 사용 조건
> **목적** · 겹치지 않는 구간을 최대 개수 선택.
>
> **조건** · start ≤ end. 끝나는 시각에 다음 구간 시작 가능. 구간별 가치가 다르면 이 규칙으로 최대 가치를 구할 수 없다.
>
> **비용** · 시간 O(n log n), 입력 복사·결과 공간 O(n).

---

## Template

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// 종료 시각 오름차순 정렬</span>
<span class="algorithm-keyword">for</span> (각 구간) {
    <span class="algorithm-keyword">if</span> (start &gt;= 이전 선택의 end) {
        <span class="algorithm-comment">// 선택하고 end 갱신</span>
    }
}
<span class="algorithm-keyword">return</span> selected;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> Interval {
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">long</span> start, end;

    Interval(<span class="algorithm-keyword">long</span> start, <span class="algorithm-keyword">long</span> end) {
        <span class="algorithm-keyword">this</span>.start = start;
        <span class="algorithm-keyword">this</span>.end = end;
    }
}

<span class="algorithm-keyword">static</span> List&lt;Interval&gt; selectIntervals(Interval[] input) {
    Interval[] intervals = input.clone();
    Arrays.sort(intervals, Comparator.comparingLong((Interval interval) -&gt; interval.end)
            .thenComparingLong(interval -&gt; interval.start));    <span class="algorithm-comment">// 종료 동률이면 길이 0인 구간을 나중에 선택</span>
    List&lt;Interval&gt; selected = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;();
    <span class="algorithm-keyword">long</span> end = Long.MIN_VALUE;

    <span class="algorithm-keyword">for</span> (Interval interval : intervals) {
        <span class="algorithm-keyword">if</span> (interval.start &gt;= end) {
            selected.add(interval);
            end = interval.end;    <span class="algorithm-comment">// 빨리 끝낼수록 이후 선택지를 더 많이 남김</span>
        }
    }
    <span class="algorithm-keyword">return</span> selected;
}</code></pre>

[[Sorting and Comparator]] · [[Sweep Line]]
