---
tags:
  - java
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# PriorityQueue

![PriorityQueue memory cue](../attachments/reference-priorityqueue.svg)

> [!reference] 개념과 사용 조건
> **목적** · 매번 가장 작은 값 또는 가장 큰 값을 꺼낸다.
>
> **조건** · 기본은 최소 힙. 넣은 객체의 비교 필드는 바꾸지 않는다. 순회 결과는 정렬 순서가 아니다.
>
> **비용** · offer/poll O(log n), peek O(1). 특정 원소 remove는 O(n). 빈 힙의 poll/peek는 null.

---

## Numbers

최소 힙에서 최솟값을 꺼내고, 최대 힙에서 최댓값을 확인한다.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// 최소 힙 또는 최대 힙 선택</span>
<span class="algorithm-comment">// offer → peek / poll</span></code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code>PriorityQueue&lt;Integer&gt; minHeap = <span class="algorithm-keyword">new</span> PriorityQueue&lt;&gt;();
PriorityQueue&lt;Integer&gt; maxHeap = <span class="algorithm-keyword">new</span> PriorityQueue&lt;&gt;(Comparator.reverseOrder());
minHeap.offer(3);
minHeap.offer(1);
Integer smallest = minHeap.poll();
maxHeap.offer(3);
maxHeap.offer(1);
Integer largest = maxHeap.peek();</code></pre>

---

## Objects

priority가 작은 작업부터, 같으면 id가 작은 작업부터 꺼내는 큐를 만든다.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// priority가 작을수록 먼저</span>
<span class="algorithm-comment">// 같은 priority면 id가 작을수록 먼저</span></code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> Task {
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span> id;
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">long</span> priority;

    Task(<span class="algorithm-keyword">int</span> id, <span class="algorithm-keyword">long</span> priority) {
        <span class="algorithm-keyword">this</span>.id = id;
        <span class="algorithm-keyword">this</span>.priority = priority;
    }
}

<span class="algorithm-keyword">static</span> PriorityQueue&lt;Task&gt; createTaskQueue() {
    <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">new</span> PriorityQueue&lt;&gt;(Comparator.comparingLong((Task task) -&gt; task.priority)
            .thenComparingInt(task -&gt; task.id));
}</code></pre>

[[Dijkstra]] · [[Heap with Lazy Deletion]]
