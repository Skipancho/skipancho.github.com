---
layout: single
title: "[코테 공부(Kotlin) - 프로그래머스] 파괴되지 않은 건물 "
categories: 
    - CodingTest
tags:
    - Kotlin
    - CodingTest
    - programmers
    - lv3
    - kakao

toc: true
toc_sticky: true
---

# 문제
## 문제 출처
프로그래머스 : [https://programmers.co.kr/learn/courses/30/lessons/92344](https://programmers.co.kr/learn/courses/30/lessons/92344)

## 문제 설명

[본 문제는 정확성과 효율성 테스트 각각 점수가 있는 문제입니다.]

N x M 크기의 행렬 모양의 게임 맵이 있습니다. 이 맵에는 내구도를 가진 건물이 각 칸마다 하나씩 있습니다. 적은 이 건물들을 공격하여 파괴하려고 합니다. 건물은 적의 공격을 받으면 내구도가 감소하고 내구도가 0이하가 되면 파괴됩니다. 반대로, 아군은 회복 스킬을 사용하여 건물들의 내구도를 높이려고 합니다.

적의 공격과 아군의 회복 스킬은 항상 직사각형 모양입니다.
예를 들어, 아래 사진은 크기가 4 x 5인 맵에 내구도가 5인 건물들이 있는 상태입니다.

![image](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/9932445f-244d-4188-a559-f16044cfa4d3/04_2022_%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8E%E1%85%A2%E1%84%86%E1%85%AE%E1%86%AB%E1%84%8C%E1%85%A6_%E1%84%91%E1%85%A1%E1%84%80%E1%85%AC%E1%84%83%E1%85%AC%E1%84%8C%E1%85%B5%E1%84%8B%E1%85%A1%E1%86%AD%E1%84%8B%E1%85%B3%E1%86%AB%E1%84%80%E1%85%A5%E1%86%AB%E1%84%86%E1%85%AE%E1%86%AF_01.png){: width="80%" .align-center}

첫 번째로 적이 맵의 (0,0)부터 (3,4)까지 공격하여 4만큼 건물의 내구도를 낮추면 아래와 같은 상태가 됩니다.

![image](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/2a3df058-d7b6-4317-9352-8f9713a9424a/04_2022_%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8E%E1%85%A2%E1%84%86%E1%85%AE%E1%86%AB%E1%84%8C%E1%85%A6_%E1%84%91%E1%85%A1%E1%84%80%E1%85%AC%E1%84%83%E1%85%AC%E1%84%8C%E1%85%B5%E1%84%8B%E1%85%A1%E1%86%AD%E1%84%8B%E1%85%B3%E1%86%AB%E1%84%80%E1%85%A5%E1%86%AB%E1%84%86%E1%85%AE%E1%86%AF_02.png){: width="80%" .align-center}

두 번째로 적이 맵의 (2,0)부터 (2,3)까지 공격하여 2만큼 건물의 내구도를 낮추면 아래와 같이 4개의 건물이 파괴되는 상태가 됩니다.

![image](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/94a07a93-71e3-447c-83cf-f855176e28c1/04_2022_%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8E%E1%85%A2%E1%84%86%E1%85%AE%E1%86%AB%E1%84%8C%E1%85%A6_%E1%84%91%E1%85%A1%E1%84%80%E1%85%AC%E1%84%83%E1%85%AC%E1%84%8C%E1%85%B5%E1%84%8B%E1%85%A1%E1%86%AD%E1%84%8B%E1%85%B3%E1%86%AB%E1%84%80%E1%85%A5%E1%86%AB%E1%84%86%E1%85%AE%E1%86%AF_03.png){: width="80%" .align-center}

세 번째로 아군이 맵의 (1,0)부터 (3,1)까지 회복하여 2만큼 건물의 내구도를 높이면 아래와 같이 2개의 건물이 파괴되었다가 복구되고 2개의 건물만 파괴되어있는 상태가 됩니다.

![image](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/145dfcf7-02aa-44fd-b01b-ff56fb5b0dad/04_2022_%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8E%E1%85%A2%E1%84%86%E1%85%AE%E1%86%AB%E1%84%8C%E1%85%A6_%E1%84%91%E1%85%A1%E1%84%80%E1%85%AC%E1%84%83%E1%85%AC%E1%84%8C%E1%85%B5%E1%84%8B%E1%85%A1%E1%86%AD%E1%84%8B%E1%85%B3%E1%86%AB%E1%84%80%E1%85%A5%E1%86%AB%E1%84%86%E1%85%AE%E1%86%AF_04.png){: width="80%" .align-center}

마지막으로 적이 맵의 (0,1)부터 (3,3)까지 공격하여 1만큼 건물의 내구도를 낮추면 아래와 같이 8개의 건물이 더 파괴되어 총 10개의 건물이 파괴된 상태가 됩니다. (내구도가 0 이하가 된 이미 파괴된 건물도, 공격을 받으면 계속해서 내구도가 하락하는 것에 유의해주세요.)

![image](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/9ce05af0-e5b9-483a-aeb4-d7c0624c2dfb/04_2022_%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8E%E1%85%A2%E1%84%86%E1%85%AE%E1%86%AB%E1%84%8C%E1%85%A6_%E1%84%91%E1%85%A1%E1%84%80%E1%85%AC%E1%84%83%E1%85%AC%E1%84%8C%E1%85%B5%E1%84%8B%E1%85%A1%E1%86%AD%E1%84%8B%E1%85%B3%E1%86%AB%E1%84%80%E1%85%A5%E1%86%AB%E1%84%86%E1%85%AE%E1%86%AF_05.png){: width="80%" .align-center}

최종적으로 총 10개의 건물이 파괴되지 않았습니다.

건물의 내구도를 나타내는 2차원 정수 배열 board와 적의 공격 혹은 아군의 회복 스킬을 나타내는 2차원 정수 배열 skill이 매개변수로 주어집니다. 적의 공격 혹은 아군의 회복 스킬이 모두 끝난 뒤 파괴되지 않은 건물의 개수를 return하는 solution함수를 완성해 주세요.

---

**제한사항**
- 1 ≤ board의 행의 길이 (= N) ≤ 1,000
- 1 ≤ board의 열의 길이 (= M) ≤ 1,000
- 1 ≤ board의 원소 (각 건물의 내구도) ≤ 1,000
- 1 ≤ skill의 행의 길이 ≤ 250,000
- skill의 열의 길이 = 6
- skill의 각 행은 [type, r1, c1, r2, c2, degree]형태를 가지고 있습니다.
    + type은 1 혹은 2입니다.
        * type이 1일 경우는 적의 공격을 의미합니다. 건물의 내구도를 낮춥니다.
        * type이 2일 경우는 아군의 회복 스킬을 의미합니다. 건물의 내구도를 높입니다.
    + (r1, c1)부터 (r2, c2)까지 직사각형 모양의 범위 안에 있는 건물의 내구도를 degree 만큼 낮추거나 높인다는 뜻입니다.
        * 0 ≤ r1 ≤ r2 < board의 행의 길이
        * 0 ≤ c1 ≤ c2 < board의 열의 길이
        * 1 ≤ degree ≤ 500
        * type이 1이면 degree만큼 건물의 내구도를 낮춥니다.
        * type이 2이면 degree만큼 건물의 내구도를 높입니다.
- 건물은 파괴되었다가 회복 스킬을 받아 내구도가 1이상이 되면 파괴되지 않은 상태가 됩니다. 즉, 최종적으로 건물의 내구도가 1이상이면 파괴되지 않은 건물입니다.


**정확성 테스트 케이스 제한 사항**
- 1 ≤ board의 행의 길이 (= N) ≤ 100
- 1 ≤ board의 열의 길이 (= M) ≤ 100
- 1 ≤ board의 원소 (각 건물의 내구도) ≤ 100
- 1 ≤ skill의 행의 길이 ≤ 100
- 1 ≤ degree ≤ 100

**효율성 테스트 케이스 제한 사항**
- 주어진 조건 외 추가 제한사항 없습니다.

---

**입출력 예**

|board|	skill|	result|
|-|-|:-:|
|[[5,5,5,5,5],[5,5,5,5,5],[5,5,5,5,5],[5,5,5,5,5]]	|[[1,0,0,3,4,4],[1,2,0,2,3,2],[2,1,0,3,1,2],[1,0,1,3,3,1]]|	10|
|[[1,2,3],[4,5,6],[7,8,9]]|	[[1,1,1,2,2,4],[1,0,0,1,1,2],[2,2,0,2,0,100]]|	6|

---

**입출력 예 설명**

입출력 예 #1

문제 예시와 같습니다.

입출력 예 #2

\<초기 맵 상태\>

![image](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/aa43439f-3d2f-4307-97ce-5910105b4487/04_2022_%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8E%E1%85%A2%E1%84%86%E1%85%AE%E1%86%AB%E1%84%8C%E1%85%A6_%E1%84%91%E1%85%A1%E1%84%80%E1%85%AC%E1%84%83%E1%85%AC%E1%84%8C%E1%85%B5%E1%84%8B%E1%85%A1%E1%86%AD%E1%84%8B%E1%85%B3%E1%86%AB%E1%84%80%E1%85%A5%E1%86%AB%E1%84%86%E1%85%AE%E1%86%AF_06.png){: width="80%" .align-center}

첫 번째로 적이 맵의 (1,1)부터 (2,2)까지 공격하여 4만큼 건물의 내구도를 낮추면 아래와 같은 상태가 됩니다.

![image](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/aa361925-45e4-4bd0-9ef7-e182ed1c6f03/04_2022_%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8E%E1%85%A2%E1%84%86%E1%85%AE%E1%86%AB%E1%84%8C%E1%85%A6_%E1%84%91%E1%85%A1%E1%84%80%E1%85%AC%E1%84%83%E1%85%AC%E1%84%8C%E1%85%B5%E1%84%8B%E1%85%A1%E1%86%AD%E1%84%8B%E1%85%B3%E1%86%AB%E1%84%80%E1%85%A5%E1%86%AB%E1%84%86%E1%85%AE%E1%86%AF_07.png){: width="80%" .align-center}

두 번째로 적이 맵의 (0,0)부터 (1,1)까지 공격하여 2만큼 건물의 내구도를 낮추면 아래와 같은 상태가 됩니다.

![image](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/43c218a1-73c4-4d54-9568-0c21aa7f6365/04_2022_%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8E%E1%85%A2%E1%84%86%E1%85%AE%E1%86%AB%E1%84%8C%E1%85%A6_%E1%84%91%E1%85%A1%E1%84%80%E1%85%AC%E1%84%83%E1%85%AC%E1%84%8C%E1%85%B5%E1%84%8B%E1%85%A1%E1%86%AD%E1%84%8B%E1%85%B3%E1%86%AB%E1%84%80%E1%85%A5%E1%86%AB%E1%84%86%E1%85%AE%E1%86%AF_08.png){: width="80%" .align-center}

마지막으로 아군이 맵의 (2,0)부터 (2,0)까지 회복하여 100만큼 건물의 내구도를 높이면 아래와 같은 상황이 됩니다.

![image](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/5190fee3-8e81-45b7-a79c-1dfc31d8e05f/04_2022_%E1%84%80%E1%85%A9%E1%86%BC%E1%84%8E%E1%85%A2%E1%84%86%E1%85%AE%E1%86%AB%E1%84%8C%E1%85%A6_%E1%84%91%E1%85%A1%E1%84%80%E1%85%AC%E1%84%83%E1%85%AC%E1%84%8C%E1%85%B5%E1%84%8B%E1%85%A1%E1%86%AD%E1%84%8B%E1%85%B3%E1%86%AB%E1%84%80%E1%85%A5%E1%86%AB%E1%84%86%E1%85%AE%E1%86%AF_09.png){: width="80%" .align-center}

총, 6개의 건물이 파괴되지 않았습니다. 따라서 6을 return 해야 합니다.

---

**제한시간 안내**

정확성 테스트 : 10초<br>
효율성 테스트 : 언어별로 작성된 정답 코드의 실행 시간의 적정 배수

---

# 풀이

**나의 풀이**

문제 개념 자체는 매우 쉽다. 다만 효율성을 통과하기 위해서는 거의 필수적으로 누적합을 이용해야 하는 듯 하다. 2차원 배열에서 (a,b)에서 (c,d)까지 n의 변화를 주기 위해서는 (a,b)와 (c+1,d+1)에 n을 더하고 (a,d+1) 와 (c+1, b)에 n을 빼면 된다.

예 ) <br>

