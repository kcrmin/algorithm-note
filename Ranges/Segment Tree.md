---
tags:
  - ranges
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Segment Tree

![Segment Tree memory cue](../attachments/reference-segment-tree.svg)

> [!reference] 개념과 사용 조건
> **목적** · 점 갱신과 구간의 합·최솟값·최댓값.
>
> **범위** · `set`의 index는 `0..n-1`. 질의는 `0 ≤ left ≤ right ≤ n`인 `[left, right)`.
>
> **변형** · sum의 항등원은 0, min은 +∞, max는 −∞. 패딩 leaf와 빈 질의에도 같은 값 사용.
>
> **비용** · 생성 O(n), 갱신·질의 O(log n), 공간 O(n). 합은 long 범위.

---

## Range Sum

원소의 값을 바꾸면서 `[left, right)` 구간의 합을 구한다.

**Structure**

<pre class="algorithm-code" data-language="text"><code>set(index, value) {
    <span class="algorithm-comment">// leaf 교체</span>
    <span class="algorithm-keyword">while</span> (부모가 있으면) {
        <span class="algorithm-comment">// 두 자식으로 부모 재계산</span>
    }
}

query(left, right) {
    <span class="algorithm-keyword">while</span> (아직 구간이 남으면) {
        <span class="algorithm-comment">// 왼쪽·오른쪽의 단독 블록을 답에 결합</span>
        <span class="algorithm-comment">// 양쪽을 부모 레벨로 이동</span>
    }
    <span class="algorithm-keyword">return</span> answer;
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> SegmentTree {
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span> size;
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">long</span>[] tree;

    SegmentTree(<span class="algorithm-keyword">long</span>[] values) {
        <span class="algorithm-keyword">int</span> capacity = 1;
        <span class="algorithm-keyword">while</span> (capacity &lt; values.length) {
            capacity &lt;&lt;= 1;
        }
        size = capacity;
        tree = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[size * 2];
        System.arraycopy(values, 0, tree, size, values.length);

        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = size - 1; i &gt; 0; i--) {
            tree[i] = tree[i * 2] + tree[i * 2 + 1];
        }
    }

    <span class="algorithm-keyword">void</span> set(<span class="algorithm-keyword">int</span> index, <span class="algorithm-keyword">long</span> value) {
        <span class="algorithm-keyword">int</span> node = size + index;
        tree[node] = value;

        <span class="algorithm-keyword">while</span> (node &gt; 1) {
            node /= 2;
            tree[node] = tree[node * 2] + tree[node * 2 + 1];
        }
    }

    <span class="algorithm-keyword">long</span> sum(<span class="algorithm-keyword">int</span> left, <span class="algorithm-keyword">int</span> right) {
        <span class="algorithm-keyword">long</span> answer = 0;
        left += size;
        right += size;

        <span class="algorithm-keyword">while</span> (left &lt; right) {
            <span class="algorithm-keyword">if</span> ((left &amp; 1) == 1) {
                answer += tree[left++];    <span class="algorithm-comment">// 오른쪽 자식이면 왼쪽 형제는 범위 밖</span>
            }
            <span class="algorithm-keyword">if</span> ((right &amp; 1) == 1) {
                answer += tree[--right];    <span class="algorithm-comment">// right는 제외 경계라 먼저 감소</span>
            }
            left /= 2;
            right /= 2;
        }

        <span class="algorithm-keyword">return</span> answer;
    }
}</code></pre>

---

## Range Minimum

원소의 값을 바꾸면서 `[left, right)` 구간의 최솟값을 구한다.

빈 구간의 반환값은 `Long.MAX_VALUE`.

**Structure**

<pre class="algorithm-code" data-language="text"><code>set(index, value) {
    <span class="algorithm-comment">// leaf 교체</span>
    <span class="algorithm-keyword">while</span> (부모가 있으면) {
        <span class="algorithm-comment">// 두 자식으로 부모 재계산</span>
    }
}

