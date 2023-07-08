---
title: "[Algorithm] 순열(Permutation)과 조합(Combination)"
excerpt: "순열과 조합 재귀로 구현하기"

categories:
  - Algorithm
tags:
  [Java, Algorithm]

toc: true
toc_sticky: true

date: 2023-07-08
last_modified_at: 2023-07-08
---

# 구현

순열과 조합의 구현방법에는 재귀, DFS - Backtracking 등이 있다. (재귀만 있는 것은 아님)  

## 1) 순열(Permutation)

- 서로 다른 n개 중에 r개를 선택하는 경우의 수
- **순서 O**
- ![](https://blog.kakaocdn.net/dn/cErpNj/btqXMe1l0qb/B8ip0bTc63gKHScIVihPgK/img.png)

```java
// 순열
private static void permutation(ArrayList<Integer> list, int count) {
    // 선택개수만큼 다 뽑았으면 출력 후 재귀종료
    if (count == 0) {
        System.out.println(list);
        permutationCnt++;
        return;
    }

    for (int i = 0; i < n; i++) {
        if (list.contains(targetArr[i])) continue; // 이미 선택한 원소면 다음진행(기존 visited 방문여부로 확인)

        list.add(targetArr[i]); // 선택
        permutation(list, count - 1); // 선택할 때마다 -1
        list.remove(list.size() - 1); // 재귀 위해 마지막에 넣은 원소 제거(선택 완료 후)
    }
}
```

- 유의할 점

  ```java
  list.remove(list.size() - 1); // 재귀 위해 마지막에 넣은 원소 제거(선택 완료 후)
  ```

  - 위와 같이 재귀종료 후 마지막 원소를 제거해야 다음 루프 (및 루프종료 + 재귀종료)를 타면서 다시 선택할 수 있다. <br/>ex) 3개를 선택한다고 했을 때, {1, 2, 4} -> {1, 3, 2} 와 같이 앞에서 선택했던 2를 다시 선택 

<br/>

## 2) 중복순열(Permutation  with Repetition)

- 서로 다른 n개 중에 r개를 선택하는 경우의 수 + **집합의 원소를 중복하여 선택가능**
- **순서 O**
- n<sup>r </sup>

```java
// 중복순열
private static void rptPermutation(ArrayList<Integer> list, int count) {
    // 선택개수만큼 다 뽑았으면 출력 후 재귀종료
    if (count == 0) {
        System.out.println(list);
        rptPermutationCnt++;
        return;
    }

    for (int i = 0; i < n; i++) {
        list.add(targetArr[i]); // 선택
        rptPermutation(list, count - 1); // 선택할 때마다 -1
        list.remove(list.size() - 1); // 재귀 위해 마지막에 넣은 원소 제거(선택 완료 후)
    }
}
```

<br/>

## 3) 조합(Combination)

- 서로 다른 n개 중에 r개를 선택하는 경우의 수

- **순서 X**

