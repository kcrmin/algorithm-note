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
    count.put(name, count.getOrDefault(name, 0) + 1); // count: 값별 횟수; 처음은 0부터
}
Set<String> unique = new HashSet<>();
for (String name : names) {
    unique.add(name); // unique: 같은 값은 한 번만 저장
}
```

**조건** names는 String[]. 평균 조회·삽입 O(1).

**분류** [[00 Maps/Core Collections|Core Collections]]

**비교** [[90 Data Structures/Frequency Array and Buckets|Frequency Array and Buckets]]
