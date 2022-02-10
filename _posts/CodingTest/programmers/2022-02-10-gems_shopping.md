---
layout: single
title: "[코테 공부(Kotlin) - 프로그래머스][카카오 인턴] 보석 쇼핑 "
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
프로그래머스 : [https://programmers.co.kr/learn/courses/30/lessons/67258](https://programmers.co.kr/learn/courses/30/lessons/67258)

## 문제 설명

[본 문제는 정확성과 효율성 테스트 각각 점수가 있는 문제입니다.]

개발자 출신으로 세계 최고의 갑부가 된 어피치는 스트레스를 받을 때면 이를 풀기 위해 오프라인 매장에 쇼핑을 하러 가곤 합니다.

어피치는 쇼핑을 할 때면 매장 진열대의 특정 범위의 물건들을 모두 싹쓸이 구매하는 습관이 있습니다.

어느 날 스트레스를 풀기 위해 보석 매장에 쇼핑을 하러 간 어피치는 이전처럼 진열대의 특정 범위의 보석을 모두 구매하되 특별히 아래 목적을 달성하고 싶었습니다.

진열된 모든 종류의 보석을 적어도 1개 이상 포함하는 가장 짧은 구간을 찾아서 구매

예를 들어 아래 진열대는 4종류의 보석(RUBY, DIA, EMERALD, SAPPHIRE) 8개가 진열된 예시입니다.

|진열대 번호|	1|	2|	3|	4|	5|	6|	7|	8|
|-|
|보석 이름	|DIA	|RUBY	|RUBY	|DIA	|DIA	|EMERALD	|SAPPHIRE	|DIA|

진열대의 3번부터 7번까지 5개의 보석을 구매하면 모든 종류의 보석을 적어도 하나 이상씩 포함하게 됩니다.

진열대의 3, 4, 6, 7번의 보석만 구매하는 것은 중간에 특정 구간(5번)이 빠지게 되므로 어피치의 쇼핑 습관에 맞지 않습니다.

진열대 번호 순서대로 보석들의 이름이 저장된 배열 gems가 매개변수로 주어집니다. 이때 모든 보석을 하나 이상 포함하는 가장 짧은 구간을 찾아서 return 하도록 solution 함수를 완성해주세요.

가장 짧은 구간의 시작 진열대 번호와 끝 진열대 번호를 차례대로 배열에 담아서 return 하도록 하며, 만약 가장 짧은 구간이 여러 개라면 시작 진열대 번호가 가장 작은 구간을 return 합니다.

---

**제한사항**
- gems 배열의 크기는 1 이상 100,000 이하입니다.
    + gems 배열의 각 원소는 진열대에 나열된 보석을 나타냅니다.
    + gems 배열에는 1번 진열대부터 진열대 번호 순서대로 보석이름이 차례대로 저장되어 있습니다.
    + gems 배열의 각 원소는 길이가 1 이상 10 이하인 알파벳 대문자로만 구성된 문자열입니다.

---

**입출력 예**

|gems	|result|
|-|
|["DIA", "RUBY", "RUBY", "DIA", "DIA", "EMERALD", "SAPPHIRE", "DIA"]|	[3, 7]|
|["AA", "AB", "AC", "AA", "AC"]|	[1, 3]|
|["XYZ", "XYZ", "XYZ"]|	[1, 1]|
|["ZZZ", "YYY", "NNNN", "YYY", "BBB"]	|[1, 5]|

---

**입출력 예 설명**

입출력 예 #1

문제 예시와 같습니다.

입출력 예 #2

3종류의 보석(AA, AB, AC)을 모두 포함하는 가장 짧은 구간은 [1, 3], [2, 4]가 있습니다.
시작 진열대 번호가 더 작은 [1, 3]을 return 해주어야 합니다.

입출력 예 #3

1종류의 보석(XYZ)을 포함하는 가장 짧은 구간은 [1, 1], [2, 2], [3, 3]이 있습니다.
시작 진열대 번호가 가장 작은 [1, 1]을 return 해주어야 합니다.

입출력 예 #4

4종류의 보석(ZZZ, YYY, NNNN, BBB)을 모두 포함하는 구간은 [1, 5]가 유일합니다.
그러므로 [1, 5]를 return 해주어야 합니다.

---

# 풀이

**나의 풀이**

처음에는 슬라이딩 윈도우를 이용해 문제를 해결하고자 했다.

(처음 코드)

```kotlin
class Solution {
    fun solution(gems: Array<String>): IntArray {
        val setSize = gems.groupBy { it }.size
        
        for (size in setSize .. gems.size){
            val map = HashMap<String,Int>()
            for( i in gems.indices){
                if (i < size){
                    map[gems[i]] = map.getOrDefault(gems[i] , 0) + 1
                }else{
                    val cnt = map.getOrDefault(gems[i-size],0)
                    if (cnt <= 1){
                        map.remove(gems[i-size])
                    }else{
                        map[gems[i-size]] = cnt - 1
                    }
                    map[gems[i]] = map.getOrDefault(gems[i] , 0) + 1
                }

                if (setSize == map.size){
                    return intArrayOf(i-size+2,i+1)
                }
            }
        }
        return intArrayOf(1,gems.size)
    }
}
```

다행스럽게도 정확도는 통과했으나 효율성에서 바로 시간 초과 ㅋㅋㅋㅋ

보석이름 - 개수 map을 보석이름 - 보석 인덱스 map으로 바꾼뒤 보석 인덱스 값이 다 확인이 되면 인덱스 젤 작은거 -> start , 젤 큰거 -> end 이런 느낌으로 수정하니 효율성 통과 (휴..)

그 풀이는 다음과 같다

1. 보석 위치가 저장될 map을 만든다.
2. 보석 위치를 순서대로 map에 갱신해준다.
3. map 크기가 보석 종류의 개수와 같으면 map의 값 중 가장 작은 값을 start, 가장 큰 값을 end로 리스트에 저장
4. [start, end] 배열 리스트를 간격이 작은 순, 간격이 같다면 start가 작은 순으로 정렬한다.
5. 정렬한 리스트의 첫 번째 값을 반환 

<br>

```kotlin
class Solution {
    fun solution(gems: Array<String>): IntArray {
        val set = gems.toHashSet()
        val size = set.size
        val map = mutableMapOf<String,Int>() 
        val gemArrays = ArrayList<IntArray>()
        gems.mapIndexed { idx, gem ->
            map.remove(gem)
            map[gem] = idx
            if (map.size == size){
                gemArrays.add(intArrayOf(map.values.iterator().next() + 1,idx + 1))
            }
        }
        gemArrays.sortWith(Comparator { o1, o2 ->
            if (o1[1]-o1[0] == o2[1]-o2[0]){
                o1[1] - o2[1]
            }
            (o1[1]-o1[0])-(o2[1]-o2[0])
        })
        return gemArrays[0]
    }
}
```

start를 map.values.sorted().first()을 이용해서 젤 작은 수를 넣으려고 했는데 시간 초과 ㅠ 생각해보니 어짜피 작은 순으로 map에 추가한거라 
iterator로 꺼내오면 되는 거였다. 수정 후 문제 해결