|(0,0)|~|(1,1)| +3|
|:-:|:-:|:-:|:-:|
|3|0|-3|0|
|0|0|0|0|
|-3|0|3|0|
|0|0|0|0|

누적합 계산

|(0,0)|~|(1,1)| +3|
|:-:|:-:|:-:|:-:|
|3|3|0|0|
|3|3|0|0|
|0|0|0|0|
|0|0|0|0|


위의 예시 처럼 누적합을 진행할 2차원 행렬을 만든 뒤, 누적합을 하고
원래의 행렬에 합하면서 최종 건물 내구도가 1이상인 경우를 세어주면 된다.

```kotlin
fun solution(board: Array<IntArray>, skill: Array<IntArray>): Int {
    var answer: Int = 0
    val array = Array(board.size+1) { IntArray(board[0].size+1) { 0 } }
    for (s in skill){
        val r1 = s[1]
        val r2 = s[3] + 1
        val c1 = s[2]
        val c2 = s[4] + 1
        when(s[0]){
            1 -> {
                array[r1][c1] -= s[5]
                array[r1][c2] += s[5]
                array[r2][c1] += s[5]
                array[r2][c2] -= s[5]
            }
            2 -> {
                array[r1][c1] += s[5]
                array[r1][c2] -= s[5]
                array[r2][c1] -= s[5]
                array[r2][c2] += s[5]
            }
        }
    }
    for (i in array.indices){
        for (j in 1 until array[0].size){
            array[i][j] += array[i][j-1]
        }
    }
    for (j in array[0].indices){
        for (i in 1 until array.size){
            array[i][j] += array[i-1][j]
        }
    }
    for (i in board.indices){
        for (j in board[0].indices){
            if (board[i][j] + array[i][j] > 0)
                answer++
        }
    }
    return answer
}
```