---
type: template
tags: ["Java"]
aliases: ["Hash"]
---

# Hashing

```java
// names는 String 배열. 횟수가 필요하면 Map, 존재만 필요하면 Set.
Map<String, Integer> count = new HashMap<>();
for (String name : names) {
    // 처음 나온 이름도 0회에서 시작해 같은 식으로 센다.
    count.put(name, count.getOrDefault(name, 0) + 1);
}

Set<String> unique = new HashSet<>();
for (String name : names) {
    unique.add(name); // 같은 값은 다시 넣지 않는다. 이미 있으면 false를 반환한다.
}
```

**예시**

```text
입력     민  준  민
Map      {민:2, 준:1}   ← 몇 번?
Set      {민, 준}       ← 있었나?
```

**주의** 순서를 보장하지 않는다. 조회·삽입 평균 O(1).

[[Subarray Sum - Existence]] · [[Subarray Sum - Count]] · [[Java Basics]]
