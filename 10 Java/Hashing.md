---
type: template
tags: ["Java"]
aliases: ["Hash"]
---

# Hashing

**언제** 값별 개수는 HashMap, 중복 없이 존재만 저장하면 HashSet.

```text
입력     민  준  민
Map      {민:2, 준:1}   ← 몇 번?
Set      {민, 준}       ← 있었나?
```

**코드** `names`는 String 배열. 컬렉션은 `java.util.*`.

```java
Map<String, Integer> count = new HashMap<>();
for (String name : names) {
    count.put(name, count.getOrDefault(name, 0) + 1);
}

Set<String> unique = new HashSet<>();
for (String name : names) {
    unique.add(name);
}
```

| 암기 포인트 | 이유 |
|---|---|
| `getOrDefault(name, 0) + 1` | 처음 나온 값도 0회에서 시작해 같은 식으로 센다. |
| `unique.add(name)` | 이미 있으면 false, 새 값이면 true다. |

**주의** 순서를 보장하지 않는다. 조회·삽입 평균 O(1).

[[Subarray Sum - Existence]] · [[Subarray Sum - Count]] · [[Java Basics]]
