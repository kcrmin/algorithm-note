---
tags:
  - data-structures
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Heap with Lazy Deletion

![Heap with Lazy Deletion memory cue](../attachments/reference-heap-with-lazy-deletion.svg)

> [!reference] 개념과 사용 조건
> **목적** · ID별 우선순위 갱신·삭제를 지원하는 최소 힙.
>
> **핵심** · Map은 현재 후보, heap에는 옛 후보도 남는다. 객체를 바꾸고 기존 객체는 수정하지 않는다.
>
> **비용** · 삽입·갱신 O(log q), 삭제 Map 평균 O(1). q번 삽입 동안 정리 비용 합계 O(q log q), 공간 O(q).

---

## Template

ID별 항목을 추가·갱신·삭제하고, 현재 우선순위가 가장 작은 항목을 조회하거나 꺼낸다.

**Structure**

<pre class="algorithm-code" data-language="text"><code>put(id, priority) {
    <span class="algorithm-comment">// 새 Entry를 현재 기록과 힙에 저장</span>
}
remove(id) {
    <span class="algorithm-comment">// 현재 기록만 삭제</span>
}
clean() {
    <span class="algorithm-keyword">while</span> (top이 현재 기록의 객체가 아니면) {
        <span class="algorithm-comment">// 옛 후보 제거</span>
    }
}
poll() {
    <span class="algorithm-comment">// clean → 최소 후보 제거 → 현재 기록 삭제</span>
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> Entry {
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span> id;
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">long</span> priority;

    Entry(<span class="algorithm-keyword">int</span> id, <span class="algorithm-keyword">long</span> priority) {
        <span class="algorithm-keyword">this</span>.id = id;
        <span class="algorithm-keyword">this</span>.priority = priority;
    }
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> LazyMinHeap {
    <span class="algorithm-keyword">final</span> Map&lt;Integer, Entry&gt; current = <span class="algorithm-keyword">new</span> HashMap&lt;&gt;();
    <span class="algorithm-keyword">final</span> PriorityQueue&lt;Entry&gt; heap = <span class="algorithm-keyword">new</span> PriorityQueue&lt;&gt;(
            Comparator.comparingLong(entry -&gt; entry.priority));

    <span class="algorithm-keyword">void</span> put(<span class="algorithm-keyword">int</span> id, <span class="algorithm-keyword">long</span> priority) {
        Entry entry = <span class="algorithm-keyword">new</span> Entry(id, priority);
        current.put(id, entry);
        heap.offer(entry);
    }

    <span class="algorithm-keyword">void</span> remove(<span class="algorithm-keyword">int</span> id) {
        current.remove(id);
    }

    <span class="algorithm-keyword">void</span> clean() {
        <span class="algorithm-keyword">while</span> (!heap.isEmpty() &amp;&amp; current.get(heap.peek().id) != heap.peek()) {
            heap.poll();    <span class="algorithm-comment">// 같은 ID·우선순위로 재삽입돼도 객체가 다르면 옛 후보</span>
        }
    }

    Entry peek() {
        clean();
        <span class="algorithm-keyword">return</span> heap.peek();    <span class="algorithm-comment">// 비었으면 null</span>
    }

    Entry poll() {
        clean();
        Entry entry = heap.poll();
        <span class="algorithm-keyword">if</span> (entry != <span class="algorithm-keyword">null</span>) current.remove(entry.id);
        <span class="algorithm-keyword">return</span> entry;
    }
}</code></pre>

[[PriorityQueue]] · [[Dijkstra]]
