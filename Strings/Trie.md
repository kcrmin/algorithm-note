---
tags:
  - strings
cssclasses:
  - algorithm-reference-sample
code-styler-ignore: true
---

# Trie

![Trie memory cue](../attachments/reference-trie.svg)

> [!reference] 개념과 사용 조건
> **목적** · 문자열 저장·검색과 접두사 검사.
>
> **조건** · 영문 소문자 a–z. 빈 문자열도 저장할 수 있다.
>
> **비용** · 삽입·검색 O(문자열 길이). 노드마다 자식 26칸을 사용한다.

---

## Template

**Structure**

<pre class="algorithm-code" data-language="text"><code><span class="algorithm-keyword">class</span> Trie {
    insert(word) {
        <span class="algorithm-keyword">for</span> (각 문자) {
            <span class="algorithm-comment">// 자식이 없으면 생성 → 자식으로 이동</span>
        }
        <span class="algorithm-comment">// 마지막 노드에 단어 끝 표시</span>
    }

    walk(text) {
        <span class="algorithm-keyword">for</span> (각 문자) {
            <span class="algorithm-keyword">if</span> (자식이 없으면) <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">null</span>;
            <span class="algorithm-comment">// 자식으로 이동</span>
        }
        <span class="algorithm-keyword">return</span> 마지막 노드;
    }

    contains(word) { <span class="algorithm-comment">/* 도착 가능 + 단어 끝 */</span> }
    startsWith(prefix) { <span class="algorithm-comment">/* 도착 가능 */</span> }
}</code></pre>

**Java**

<pre class="algorithm-code" data-language="java"><code><span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> Node {
    <span class="algorithm-keyword">final</span> Node[] children = <span class="algorithm-keyword">new</span> Node[26];
    <span class="algorithm-keyword">boolean</span> end;
}

<span class="algorithm-keyword">static</span> <span class="algorithm-keyword">class</span> Trie {
    <span class="algorithm-keyword">final</span> Node root = <span class="algorithm-keyword">new</span> Node();

    <span class="algorithm-keyword">void</span> insert(String word) {
        Node node = root;
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = 0; i &lt; word.length(); i++) {
            <span class="algorithm-keyword">int</span> index = word.charAt(i) - 'a';
            <span class="algorithm-keyword">if</span> (node.children[index] == <span class="algorithm-keyword">null</span>) {
                node.children[index] = <span class="algorithm-keyword">new</span> Node();
            }
            node = node.children[index];
        }
        node.end = <span class="algorithm-keyword">true</span>;    <span class="algorithm-comment">// 접두사에 도착한 것과 단어 일치를 구별</span>
    }

    Node walk(String text) {
        Node node = root;
        <span class="algorithm-keyword">for</span> (<span class="algorithm-keyword">int</span> i = 0; i &lt; text.length(); i++) {
            node = node.children[text.charAt(i) - 'a'];
            <span class="algorithm-keyword">if</span> (node == <span class="algorithm-keyword">null</span>) <span class="algorithm-keyword">return</span> <span class="algorithm-keyword">null</span>;
        }
        <span class="algorithm-keyword">return</span> node;
    }

    <span class="algorithm-keyword">boolean</span> contains(String word) {
        Node node = walk(word);
        <span class="algorithm-keyword">return</span> node != <span class="algorithm-keyword">null</span> &amp;&amp; node.end;
    }

    <span class="algorithm-keyword">boolean</span> startsWith(String prefix) {
        <span class="algorithm-keyword">return</span> walk(prefix) != <span class="algorithm-keyword">null</span>;
    }
}</code></pre>
