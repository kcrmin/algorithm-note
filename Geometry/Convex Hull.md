---
tags:
  - geometry
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Convex Hull

![Convex Hull memory cue](../attachments/reference-convex-hull.svg)

> [!reference] 개념과 사용 조건
> **목적** · 모든 점을 포함하는 가장 작은 볼록 다각형의 꼭짓점.
>
> **조건** · 정수 좌표, 각 좌표의 절댓값 ≤ 10⁹. 중복 점은 제거한다.
>
> **반환** · 반시계 순서, 시작점 중복 없음. 변 위의 중간 점은 제외하며 전부 일직선이면 양끝만 반환.
>
> **비용** · O(n log n), 공간 O(n). 빈 입력·한 점도 처리한다.

---

## Monotone Chain

모든 점을 둘러싸는 가장 작은 볼록 다각형의 꼭짓점을 반시계 순서로 구한다.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// x, y 순서 정렬 → 중복 제거</span>
<span class="algorithm-keyword">for</span> (왼쪽부터 각 점) {
    <span class="algorithm-keyword">while</span> (마지막 두 점과 새 점이 좌회전하지 않으면) { <span class="algorithm-comment">/* 마지막 점 제거 */</span> }
    <span class="algorithm-comment">// 아래쪽 껍질에 새 점 추가</span>
}

<span class="algorithm-keyword">for</span> (오른쪽부터 각 점) {
    <span class="algorithm-keyword">while</span> (위쪽 껍질이 좌회전하지 않으면) { <span class="algorithm-comment">/* 마지막 점 제거 */</span> }
    <span class="algorithm-comment">// 위쪽 껍질에 새 점 추가</span>
}
<span class="algorithm-keyword">return</span> 중복된 시작점을 뺀 껍질;</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> Point {
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">long</span> x, y;

    Point(<span class="algorithm-keyword">long</span> x, <span class="algorithm-keyword">long</span> y) {
        <span class="algorithm-keyword">this</span>.x = x;
        <span class="algorithm-keyword">this</span>.y = y;
    }
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">long</span> cross(Point a, Point b, Point c) {
    <span class="algorithm-keyword">return</span> (b.x - a.x) * (c.y - a.y) - (b.y - a.y) * (c.x - a.x);
}

<span class="algorithm-keyword">static</span> Point[] convexHull(Point[] input) {
    Point[] sorted = input.clone();
    Arrays.sort(sorted, Comparator.comparingLong((Point p) -&gt; p.x)
            .thenComparingLong(p -&gt; p.y));

    List&lt;Point&gt; points = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;();
    <span class="algorithm-keyword">for</span> (Point point : sorted) {
        <span class="algorithm-keyword">if</span> (points.isEmpty() || point.x != points.get(points.size() - 1).x
                || point.y != points.get(points.size() - 1).y) {
            points.add(point);
        }
    }
    <span class="algorithm-keyword">int</span> n = points.size();
    <span class="algorithm-keyword">if</span> (n &lt;= 1) <span class="algorithm-keyword">return</span> points.toArray(<span class="algorithm-keyword">new</span> Point[0]);

    Point[] hull = <span class="algorithm-keyword">new</span> Point[2 * n];
    <span class="algorithm-keyword">int</span> size = 0;
    <span class="algorithm-keyword">for</span> (Point point : points) {
        <span class="algorithm-keyword">while</span> (size &gt;= 2 &amp;&amp; cross(hull[size - 2], hull[size - 1], point) &lt;= 0) {
            size--;    <span class="algorithm-comment">// 일직선의 중간 점까지 제거하려고 &lt;= 사용</span>
        }
        hull[size++] = point;
    }

    <span class="algorithm-keyword">int</span> lowerSize = size;
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = n - 2; i &gt;= 0; i--) {
        Point point = points.get(i);
        <span class="algorithm-keyword">while</span> (size &gt; lowerSize &amp;&amp; cross(hull[size - 2], hull[size - 1], point) &lt;= 0) {
            size--;    <span class="algorithm-comment">// 아래쪽 껍질을 지우지 않고 위쪽만 검사</span>
        }
        hull[size++] = point;
    }
    <span class="algorithm-keyword">return</span> Arrays.copyOf(hull, size - 1);    <span class="algorithm-comment">// 마지막에 다시 넣은 시작점 제외</span>
}</code></pre>

[[CCW and Segment Intersection]]