- ![](https://blog.kakaocdn.net/dn/bQEZNg/btq21L8ylyu/al7s6rnAwMCZLXZO4hoIC0/img.png)

  

```java
// 조합
private static void combination(ArrayList<Integer> list, int index, int count) {
    // 선택개수만큼 다 뽑았으면 출력 후 재귀종료
    if (count == 0) {
        System.out.println(list);
        combinationCnt++;
        return;
    }

    for (int i = index; i < n; i++) {
        list.add(targetArr[i]); // 선택
        combination(list, i + 1, count - 1); // 선택할 때마다 -1
        list.remove(list.size() - 1); // 재귀 위해 마지막에 넣은 원소 제거(선택 완료 후)
    }
}
```

<br/>

## 4) 중복조합(Combination  with Repetition)

- 서로 다른 n개 중에 r개를 선택하는 경우의 수 + **집합의 원소를 중복하여 선택가능**
- **순서 X**
- ![](https://blog.kakaocdn.net/dn/bttqme/btqXHfNU33g/7t4FEAp9Qv1goDkkNALQO0/img.png)

```java
// 중복조합
private static void rptCombination(ArrayList<Integer> list, int index, int count) {
    // 선택개수만큼 다 뽑았으면 출력 후 재귀종료
    if (count == 0) {
        System.out.println(list);
        rptCombinationCnt++;
        return;
    }

    for (int i = index; i < n; i++) {
        list.add(targetArr[i]); // 선택
        rptCombination(list, i, count - 1); // 선택할 때마다 -1
        list.remove(list.size() - 1); // 재귀 위해 마지막에 넣은 원소 제거(선택 완료 후)
    }
}
```

<br/>

<br/>



---

## 전체코드

```java
import java.util.ArrayList;

/*
    ** 순열/ 중복순열/ 조합/ 중복조합
    * 재귀구현
    * 1부터 4까지의 자연수 4개중 2개를 선택
 */
public class Permutation_2 {
    static int[] targetArr;         // 대상 집합(뽑을 기준배열)
    static int n;                   // 기준배열 길이
    static int permutationCnt;      // 순열의 경우의 수
    static int rptPermutationCnt;   // 중복순열의 경우의 수
    static int combinationCnt;      // 조합의 경우의 수
    static int rptCombinationCnt;   // 중복조합의 경우의 수
    static int r;                   // 뽑을 개수

    public static void main(String[] args) {
        targetArr = new int[] {1, 2, 3, 4};
        n = targetArr.length;
        r = 2;

        permutation(new ArrayList<Integer>(), r);
        System.out.println("[순열의 경우의 수] = " + permutationCnt);
        System.out.println("-------------------");

        rptPermutation(new ArrayList<Integer>(), r);
        System.out.println("[중복순열의 경우의 수] = " + rptPermutationCnt);
        System.out.println("-------------------");

        combination(new ArrayList<Integer>(), 0, r);
        System.out.println("[조합의 경우의 수] = " + combinationCnt);
        System.out.println("-------------------");

        rptCombination(new ArrayList<Integer>(), 0, r);
        System.out.println("[중복조합의 경우의 수] = " + rptCombinationCnt);

    }

    // 순열
    private static void permutation(ArrayList<Integer> list, int count) {
        // 선택개수만큼 다 뽑았으면 출력 후 재귀종료
        if (count == 0) {
            System.out.println(list);
            permutationCnt++;
            return;
        }

        for (int i = 0; i < n; i++) {
            if (list.contains(targetArr[i])) continue; // 이미 선택한 원소면 다음진행(기존 visited 방문여부로 확인)

            list.add(targetArr[i]); // 선택
            permutation(list, count - 1); // 선택할 때마다 -1
            list.remove(list.size() - 1); // 재귀 위해 마지막에 넣은 원소 제거(선택 완료 후)
        }
    }

    // 중복순열
    private static void rptPermutation(ArrayList<Integer> list, int count) {
        // 선택개수만큼 다 뽑았으면 출력 후 재귀종료
        if (count == 0) {
            System.out.println(list);
            rptPermutationCnt++;
            return;
        }

        for (int i = 0; i < n; i++) {
            list.add(targetArr[i]); // 선택
            rptPermutation(list, count - 1); // 선택할 때마다 -1
            list.remove(list.size() - 1); // 재귀 위해 마지막에 넣은 원소 제거(선택 완료 후)
        }
    }

    // 조합
    private static void combination(ArrayList<Integer> list, int index, int count) {
        // 선택개수만큼 다 뽑았으면 출력 후 재귀종료
        if (count == 0) {
            System.out.println(list);
            combinationCnt++;
            return;
        }

        for (int i = index; i < n; i++) {
            list.add(targetArr[i]); // 선택
            combination(list, i + 1, count - 1); // 선택할 때마다 -1
            list.remove(list.size() - 1); // 재귀 위해 마지막에 넣은 원소 제거(선택 완료 후)
        }
    }

    // 중복조합
    private static void rptCombination(ArrayList<Integer> list, int index, int count) {
        // 선택개수만큼 다 뽑았으면 출력 후 재귀종료
        if (count == 0) {
            System.out.println(list);
            rptCombinationCnt++;
            return;
        }

        for (int i = index; i < n; i++) {
            list.add(targetArr[i]); // 선택
            rptCombination(list, i, count - 1); // 선택할 때마다 -1
            list.remove(list.size() - 1); // 재귀 위해 마지막에 넣은 원소 제거(선택 완료 후)
        }
    }
}

```



#### Reference

- [https://hanyeop.tistory.com/362](https://hanyeop.tistory.com/362)
