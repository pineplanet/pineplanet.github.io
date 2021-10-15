---
title: 2021-10-14 Programmers.42840 모의고사 문제 풀이 
tags: [Python,Algorithm,완전탐색]
categories: Algorithm
---
이번주에는 완전 탐색과 이분 탐색을 배웠습니다. 
그리고 오늘은 아래의 문제를 풀어볼꺼에요. 

https://programmers.co.kr/learn/courses/30/lessons/42840

문제는 다음과 같습니다. 

수포자는 수학을 포기한 사람의 준말입니다. 수포자 삼인방은 모의고사에 수학 문제를 전부 찍으려 합니다. 수포자는 1번 문제부터 마지막 문제까지 다음과 같이 찍습니다.

1번 수포자가 찍는 방식: 1, 2, 3, 4, 5, 1, 2, 3, 4, 5, ...


2번 수포자가 찍는 방식: 2, 1, 2, 3, 2, 4, 2, 5, 2, 1, 2, 3, 2, 4, 2, 5, ...


3번 수포자가 찍는 방식: 3, 3, 1, 1, 2, 2, 4, 4, 5, 5, 3, 3, 1, 1, 2, 2, 4, 4, 5, 5, ...

1번 문제부터 마지막 문제까지의 정답이 순서대로 들은 배열 answers가 주어졌을 때, 가장 많은 문제를 맞힌 사람이 누구인지 배열에 담아 return 하도록 solution 함수를 작성해주세요.

1번 수포자는 1, 2, 3, 4, 5 를 반복 해서 찍고,

2번 수포자는 2, 1, 2, 3, 2,/ 4, 2, 5를 반복,

3번 수포자는 3, 3, 1, 1, 2,/ 2, 4, 4, 5, 5를 반복해서 찍습니다. 


[1,2,3,4,5]가 답 인 경우, 1번 수포자는 모든 문제를 맞추고, 2번 수포자와 3번 수포자는 모두 틀리네요. 

[1,3,2,4,2]가 답인 경우, 모든 수포자가 2개씩 맞추니 공동 1등이에요. 

그렇다면, 일단 주어지는 답 갯수로 각 수포자의 답안을 만들어보겠습니다. 

그리고 답과 모든 수포자들의 답을 인덱스를 이용해서 하나하나 비교 한 다음, 맞춘 정답을 카운트 해서 저장하고 

여기서 제일 높은 값을 찾아서 저장한 뒤 

가장 많이 맞춘 수포자를 찾아낼 거에요. 

아래와 같이 코드를 짰는데 무사히 통과 했습니다! 

```python
def solution(answers):
    answer = []
    p1Pattern = [1, 2, 3, 4, 5]
    p2Pattern = [2, 1, 2, 3, 2, 4, 2, 5]
    p3Pattern = [3, 3, 1, 1, 2, 2, 4, 4, 5, 5]

    sheet = []
    answersLength = len(answers)
    for i in [p1Pattern, p2Pattern, p3Pattern]:
        if answersLength > len(i):
            mul = round(len(answers) / len(i)) + 1
            sheet.append(i * mul)
        else:
            sheet.append(i)

    count = {0: 0, 1: 0, 2: 0}
    for i in range(answersLength):
        for j in range(len(sheet)):
            if sheet[j][i] == answers[i]:
                count[j] += 1

    Max = (max(count.values()))

    for i in count.keys():

        if count[i] == Max:
            answer.append(i+1)
    print (answer)
    return answer
```

