---
tags:
  - strings
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# KMP

![KMP memory cue](../attachments/reference-kmp.svg)

> [!reference] 개념과 사용 조건
> **목적** · 문자열에서 패턴의 모든 시작 위치 찾기. 겹치는 일치도 포함한다.
>
> **pi[i]** · 0..i의 접두사이면서 접미사인 가장 긴 길이. 문자열 전체 길이는 제외한다.
>
> **반환·비용** · 0-based 위치 목록. 빈 패턴은 모든 경계 0..n. O(n + m).

---

## Pattern Search

**Structure**

<pre class="algorithm-code" data-language="text"><code>prefixFunction(pattern) {
    <span class="algorithm-keyword">for</span> (두 번째 문자부터) {
        <span class="algorithm-keyword">while</span> (불일치하고 일치 길이가 남아 있으면) {
            <span class="algorithm-comment">// 더 짧은 접두사·접미사로 이동</span>
        }
        <span class="algorithm-keyword">if</span> (일치하면) { <span class="algorithm-comment">/* 일치 길이 +1 */</span> }
        <span class="algorithm-comment">// pi에 저장</span>
    }
    <span class="algorithm-keyword">return</span> pi;
}

findAll(text, pattern) {
    <span class="algorithm-comment">// pi 생성</span>
    <span class="algorithm-keyword">for</span> (본문의 각 문자) {
        <span class="algorithm-keyword">while</span> (불일치하면) { <span class="algorithm-comment">/* pi를 따라 후퇴 */</span> }
        <span class="algorithm-keyword">if</span> (일치하면) { <span class="algorithm-comment">/* 일치 길이 +1 */</span> }
        <span class="algorithm-keyword">if</span> (패턴 전체가 일치하면) {
            <span class="algorithm-comment">// 시작 위치 기록 → 겹칠 수 있는 길이로 후퇴</span>
        }
    }
    <span class="algorithm-keyword">return</span> 위치 목록;
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span>[] prefixFunction(String s) {
    <span class="algorithm-keyword">int</span>[] pi = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[s.length()];
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = 1, j = 0; i &lt; s.length(); i++) {
        <span class="algorithm-keyword">while</span> (j &gt; 0 &amp;&amp; s.charAt(i) != s.charAt(j)) {
            j = pi[j - 1];    <span class="algorithm-comment">// 이미 맞은 부분의 접두사·접미사만 후보</span>
        }
        <span class="algorithm-keyword">if</span> (s.charAt(i) == s.charAt(j)) j++;
        pi[i] = j;
    }
    <span class="algorithm-keyword">return</span> pi;
}

<span class="algorithm-keyword">static</span> List&lt;Integer&gt; findAll(String text, String pattern) {
    List&lt;Integer&gt; matches = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;();
    <span class="algorithm-keyword">int</span> n = text.length(), m = pattern.length();
    <span class="algorithm-keyword">if</span> (m == 0) {
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = 0; i &lt;= n; i++) matches.add(i);
        <span class="algorithm-keyword">return</span> matches;
    }

    <span class="algorithm-keyword">int</span>[] pi = prefixFunction(pattern);
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = 0, j = 0; i &lt; n; i++) {
        <span class="algorithm-keyword">while</span> (j &gt; 0 &amp;&amp; text.charAt(i) != pattern.charAt(j)) {
            j = pi[j - 1];
        }
        <span class="algorithm-keyword">if</span> (text.charAt(i) == pattern.charAt(j)) j++;
        <span class="algorithm-keyword">if</span> (j == m) {
            matches.add(i - m + 1);
            j = pi[j - 1];    <span class="algorithm-comment">// 겹친 일치의 시작 부분을 버리지 않음</span>
        }
    }
    <span class="algorithm-keyword">return</span> matches;
}</code></pre>

---

## Borders and Repetition

Border는 문자열 전체를 제외한 접두사·접미사. repetitionLength는 문자열을 정확히 반복해서 만드는 최소 단위 길이.

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-comment">// pi를 생성</span>

borders(s) {
    <span class="algorithm-keyword">for</span> (가장 긴 접두사·접미사부터 pi를 따라) {
        <span class="algorithm-comment">// 길이 기록</span>
    }
    <span class="algorithm-keyword">return</span> 작은 길이부터 정렬한 목록;
}

repetitionLength(s) {
    <span class="algorithm-comment">// 후보 길이 = 전체 길이 − 마지막 pi</span>
    <span class="algorithm-keyword">if</span> (전체 길이가 후보 길이로 나누어떨어지면) <span class="algorithm-keyword">return</span> 후보 길이;
    <span class="algorithm-keyword">return</span> 전체 길이;
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span>[] prefixFunction(String s) {
    <span class="algorithm-keyword">int</span>[] pi = <span class="algorithm-keyword">new</span> <span class="algorithm-keyword">int</span>[s.length()];
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = 1, j = 0; i &lt; s.length(); i++) {
        <span class="algorithm-keyword">while</span> (j &gt; 0 &amp;&amp; s.charAt(i) != s.charAt(j)) {
            j = pi[j - 1];
        }
        <span class="algorithm-keyword">if</span> (s.charAt(i) == s.charAt(j)) j++;
        pi[i] = j;
    }
    <span class="algorithm-keyword">return</span> pi;
}

<span class="algorithm-keyword">static</span> List&lt;Integer&gt; borders(String s) {
    List&lt;Integer&gt; result = <span class="algorithm-keyword">new</span> ArrayList&lt;&gt;();
    <span class="algorithm-keyword">if</span> (s.isEmpty()) <span class="algorithm-keyword">return</span> result;

    <span class="algorithm-keyword">int</span>[] pi = prefixFunction(s);
    <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> length = pi[s.length() - 1]; length &gt; 0; length = pi[length - 1]) {
        result.add(length);
    }
    Collections.reverse(result);
    <span class="algorithm-keyword">return</span> result;
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">int</span> repetitionLength(String s) {
    <span class="algorithm-keyword">int</span> n = s.length();
    <span class="algorithm-keyword">if</span> (n == 0) <span class="algorithm-keyword">return</span> 0;

    <span class="algorithm-keyword">int</span>[] pi = prefixFunction(s);
    <span class="algorithm-keyword">int</span> length = n - pi[n - 1];
    <span class="algorithm-keyword">return</span> n % length == 0 ? length : n;    <span class="algorithm-comment">// 끝에 남는 조각 없이 반복되어야 함</span>
}</code></pre>
