---
layout: single
title: "[코테 공부(Kotlin) - 프로그래머스] 단어 변환 "
categories: 
    - CodingTest
tags:
    - Kotlin
    - programmers
    - lv3

toc: true
toc_sticky: true
---

# 문제
## 문제 출처
프로그래머스 : [단어 변환](https://programmers.co.kr/learn/courses/30/lessons/43163)

## 문제 설명

두 개의 단어 begin, target과 단어의 집합 words가 있습니다. 아래와 같은 규칙을 이용하여 begin에서 target으로 변환하는 가장 짧은 변환 과정을 찾으려고 합니다.

1. 한 번에 한 개의 알파벳만 바꿀 수 있습니다.
2. words에 있는 단어로만 변환할 수 있습니다.

예를 들어 begin이 "hit", target가 "cog", words가 ["hot","dot","dog","lot","log","cog"]라면 "hit" -> "hot" -> "dot" -> "dog" -> "cog"와 같이 4단계를 거쳐 변환할 수 있습니다.

두 개의 단어 begin, target과 단어의 집합 words가 매개변수로 주어질 때, 최소 몇 단계의 과정을 거쳐 begin을 target으로 변환할 수 있는지 return 하도록 solution 함수를 작성해주세요.

---

**제한사항**
- 각 단어는 알파벳 소문자로만 이루어져 있습니다.
- 각 단어의 길이는 3 이상 10 이하이며 모든 단어의 길이는 같습니다.
- words에는 3개 이상 50개 이하의 단어가 있으며 중복되는 단어는 없습니다.
- begin과 target은 같지 않습니다.
- 변환할 수 없는 경우에는 0를 return 합니다.

---

**입출력 예**

|begin	|target	|words	|return|
|-|-|-|-|
|"hit"|	"cog"	|["hot", "dot", "dog", "lot", "log", "cog"]|	4|
|"hit"|	"cog"|	["hot", "dot", "dog", "lot", "log"]|	0|

---

**입출력 예 설명**

예제 #1

문제에 나온 예와 같습니다.

예제 #2

target인 "cog"는 words 안에 없기 때문에 변환할 수 없습니다.

---

# 풀이

bfs를 이용해서 풀면 될 거 같긴 한데 문제에 효율성 테스트는 없는걸 보고 그냥 플로이드 워셜을 이용해 문제를 해결했다.

그래프로 생각을 하면 문자 1개만 다른 단어 사이에만 엣지가 있고 그 거리가 1이라고 생각하면 된다. 그리고 플로이드 워셜 알고리즘을 사용해 각 단어 사이의 최단 거리를 구하고 마지막으로 시작 단어와 타겟 단어 사이의 최단 거리를 반환한다.

1. words에는 begin이 없으므로 begin을 포함한 배열을 만든다.
2. 확장한 배열을 기준으로 각 단어사이의 거리를 2차원 배열로 만든다.
3. Floyd-Warshall 알고리즘을 이용해 각 단어 사이의 최단 거리를 구한다.
4. begin과 target 사이의 최단 거리를 반환한다.

```kotlin
class Solution {
    fun solution(begin: String, target: String, words: Array<String>): Int {
        if (!words.contains(target)) return 0
        val extendWords = words.plus(begin)
        val route = Array(extendWords.size){IntArray(extendWords.size){51}}
        val start = extendWords.size - 1
        var end = -1
        for (i in extendWords.indices){
            route[i][i] = 0
            if (extendWords[i] == target){
                end = i
            }
            for (j in extendWords.indices){
                if (i != j && route[i][j] == 51 && wordCheck(extendWords[i],extendWords[j])){
                    route[i][j] = 1
                    route[j][i] = 1
                }
            }
        }

        for (i in extendWords.indices){
            for (j in extendWords.indices){
                for (k in extendWords.indices){
                    if (route[j][k] > route[j][i] + route[i][k])
                        route[j][k] = route[j][i] + route[i][k]
                }
            }
        }
        return route[start][end]
    }

    private fun wordCheck(a: String, b:String) : Boolean{
        var cnt = 0
        for (i in a.indices){
            if (a[i] != b[i]){
                cnt++
                if (cnt >= 2) return false
            }
        }
        return cnt == 1
    }
}
```