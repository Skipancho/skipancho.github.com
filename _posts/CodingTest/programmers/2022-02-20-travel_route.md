---
layout: single
title: "[코테 공부(Kotlin) - 프로그래머스] 여행경로 "
categories: 
    - CodingTest
tags:
    - Kotlin
    - programmers
    - lv3
    - dfs

toc: true
toc_sticky: true
---

# 문제
## 문제 출처
프로그래머스 : [여행경로](https://programmers.co.kr/learn/courses/30/lessons/43164)

## 문제 설명

주어진 항공권을 모두 이용하여 여행경로를 짜려고 합니다. 항상 "ICN" 공항에서 출발합니다.

항공권 정보가 담긴 2차원 배열 tickets가 매개변수로 주어질 때, 방문하는 공항 경로를 배열에 담아 return 하도록 solution 함수를 작성해주세요.

---

**제한사항**

- 모든 공항은 알파벳 대문자 3글자로 이루어집니다.
- 주어진 공항 수는 3개 이상 10,000개 이하입니다.
- tickets의 각 행 [a, b]는 a 공항에서 b 공항으로 가는 항공권이 있다는 의미입니다.
- 주어진 항공권은 모두 사용해야 합니다.
- 만일 가능한 경로가 2개 이상일 경우 알파벳 순서가 앞서는 경로를 return 합니다.
- 모든 도시를 방문할 수 없는 경우는 주어지지 않습니다.

---

**입출력 예**

|tickets	|return|
|-|-|
|[["ICN", "JFK"], ["HND", "IAD"], ["JFK", "HND"]]|	["ICN", "JFK", "HND", "IAD"]|
|[["ICN", "SFO"], ["ICN", "ATL"], ["SFO", "ATL"], ["ATL", "ICN"], ["ATL","SFO"]]|	["ICN", "ATL", "ICN", "SFO", "ATL", "SFO"]|

---

**입출력 예 설명**

예제 #1

["ICN", "JFK", "HND", "IAD"] 순으로 방문할 수 있습니다.

예제 #2

["ICN", "SFO", "ATL", "ICN", "ATL", "SFO"] 순으로 방문할 수도 있지만 ["ICN", "ATL", "ICN", "SFO", "ATL", "SFO"] 가 알파벳 순으로 앞섭니다.

---

# 풀이

**나의 풀이**

티켓 정보를 저장한 뒤 dfs를 이용해 티켓으로 만들 수 있는 경로를 모두 탐색한 뒤 모든 티켓을 사용하는 경우를 찾으면 된다. 티켓 정보를 저장할 때 알파벳 순으로 미리 정렬시켜두면 자동으로 가장 먼저 나오는 경로가 가장 알파벳 순서가 앞서는 경로가 된다.

```kotlin
class Solution {
    private val map = HashMap<String,ArrayList<String>>()
    private val arrayList = ArrayList<List<String>>()

    fun solution(tickets: Array<Array<String>>): Array<String> {
        var answer = arrayOf<String>()
        val t = mutableListOf<Ticket>()

        tickets.forEach {
            t.add(Ticket(it[0],it[1]))
            if (map[it[0]] == null){
                map[it[0]] = arrayListOf(it[1])
            }else{
                map[it[0]]?.add(it[1])
            }
        }
        map.forEach { (t, u) ->
            u.sort()
        }
        val startTicket = Ticket("","ICN")
        dfs(startTicket,t, mutableListOf<String>(),tickets.size + 1)
        return arrayList[0].toTypedArray()
    }

    private fun dfs(pos : Ticket, t : MutableList<Ticket>, route:MutableList<String>,n : Int){
        val new_tickets = t.map { it }.toMutableList()
        val new_route = route.map { it }.toMutableList()
        if (new_route.isNotEmpty())
            new_tickets.remove(pos)
        new_route.add(pos.destination)

        if (new_tickets.isEmpty()){
            if (new_route.size == n) {
                arrayList.add(new_route)
            }
            return
        }
        val cur = pos.destination
        map[cur]?.forEach {
            if (new_tickets.contains(Ticket(cur,it))){
                dfs(Ticket(cur,it),new_tickets,new_route, n)
            }
        }
    }

    data class Ticket(
        val start : String,
        val destination : String
    )
}
```

