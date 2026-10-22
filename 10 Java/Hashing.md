---
type: template
tags: ["Java"]
aliases: ["Hash"]
category: core-collections
categories: [core-collections]
---

# Hashing

```java
Map<String, Integer> count = new HashMap<>();
for (String name : names) {
    // 처음 나온 값도 0회부터 센다.
    count.put(name, count.getOrDefault(name, 0) + 1);
}
Set<String> unique = new HashSet<>();
for (String name : names) {
    unique.add(name); // 같은 값은 다시 들어가지 않는다.
}
```

**조건** names는 String[]. 평균 조회·삽입 O(1).

**분류** [[00 Maps/Core Collections|Core Collections]]

**비교** [[90 Data Structures/Frequency Array and Buckets|Frequency Array and Buckets]]