query(left, right) {
    <span class="algorithm-keyword">while</span> (아직 구간이 남으면) {
        <span class="algorithm-comment">// 왼쪽·오른쪽의 단독 블록을 답에 결합</span>
        <span class="algorithm-comment">// 양쪽을 부모 레벨로 이동</span>
    }
    <span class="algorithm-keyword">return</span> answer;
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">import</span> java.util.*;

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> MinSegmentTree {
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span> size;
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">long</span>[] tree;

    MinSegmentTree(<span class="algorithm-keyword">long</span>[] values) {
        <span class="algorithm-keyword">int</span> capacity = 1;
        <span class="algorithm-keyword">while</span> (capacity &lt; values.length) {
            capacity &lt;&lt;= 1;
        }
        size = capacity;
        tree = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[size * 2];
        Arrays.fill(tree, <mark class="algorithm-change">Long.MAX_VALUE</mark>);    <span class="algorithm-comment">// 패딩 leaf도 결합 결과에 영향을 주지 않아야 함</span>
        System.arraycopy(values, 0, tree, size, values.length);

        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = size - 1; i &gt; 0; i--) {
            tree[i] = <mark class="algorithm-change">Math.min</mark>(tree[i * 2], tree[i * 2 + 1]);
        }
    }

    <span class="algorithm-keyword">void</span> set(<span class="algorithm-keyword">int</span> index, <span class="algorithm-keyword">long</span> value) {
        <span class="algorithm-keyword">int</span> node = size + index;
        tree[node] = value;

        <span class="algorithm-keyword">while</span> (node &gt; 1) {
            node /= 2;
            tree[node] = <mark class="algorithm-change">Math.min</mark>(tree[node * 2], tree[node * 2 + 1]);
        }
    }

    <span class="algorithm-keyword">long</span> min(<span class="algorithm-keyword">int</span> left, <span class="algorithm-keyword">int</span> right) {
        <span class="algorithm-keyword">long</span> answer = <mark class="algorithm-change">Long.MAX_VALUE</mark>;
        left += size;
        right += size;

        <span class="algorithm-keyword">while</span> (left &lt; right) {
            <span class="algorithm-keyword">if</span> ((left &amp; 1) == 1) {
                answer = <mark class="algorithm-change">Math.min</mark>(answer, tree[left++]);    <span class="algorithm-comment">// 오른쪽 자식이면 왼쪽 형제는 범위 밖</span>
            }
            <span class="algorithm-keyword">if</span> ((right &amp; 1) == 1) {
                answer = <mark class="algorithm-change">Math.min</mark>(answer, tree[--right]);    <span class="algorithm-comment">// right는 제외 경계라 먼저 감소</span>
            }
            left /= 2;
            right /= 2;
        }

        <span class="algorithm-keyword">return</span> answer;
    }
}</code></pre>

---

## Range Maximum

원소의 값을 바꾸면서 `[left, right)` 구간의 최댓값을 구한다.

빈 구간의 반환값은 `Long.MIN_VALUE`.

**Structure**

<pre class="algorithm-code" data-language="text"><code>set(index, value) {
    <span class="algorithm-comment">// leaf 교체</span>
    <span class="algorithm-keyword">while</span> (부모가 있으면) {
        <span class="algorithm-comment">// 두 자식으로 부모 재계산</span>
    }
}

query(left, right) {
    <span class="algorithm-keyword">while</span> (아직 구간이 남으면) {
        <span class="algorithm-comment">// 왼쪽·오른쪽의 단독 블록을 답에 결합</span>
        <span class="algorithm-comment">// 양쪽을 부모 레벨로 이동</span>
    }
    <span class="algorithm-keyword">return</span> answer;
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">import</span> java.util.*;

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> MaxSegmentTree {
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">int</span> size;
    <span class="algorithm-keyword">final</span> <span class="algorithm-keyword">long</span>[] tree;

    MaxSegmentTree(<span class="algorithm-keyword">long</span>[] values) {
        <span class="algorithm-keyword">int</span> capacity = 1;
        <span class="algorithm-keyword">while</span> (capacity &lt; values.length) {
            capacity &lt;&lt;= 1;
        }
        size = capacity;
        tree = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">long</span>[size * 2];
        Arrays.fill(tree, <mark class="algorithm-change">Long.MIN_VALUE</mark>);    <span class="algorithm-comment">// 패딩 leaf도 결합 결과에 영향을 주지 않아야 함</span>
        System.arraycopy(values, 0, tree, size, values.length);

        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = size - 1; i &gt; 0; i--) {
            tree[i] = <mark class="algorithm-change">Math.max</mark>(tree[i * 2], tree[i * 2 + 1]);
        }
    }

    <span class="algorithm-keyword">void</span> set(<span class="algorithm-keyword">int</span> index, <span class="algorithm-keyword">long</span> value) {
        <span class="algorithm-keyword">int</span> node = size + index;
        tree[node] = value;

        <span class="algorithm-keyword">while</span> (node &gt; 1) {
            node /= 2;
            tree[node] = <mark class="algorithm-change">Math.max</mark>(tree[node * 2], tree[node * 2 + 1]);
        }
    }

    <span class="algorithm-keyword">long</span> max(<span class="algorithm-keyword">int</span> left, <span class="algorithm-keyword">int</span> right) {
        <span class="algorithm-keyword">long</span> answer = <mark class="algorithm-change">Long.MIN_VALUE</mark>;
        left += size;
        right += size;

        <span class="algorithm-keyword">while</span> (left &lt; right) {
            <span class="algorithm-keyword">if</span> ((left &amp; 1) == 1) {
                answer = <mark class="algorithm-change">Math.max</mark>(answer, tree[left++]);    <span class="algorithm-comment">// 오른쪽 자식이면 왼쪽 형제는 범위 밖</span>
            }
            <span class="algorithm-keyword">if</span> ((right &amp; 1) == 1) {
                answer = <mark class="algorithm-change">Math.max</mark>(answer, tree[--right]);    <span class="algorithm-comment">// right는 제외 경계라 먼저 감소</span>
            }
            left /= 2;
            right /= 2;
        }

        <span class="algorithm-keyword">return</span> answer;
    }
}</code></pre>

[[Fenwick Tree]] · [[Lazy Segment Tree]]
