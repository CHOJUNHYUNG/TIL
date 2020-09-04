컴프리헨션(함축)은 하나 이상의 이터레이터로부터 파이썬 자료구조를 만드는 콤팩트한 방법.

# 리스트 컴프리헨션
``` python
# [표현식 for 항목 in 객체]
[x for x in range(3)]
>>> [0, 1, 2]

# [표현식 for 항목 in 객체 if 조건]
[x for x in range(6) if x % 2 == 0]
>>> [0, 2, 4]

# [표현식 for 항목 in 객체 for 항목 in 객체]
[(x,y) for x in range(3) for y in range(2)]
>>> [(0,0),(0,1),(1,0),(1,1),(2,0),(2,1)]
```

# 딕셔너리 컴프리헨션
``` python
# {key 표현식 : value 표현식 for 표현식 in 객체}
word = 'letters'
{letter: word.count(letter) for letter in word}
>>> {'l':1, 'e':2, 't:2', 'r':1, 's':1}

# if, 다중 for문 가능
```

# 셋 컴프리헨션
``` python
# {표현식 for 표현식 in 객체}
{number for number in range(1,6) if number%3 == 1}
>>> {1,4}
```

# 제너레이터 컴프리헨션
튜플은 컴프리헨션이 없다. 튜플 형식의 컴프리헨션은 제너레이터 객체를 반환한다.
``` python
number_thing = (number for number in range(1,6))
number_thing
>>> <class 'generator'>

list(number_thing)
>>> [1, 2, 3, 4, 5]
```
