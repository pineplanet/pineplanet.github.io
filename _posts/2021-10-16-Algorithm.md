---
title: 2021-10-16 Programmers.42839 소수찾기 문제 풀이 
tags: [Python,Algorithm,완전탐색]
categories: Algorithm
---
# 문제 
오늘은 [소수찾기](https://programmers.co.kr/learn/courses/30/lessons/42839) 문제를 풀어보기 했어요 


# 문제 설명
한자리 숫자가 적힌 종이 조각이 흩어져있습니다. 흩어진 종이 조각을 붙여 소수를 몇 개 만들 수 있는지 알아내려 합니다.

각 종이 조각에 적힌 숫자가 적힌 문자열 numbers가 주어졌을 때, 종이 조각으로 만들 수 있는 소수가 몇 개인지 return 하도록 solution 함수를 완성해주세요.

"17" 을 입력 받으면 숫자 1, 7, 17, 71 을 만들 수 가 있고, 이중에서 소수는 7,17,71입니다. 
이 숫자들의 개수를 리턴하면 되는 문제입니다.

# 문제 풀이

문제를 풀기 전에 쉽게 소수를 판별할 방법이 있는지, 소수의 특성을 찾아봅시다. 

소수는 1과 자기 자신 외의 숫자로는 나누어 지지 않는 수인데, 2 부터 우리가 판달할 숫자의 제곱근 까지의 숫자까지만 나누어서 나머지 값이 0이면 소수가 아닌걸로 판단하면 되는군요!.. 

그렇다면, 
1. 입력받은 문자열을 조합해서 가능한 모든 숫자를 만들고, 0부터 시작하는 숫자는 제거해서 집합을 만들고요. 

2. 이 중에서 소수인 수를 찾아서 따로 저장을 해둘거에요. 
    1. 2부터 해당 수의 제곱근까지의 수를 나누어서 나머지가 0이 아닌 경우 카운트를 하고,
    2. 모든 수를 다 나누었는데 카운트가 한번도 되지 않는 수를 소수 집합에 저장합니다.
3. 소수 집합의 길이를 리턴 합니다.  

# 코드 
```python
import itertools
import math
from itertools import product

def solution(numbers):
    answer = 0
    newNumber = []
    for i in range(len(numbers)):
        for j in itertools.permutations(numbers,i+1):
            if j[0] != '0':
                str_num = "".join(j)
                if str_num != '1':
                    newNumber.append(str_num)
    newNumber = list(set(newNumber))
    answers = []
    for n in newNumber:
        num = int(n)
        nSqrt = math.sqrt(num)
        x = 2
        cnt = 0
        while x <= nSqrt:
            if (num % x == 0):
                cnt += 1

            x += 1
        if num % nSqrt == 0:
            cnt += 1
        if cnt == 0:
            answers.append(num)


    answer = len(answers)
    print(answers)
    return answer
print(solution("17"))


```