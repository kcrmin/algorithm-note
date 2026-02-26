---
type: data-structure
tags:
  - trie
  - string
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

## API & State Change

| Operation | Code | 설명 |
| :-------- | :--- | :--- |
| Insert | `insert(s)` | 문자열 s 추가 |
| Search | `search(s)` | s가 존재하는지 (또는 접두사 개수) |
| Child | `node.next[c - 'a']` | 다음 글자로 이동 |

## Patterns

### Pattern A: 알파벳 소문자 Trie (존재 여부)

```java
class Trie {
    Trie[] next = new Trie[26];
    boolean end;

    void insert(String s) {
        Trie cur = this;
        for (char c : s.toCharArray()) {
            int i = c - 'a';
            if (cur.next[i] == null) cur.next[i] = new Trie();
            cur = cur.next[i];
        }
        cur.end = true;
    }

    boolean search(String s) {
        Trie cur = this;
        for (char c : s.toCharArray()) {
            int i = c - 'a';
            if (cur.next[i] == null) return false;
            cur = cur.next[i];
        }
        return cur.end;
    }

    boolean startsWith(String p) {
        Trie cur = this;
        for (char c : p.toCharArray()) {
            int i = c - 'a';
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
int[] cnt;  // 노드별로 카운트
void insert(String s) {
    Trie cur = this;
    for (char c : s.toCharArray()) {
        cur.cnt++;
        int i = c - 'a';
        if (cur.next[i] == null) cur.next[i] = new Trie();
        cur = cur.next[i];
    }
    cur.cnt++;
    cur.end = true;
}
```

## Pitfalls & Tips

- 문자 집합 크기에 따라 next를 Map<Character, Trie>로 두면 메모리 절약 가능
- 루트는 빈 문자열; 검색 시 루트에서 시작
- 단어 끝만 체크할지, 접두사도 허용할지(prefix) 구분
