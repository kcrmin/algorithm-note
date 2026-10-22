---
type: template
tags: ["Java"]
aliases: ["Hash"]
category: core-collections
categories: [core-collections]
---

# Hashing

```text
names     ["A", "B", "A"]
            ↓          ↓
Map       { A:2, B:1 }      값 → 횟수
Set       { A, B }          존재 여부만
```

## Steps

| Step | Pseudocode |
| --- | --- |
| **Count** | // 이름을 key로 찾는다. 없으면 0회부터 시작해 한 번 증가시킨다. |
| **Unique** | // 이름을 집합에 넣는다. 같은 값은 한 번만 남는다. |

## Java

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

`names`는 String[] · `java.util.*` · 평균 조회·삽입 `O(1)`

## Watch points

| 선택 | 이유 / 실수 |
| --- | --- |
| Map / Set 선택 | 값별 횟수나 정보가 필요하면 Map, 존재 여부만 필요하면 Set. |
| `getOrDefault(key, 0)` | 처음 등장한 key는 get이 null을 반환한다. 기본값으로 0을 준 뒤 더한다. |
| 포함 확인 | Map은 `containsKey`, Set은 `contains`. 이름이 다르다. |
| 순서 | HashMap·HashSet의 순회 순서는 정렬 순서가 아니다. |
| 사용자 정의 key | 내용으로 같은 key를 취급하려면 equals와 hashCode를 함께 구현한다. |

---

**분류** [[00 Maps/Core Collections|Core Collections]]

**비교** [[90 Data Structures/Frequency Array and Buckets|Frequency Array and Buckets]]
