---
layout: single
title: "[코테 공부(Kotlin) - 프로그래머스] k진수에서 소수 개수 구하기 "
categories: 
    - CodingTest
tags:
    - Kotlin
    - CodingTest
    - programmers
    - lv2
    - kakao

toc: true
toc_sticky: true
---

# 문제
## 문제 출처
프로그래머스 : [https://programmers.co.kr/learn/courses/30/lessons/92335](https://programmers.co.kr/learn/courses/30/lessons/92335)

## 문제 설명

양의 정수 n이 주어집니다. 이 숫자를 k진수로 바꿨을 때, 변환된 수 안에 아래 조건에 맞는 소수(Prime number)가 몇 개인지 알아보려 합니다.

- 0P0처럼 소수 양쪽에 0이 있는 경우
- P0처럼 소수 오른쪽에만 0이 있고 왼쪽에는 아무것도 없는 경우
- 0P처럼 소수 왼쪽에만 0이 있고 오른쪽에는 아무것도 없는 경우
- P처럼 소수 양쪽에 아무것도 없는 경우
- 단, P는 각 자릿수에 0을 포함하지 않는 소수입니다.
    + 예를 들어, 101은 P가 될 수 없습니다.

예를 들어, 437674을 3진수로 바꾸면 211020101011입니다. 여기서 찾을 수 있는 조건에 맞는 소수는 왼쪽부터 순서대로 211, 2, 11이 있으며, 총 3개입니다. (211, 2, 11을 k진법으로 보았을 때가 아닌, 10진법으로 보았을 때 소수여야 한다는 점에 주의합니다.) 211은 P0 형태에서 찾을 수 있으며, 2는 0P0에서, 11은 0P에서 찾을 수 있습니다.

정수 n과 k가 매개변수로 주어집니다. n을 k진수로 바꿨을 때, 변환된 수 안에서 찾을 수 있는 위 조건에 맞는 소수의 개수를 return 하도록 solution 함수를 완성해 주세요.

---

**제한사항**

- 1 ≤ n ≤ 1,000,000
- 3 ≤ k ≤ 10

---

**입출력 예**

|n	|k	|result|
|437674|	3|	3|
|110011|	10|	2|

---

**입출력 예 설명**

입출력 예 #1

문제 예시와 같습니다.

입출력 예 #2

110011을 10진수로 바꾸면 110011입니다. 여기서 찾을 수 있는 조건에 맞는 소수는 11, 11 2개입니다. 이와 같이, 중복되는 소수를 발견하더라도 모두 따로 세어야 합니다.

---

**제한시간 안내**<br>
정확성 테스트 : 10초

---

# 풀이
**나의 풀이**

처음에는 조건을 읽고 뭔소린가 했지만 조금만 생각해보면 0을 기준으로 수를 분리해 소수인지 확인하는 문제란걸 알 수 있다.
수를 쉽게 분리하기 위하여 문자열로 변환 -> split("0")으로 0을 기준으로 분리 -> 다시 정수형으로 변환시키는 과정을 거쳤다.


소수를 판별하는 함수를 만들 때 별 생각없이 Int로 입력 값을 넣었다가 런타임 에러 뜨는걸 보고 후다닥 Long으로 고침 ㅋㅋㅋ

에라토스테네스의 체를 만들어야 하나 고민도 했지만 소수 범위도 잘 모르겠고 오히려 손해날거 같아서 안만들었지만
무난하게 통과되어서 그냥 넘어갔다.

풀이과정은 다음과 같다.

1. n을 k진수형으로 변환 (이때 문자열로 변환)
2. 문자열을 0을 기준으로 분리
3. 분리한 수들을 다시 정수형으로 변환해 소수인지 판별
4. 소수의 개수를 반환

<br>

```kotlin
fun solution(n: Int, k: Int): Int {
    var answer: Int = 0
    val nums = numbers(n,k).split("0")
    for(num in nums){
        if (num.isNotBlank()&& num != "1" && isPrime(num.toLong()))
            answer++
    }
    return answer
}

private fun numbers(n : Int, k : Int) : String{
    var result = ""
    var temp = n
    while(temp > 0){
        result = "${temp%k}$result"
        temp /= k
    }
    return result
}
private fun isPrime(n : Long) : Boolean{
    //if (n == 1L) return false
    var i = 2L
    while (i*i <= n){
        if (n%i==0L) return false
        i++
    }
    return true
}
```

---

<br>

**다른 사람의 풀이**

이거는 그냥 내가 모르던 부분이라 보고 놀라서 들고옴

```kotlin
import java.math.*

class Solution {
    fun solution(n: Int, k: Int): Int {
        var answer: Int = 0
        val newN = n.toString(k).split("0")
        for(i in newN) {
            if(i == "" || i == "0" || i == "1") continue
            if(BigInteger(i).isProbablePrime(1)) answer ++
        }
        return answer
    }
}
```
<br><br><br>
첫 번째로 놀란 부분은 바로 다음 함수의 존재인데

```kotlin
public inline fun Int.toString(radix : Int) : String
```

아니 문자열로 변환할 때 n진수 변환까지 바로 해준다고?? 약간의 충격과 놀라움 또 하나 배워갑니다~(꾸벅)

또 BigInteger 클래스의 isProbablePrime() 메소드인데 에라토스테네스의 체 또는 N의 제곱근까지 나누어지는 수를 확인하는 소수 판별법과 달리 Miller-Rabis`s algorithm을 기반으로 확률적으로 소수를 판별하는 함수라고 한다. 이때 인자로 들어가는 certainty를 이용해 해당 수가 소수일 확률이 1-(1/2)^certainty을 넘으면 true 아니면 false를 반환한다.


