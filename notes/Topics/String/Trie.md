---
type: data-structure
tags:
  - string
  - trie
language: java
status: in-review
source: []
---

# Trie (트라이)

> **한 줄 요약:** 문자열 집합을 저장·검색하는 트리 (접두사 공통 부분 공유)

## Core Concept

- **When to use:** 접두사 검색, 자동완성, 문자열 집합에서 존재 여부/개수
- **Key Point:**
  - 각 노드는 자식 포인터(또는 배열)를 가짐 — 보통 알파벳 26개, 숫자 10개 등
  - 루트에서 한 글자씩 내려가며 삽입/검색
  - 끝 노드에 표시(단어 끝) 또는 개수 저장
  - B형에서는 노드 수가 매우 커질 수 있어, `new Trie()`를 매번 만드는 방식보다 배열 풀(pool) 방식이 더 빠를 때가 많음

## API & State Change

| Operation | Code | 설명 |
| :-------- | :--- | :--- |
| Insert | `insert(s)` | 문자열 s 추가 |
| Search | `search(s)` | s가 존재하는지 (또는 접두사 개수) |
| Child | `node.next[c - 'a']` | 다음 글자로 이동 |
| Prefix count | `node.pass` | 해당 prefix를 지나는 문자열 개수 |

## Patterns

### Pattern A: 알파벳 소문자 Trie (존재 여부)

```java
class Trie {
    Trie[] next = new Trie[26];
    boolean end;

    void insert(String s) {
        Trie cur = this;
        for (int k = 0; k < s.length(); k++) {
            int i = s.charAt(k) - 'a'; // toCharArray 임시 배열 생성을 피함
            if (cur.next[i] == null) cur.next[i] = new Trie(); // 필요할 때만 노드 생성
            cur = cur.next[i];
        }
        cur.end = true;
    }

    boolean search(String s) {
        Trie cur = this;
        for (int k = 0; k < s.length(); k++) {
            int i = s.charAt(k) - 'a';
            if (cur.next[i] == null) return false;
            cur = cur.next[i];
        }
        return cur.end;
    }

    boolean startsWith(String p) {
        Trie cur = this;
        for (int k = 0; k < p.length(); k++) {
            int i = p.charAt(k) - 'a';
            if (cur.next[i] == null) return false;
            cur = cur.next[i];
        }
        return true;
    }
}
```

### Pattern B: 접두사 개수 카운트

- insert 시 경로의 각 노드에 cnt++

```java
class Trie {
    Trie[] next = new Trie[26];
    int pass;      // 이 노드를 지나는 문자열 수
    int endCount;  // 이 노드에서 끝나는 문자열 수

    void insert(String s) {
        Trie cur = this;
        cur.pass++;
        for (int k = 0; k < s.length(); k++) {
            int i = s.charAt(k) - 'a';
            if (cur.next[i] == null) cur.next[i] = new Trie();
            cur = cur.next[i];
            cur.pass++;
        }
        cur.endCount++;
    }

    int prefixCount(String p) {
        Trie cur = this;
        for (int k = 0; k < p.length(); k++) {
            int i = p.charAt(k) - 'a';
            if (cur.next[i] == null) return 0;
            cur = cur.next[i];
        }
        return cur.pass;
    }
}
```

### Pattern C: 배열 풀 Trie (객체 생성 최소화)
- 문자 집합이 작고 최대 노드 수를 추정할 수 있을 때 매우 빠름
```java
class TriePool {
    static final int ALPHA = 26;
    int[][] nxt;      // nxt[node][c] = 다음 노드 번호 (0이면 없음)
    int[] endCount;
    int nodes = 1;    // 0 = root

    TriePool(int maxNodes) {
        nxt = new int[maxNodes][ALPHA];
        endCount = new int[maxNodes];
    }

    void insert(String s) {
        int cur = 0;
        for (int k = 0; k < s.length(); k++) {
            int c = s.charAt(k) - 'a';
            if (nxt[cur][c] == 0) nxt[cur][c] = nodes++;
            cur = nxt[cur][c];
        }
        endCount[cur]++;
    }
}
```

## Pitfalls & Tips

- 문자 집합 크기에 따라 next를 Map<Character, Trie>로 두면 메모리 절약 가능
- 루트는 빈 문자열; 검색 시 루트에서 시작
- 단어 끝만 체크할지, 접두사도 허용할지(prefix) 구분
- 문자열 입력이 매우 많으면 `toCharArray()` 반복 생성도 비용이므로 `charAt(i)` 루프가 더 나을 수 있음
