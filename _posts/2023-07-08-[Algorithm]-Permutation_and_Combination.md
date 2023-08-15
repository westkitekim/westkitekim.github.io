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
last_modified_at: 2023-08-15
---

<style>
    #highlight {
        background-color: #e9e97f
    }
</style>

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

### <span class="highlight">주의</span>!!!!!!

하지만 위 코드처럼 list.contains로 이미 선택한 원소를 판별한다면 입력된 배열에서 중복되는 원소가 있는 경우, 결과값이 달라지는 문제가 발생한다. <br/>

``` java
targetArr = new char[] {'a', 'a', 'b', 'c'};
```

입력 배열이 위와 같이 주어진다면 두 번째 'a' 는 중복원소 판별에서 skip되어 경우의 수가 적어지게 된다.<br/>따라서 아래 코드처럼 방문여부 배열을 따로 만들어 이미 선택한 원소인지 확인한다.

1. **출력값을 list로 사용하는 경우**<br/>

   ```java
   // 순열
   private static void permutation(ArrayList<String> list, int count) {
       // 선택개수만큼 다 뽑았으면 출력 후 재귀종료
       if (count == 0) {
           System.out.println(list);
           permutationCnt++;
           return;
       }
   
       for (int i = 0; i < n; i++) {
           if (!isVisited[i]) {
               isVisited[i] = true;
               list.add(targetArr[i]); // 선택
               permutation(list, count - 1); // 선택할 때마다 -1
               list.remove(list.size() - 1);
               isVisited[i] = false;
           }
       }
   }
   ```

   

2. **depth, 출력값을 배열로 변경**<br/>

   ```java
   public class Permutation {
   
       public static void main(String[] args) {
           char[] input = new char[]{'a','a','b','c'}; // nPr에서 n의 값 => 선택가능한 가지의 수
           int r = 2; // 몇개 뽑을지 정하는 수 => output의 크기가 됨
           nPr(0, r, input, new boolean[input.length], new char[r]);
       }
   
       static void nPr(int depth, int r, char[] input, boolean[] isVisited, char[] output) {
           if(depth == r) {                // dfs의 깊이가 출력길이 r 만큼 되면 출력
               for(char c : output) {
                   System.out.print(c);
               }
               System.out.println();
               return;
           }
   
           for(int i=0; i<input.length; i++) {
               if(!isVisited[i]) {                 // input의 값들이 이미 선정되어 output에 들어갔는지 안들어갔는지 확인
                   output[depth] = input[i];       // output에 선택한 값 입력
                   isVisited[i] = true;            // 해당 값을 output에 입력했다고 표시
                   nPr(depth+1, r, input, isVisited, output);
                   isVisited[i] = false;           // 바로 윗줄에서 재귀를 불렀으므로 선택한 값은 다시 사용할 수 있도록 해야함
               }
           }
       }
   }
   ```

   

> ***맨 하단의 전체코드 버전2 확인*** <br/>(소중한 피드백을 주신 아무개님 감사합니다.)

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

- **순서 X -> 조합은 순서 상관없으니 종류로 생각!**

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
- **순서 X -> 조합은 순서 상관없으니 종류로 생각!**
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

### 버전2

``` java
/*
    ** 순열/ 중복순열/ 조합/ 중복조합 **
    재귀구현
    1부터 4까지의 자연수 4개중 2개를 선택

    2023-08-14 코드 수정
    - list.contains -> boolean isVisited
    - contains 사용시 중복 배열 발생
 */
public class Permutation_4_boolean {
    static char[] input;            // 대상 집합(뽑을 기준배열)
    static int r;                   // 뽑을 개수
    static int n;                   // 기준배열 길이
    static int permutationCnt;      // 순열의 경우의 수
    static int rptPermutationCnt;   // 중복순열의 경우의 수
    static int combinationCnt;      // 조합의 경우의 수
    static int rptCombinationCnt;   // 중복조합의 경우의 수

    public static void main(String[] args) {
        input = new char[]{'a','a','b','c'}; // nPr에서 n의 값 => 선택가능한 가지의 수
        n = input.length;
        r = 2; // 몇개 뽑을지 정하는 수 => output의 크기가 됨

        permutation(0, r, input, new boolean[n], new char[r]);
        System.out.println("[순열의 경우의 수] = " + permutationCnt);
        System.out.println("-------------------");

        rptPermutation(0, r, input, new char[r]);
        System.out.println("[중복순열의 경우의 수] = " + rptPermutationCnt);
        System.out.println("-------------------");

        combination(0, r, 0, input, new boolean[n]);
        System.out.println("[조합의 경우의 수] = " + combinationCnt);
        System.out.println("-------------------");

        rptCombination(0, r, 0, input, new char[r]);
        System.out.println("[중복조합의 경우의 수] = " + rptCombinationCnt);

    }

    // 1. 순열
    static void permutation(int depth, int r, char[] input, boolean[] isVisited, char[] output) {
        if(depth == r) {                // dfs의 깊이가 출력길이 r 만큼 되면 출력
            for(char c : output) {
                System.out.print(c);
            }
            System.out.println();
            permutationCnt++;
            return;
        }

        for(int i = 0; i < input.length; i++) {
            if(!isVisited[i]) {                 // input의 값들이 이미 선정되어 output에 들어갔는지 안들어갔는지 확인
                output[depth] = input[i];       // output에 선택한 값 입력
                isVisited[i] = true;            // 해당 값을 output에 입력했다고 표시
                permutation(depth+1, r, input, isVisited, output);
                isVisited[i] = false;           // 바로 윗줄에서 재귀를 불렀으므로 선택한 값은 다시 사용할 수 있도록 해야함
            }
        }
    }

    // 2. 중복순열
    private static void rptPermutation(int depth, int r, char[] input, char[] output) {
        if(depth == r) {                // dfs의 깊이가 출력길이 r 만큼 되면 출력
            for(char c : output) {
                System.out.print(c);
            }
            System.out.println();
            rptPermutationCnt++;
            return;
        }

        // 방문체크X
        for(int i = 0; i < input.length; i++) {
            output[depth] = input[i];       // output에 선택한 값 입력
            rptPermutation(depth + 1, r, input, output);
        }
    }

    // 3. 조합
    private static void combination(int depth, int r, int start, char[] input, boolean[] isVisited) {
        if(depth == r) {                // dfs의 깊이가 출력길이 r 만큼 되면 출력
            for (int i = 0; i < n; i++) {
                if (isVisited[i]) {
                    System.out.print(input[i]);
                }
            }
            System.out.println();
            combinationCnt++;
            return;
        }

        for (int i = start; i < n; i++) {
            isVisited[i] = true;
            combination(depth + 1, r, i + 1, input, isVisited);
            isVisited[i] = false;
        }
    }

    // 4. 중복조합
    private static void rptCombination(int depth, int r, int start, char[] input, char[] output) {
        // 선택개수만큼 다 뽑았으면 출력 후 재귀종료
        if(depth == r) {                // dfs의 깊이가 출력길이 r 만큼 되면 출력
            for(char c : output) {
                System.out.print(c);
            }
            System.out.println();
            rptCombinationCnt++;
            return;
        }

        for (int i = start; i < n; i++) {
            output[depth] = input[i];
            rptCombination(depth + 1, r, i, input, output); // start index 증가시키지 않는다
        }
    }
}
```



### 버전1

- 출력값 : List, 방문체크 배열 사용X

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
