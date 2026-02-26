---
deprecated: true
note: "Inbox 정리용 노트 (deprecated)"
---

# 섹션 7 - Recursive Tree Graph

## 1. Recursive 기초
```java
public class Main {
    void recursive(int n) {
        if (n == 0) return; // 종료 조건 (base case)
        recursive(n - 1); // 재귀 호출
    }
}
```
> Base Case: 종료 조건 필수
> 
> Stack 구조: 호출마다 스택 프레임 생성, 과도하면 StackOverflowError


## 2. Tree 기초
```java
public class Main {
    static class Node {
        int data;
        Node lt, rt;
        
        public Node(int data) {
            this.data = data;
            lt = null;
            rt = null;
        }
    }
    
    public static void DFS(Node node) {
        if (root == null) return;
        
        // 중위 순회
        DFS(root.lt);
        System.out.println(node.data + " ");
        DFS(root.rt);
    }
}
```
| 순회 방식         |     순서     | 코드 위치  |
|:--------------|:----------:|:-------|
| 전위(preorder)  | 부모 → 좌 → 우 | DFS 위  |
| 중위(inorder)   | 좌 → 부모 → 우 | DFS 사이 |
| 후위(postorder) | 좌 → 우 → 부모 | DFS 아래 |
> 전위: 구조 파악용
> 
> 중위: BST 정렬 출력용
> 
> 후위: 서브트리 계산용 (ex. 높이, 합계 등)


## 3. Graph 기초

```java
import java.util.ArrayList;

public class Main {
    static ArrayList<ArrayList<Integer>> graph;
    static boolean[] visited;
    
    public static void DFS(int v) {
        visited[v] = true;
        for (int nv : graph.get(v)) {
            if (!visited[nv]) DFS(nv);
        }
    }
}
```
> [S07_P10.java](../../src/inflearn/section07_recursive_tree_graph/INF_S07_P10.java) 참고


## 3. 주의 포인트
|     코드 / 개념      | 요약                        |
|:----------------:|:--------------------------|
| StackOverflow 방지 | 입력 크기 크면 DFS 대신 BFS 고려    |
|   visited 초기화    | 테스트 케이스마다 리셋              |
|    인접리스트 초기화     | 꼭 n+1 크기로 생성 (1-index 기준) |
|    그래프 입력 순서     | graph.add() 순서 실수 주의      |


## **핵심 요약**
- 재귀 → Base Case 필수 / 호출 순서로 로직 제어
- 트리 순회 → 전위/중위/후위 위치만 다름
- DFS 그래프 탐색 → visited + 인접리스트 필수
- 무방향 그래프 → addEdge 양방향
- StackOverflow 조심 → BFS로 대체 가능

## 문제 다시 풀기
### **[Inflearn](../../src/inflearn/section07_recursive_tree_graph)**
| 분류  | 문제 이름                                                                                              | 포인트 메모             |
|:---:|:---------------------------------------------------------------------------------------------------|:-------------------|
| 기초  | [07-02 재귀를 이용한 이진수 출력](../../src/inflearn/section07_recursive_tree_graph/INF_S07_P02.java)         | 감잡기 문제             |
| 기초  | [07-03 팩토리얼](../../src/inflearn/section07_recursive_tree_graph/INF_S07_P03.java)                   | 감잡기 문제             |
| 기초  | [07-04 피보나치 수열](../../src/inflearn/section07_recursive_tree_graph/INF_S07_P04.java)                | 감잡기 문제             |
| 필수  | [07-05 이진트리 순회(깊이 우선탐색)](../../src/inflearn/section07_recursive_tree_graph/INF_S07_P05.java)       | Node 클래스 생성        |
| 필수  | [07-06 부분집합 구하기(DFS)](../../src/inflearn/section07_recursive_tree_graph/INF_S07_P06.java)          | 필수 지식              |
| 필수  | [07-07 이진트리 순회(넓이우선탐색 : 레벨탐색)](../../src/inflearn/section07_recursive_tree_graph/INF_S07_P07.java) | 필수 지식              |
| 감잡기 | [07-08 송아지 찾기(BFS: 상태트리탐색)](../../src/inflearn/section07_recursive_tree_graph/INF_S07_P08.java)    | 필수 지식              |
| 감잡기 | [07-10 그래프와 인접 행렬](../../src/inflearn/section07_recursive_tree_graph/INF_S07_P10.java)             | 그래프 필수 지식          |
| 필수  | [07-11 경로 탐색(인접행렬)](../../src/inflearn/section07_recursive_tree_graph/INF_S07_P11.java)            | 그래프 탐색             |
| 응용  | [07-12 경로 탐색(인접리스트)](../../src/inflearn/section07_recursive_tree_graph/INF_S07_P12.java)           | ArrayList로 메모리 최적화 |
| 응용  | [07-13 그래프 최단거리(BFS)](../../src/inflearn/section07_recursive_tree_graph/INF_S07_P13.java)          | 반복 학습 필요           |