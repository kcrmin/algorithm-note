---
tags:
  - intervals
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Sweep Line

![Sweep Line memory cue](../attachments/reference-sweep-line.svg)

> [!reference] 개념과 사용 조건
> **목적** · 동시에 활성인 구간의 최대 개수.
>
> **조건** · 반열린 구간 `[start, end)`, start < end. 종료 시각과 다음 시작 시각은 겹치지 않는다.
>
> **비용** · 이벤트 2n개 정렬. 시간 O(n log n), 공간 O(n).

---

## Maximum Overlap

반열린 구간 [start, end)가 동시에 가장 많이 겹치는 순간의 구간 개수를 구한다.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// 시작·종료 이벤트 생성 → 시각순 정렬</span>
<span class="algorithm-keyword">while</span> (이벤트가 남았으면) {
    <span class="algorithm-comment">// 같은 시각의 변화 전부 적용</span>
    <span class="algorithm-comment">// answer: 활성 개수 최댓값</span>
}
<span class="algorithm-keyword">return</span> answer;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> Event {
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">long</span> time;
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span> delta;

    Event(<span class="algorithm-keyword">long</span> time, <span class="algorithm-keyword">int</span> delta) {
        <span class="algorithm-keyword">this</span>.time = time;
        <span class="algorithm-keyword">this</span>.delta = delta;
    }
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span> maxOverlap(<span class="algorithm-keyword">long</span>[][] intervals) {
    List&lt;Event&gt; events = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;();
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">long</span>[] interval : intervals) {
        events.add(<span class="algorithm-keyword">new</span> Event(interval[0], 1));
        events.add(<span class="algorithm-keyword">new</span> Event(interval[1], -1));
    }
    events.sort(Comparator.comparingLong(event -&gt; event.time));
    <span class="algorithm-keyword">int</span> i = 0, active = 0, answer = 0;

    <span class="algorithm-keyword">while</span> (i &lt; events.size()) {
        <span class="algorithm-keyword">long</span> time = events.get(i).time;
        <span class="algorithm-keyword">while</span> (i &lt; events.size() &amp;&amp; events.get(i).time == time) {
            active += events.get(i++).delta;
        }
        answer = Math.max(answer, active);    <span class="algorithm-comment">// 같은 시각의 종료·시작을 중간에 세지 않음</span>
    }
    <span class="algorithm-keyword">return</span> answer;
}</code></pre>

[[Difference Array]] · [[Interval Scheduling]]
