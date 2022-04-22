---
layout: single
title: "[코테 공부(Kotlin) - 프로그래머스] 징검다리 "
categories: 
    - CodingTest
tags:
    - Kotlin
    - programmers
    - lv4
    - BinarySearch

toc: true
toc_sticky: true
---

# 문제
## 문제 출처
프로그래머스 : [징검다리](https://programmers.co.kr/learn/courses/30/lessons/43236?language=kotlin)

## 문제 설명

출발지점부터 distance만큼 떨어진 곳에 도착지점이 있습니다. 그리고 그사이에는 바위들이 놓여있습니다. 바위 중 몇 개를 제거하려고 합니다.
예를 들어, 도착지점이 25만큼 떨어져 있고, 바위가 [2, 14, 11, 21, 17] 지점에 놓여있을 때 바위 2개를 제거하면 출발지점, 도착지점, 바위 간의 거리가 아래와 같습니다.

|제거한 바위의 위치|	각 바위 사이의 거리|	거리의 최솟값|
|:-:|:-:|:-:|
|[21, 17]	|[2, 9, 3, 11]	|2|
|[2, 21]	|[11, 3, 3, 8]	|3|
|[2, 11]	|[14, 3, 4, 4]	|3|
|[11, 21]	|[2, 12, 3, 8]	|2|
|[2, 14]	|[11, 6, 4, 4]	|4|

위에서 구한 거리의 최솟값 중에 가장 큰 값은 4입니다.

출발지점부터 도착지점까지의 거리 distance, 바위들이 있는 위치를 담은 배열 rocks, 제거할 바위의 수 n이 매개변수로 주어질 때, 바위를 n개 제거한 뒤 각 지점 사이의 거리의 최솟값 중에 가장 큰 값을 return 하도록 solution 함수를 작성해주세요.

---

**제한사항**
- 도착지점까지의 거리 distance는 1 이상 1,000,000,000 이하입니다.
- 바위는 1개 이상 50,000개 이하가 있습니다.
- n 은 1 이상 바위의 개수 이하입니다.

---

**입출력 예**

|distance	|rocks	|n	|return|
|-|
|25	|[2, 14, 11, 21, 17]	|2	|4|

---

# 풀이

**나의 풀이**

이분 탐색 문제는 대부분 mid로 비교할 값을 정하기만 하면 구현자체는 쉽다.
이 문제의 경우 각 바위별 거리의 최솟값을 mid로 정하였다.

예측한 mid값 이하의 거리를 가지는 바위를 전부 다 제거하고 제거한 바위의 수량이
n 보다 클 경우 거리를 더 좁혀야하므로 현재 mid를 다음 end로 지정하고 탐색을 반복한다. 또는 제거한 바위의 수량이 n보다 작을 경우 바위 사이의 간격이 더 넓어야 하므로 mid + 1을 다음 start로 지정하고 탐색한다.

```kotlin
fun solution(distance: Int, rocks: IntArray, n: Int): Int {
    var answer = 0
    var start = 0
    var end = distance
    val newRocks = rocks.sorted().plus(distance)//도착 지점 추가
    while (start < end){
        val mid = (start + end) / 2
        var curRock = 0
        var cnt = 0 //제거한 바위 수
        for(rock in newRocks){
            if (rock - curRock < mid) cnt++ //거리가 mid 이하이면 계속 제거
            else curRock = rock //mid 이상이면 다음 바위로 넘어감
        }
        if (cnt > n) end = mid
        else { 
            if(mid > answer) answer = mid //거리 최댓값 갱신
            start = mid + 1
        }
    }
    return answer
}
```