# 조건문

* if, else
``` python
if True:
	print('True')
  
else:
	print('False')
```

* elif
``` python
color = 'puce'
if color == 'red':
  print("it's a tomato")
  
elif  color == 'green':
  print("it's a green papper")
  
 else:
  print("I've never heard of the color", color)
```
* 비교 연산자
``` python
== : 같다
!= : 다르다
< : 작다 / > : 크다
<= : 작거나 같다 / >= : 크거나 같다
in ... : 멤버십
```
* 부울 연산자 : and , or , not
  * and : 두 조건 모두가 참이면 True
  * or : 두 조건중 하나라도 참이면 True
``` python
x = 7

(5 < x) and (x < 10)
>>> True

(5 < x) or ( x < 10)
>>> True

(5 < x) and not (x > 10)
>>> True

5 < x < 10 # 파이썬에서는 한 번에 여러번 비교할 수 있다.
>>> True
```



# While문
: 조건이 참인 동안 반복하여 수행
* break : 조건을 만족할때 while문 종료.
* continue : 조건을 만족하면 while문 건너띔.
``` python
while True:
  value = input("Integer, please [q to quit] : ")
  if value == 'q':
    break
   number = int(value)
  if number % 2 == 0:
    continue
   print(number)
```
* else : while문을 모두 실행한 경우 break가 실행되지 않았다면 else문 실행
``` python
numbers = [1, 3, 5]
position = 0
while position < len(numbers):
  number = numbers[position]
  if number % 2 == 0:
    print('Found even number', number)
    break
   position += 1
 else:
  print('No even number found')
```

# for 문
* 시퀀스 순회
``` python
word = 'cat'
for w in word:
  print(w,end=" ")
>>> c a t

# dictionary 순회
dic = {'key':'value','room':'ballroom'}
for key in dic:
  print(key, end=" ")
>>> key room

for value in dic.values():
  print(value, end=" ")
>>> value vallroom

for item in dic.items():
  print(item end=" ")
>>> ('key', 'value') ('room', 'ballrom')
```

* break, continue : while문과 똑같이 동장.
* else : 모든 항목을 순회했는지 확인.. break에 의한 중단여부 판단!
``` python
numbers = [1,3,5]
for num in numbers:
  if num % 2 == 0:
    print('Found even number', num)
    break
else:
  print('No even number found')
>>> No even number found
```

## 여러 시퀀스 순회 : zip( )
* 병렬 순회
가장 짧은 시퀀스 기준으로 순회.
``` python
days = ['Monday','Tuesday','Wednesday']
drinks = ['coffe','tea','beer']
desserts = ['tiramisu','ice cream','pie','pudding']

for day, drink, dessert in zip(days, drinks, desserts):
	print(day,': drink',drink,'- enjoy',dessert)
>>> Monday : drink coffe - enjoy tiramisu
Tuesday : drink tea - enjoy ice cream
Wednesday : drink beer - enjoy pie
```
* 튜플/딕셔너리 생성
``` python
english = 'Monday', 'Tuesday', 'Wednesday'
french = 'Lundi', 'Mardi', 'Mercredi'

list( zip(english, french) )
>>> [('Monday','Lundi'), ('Tuesday','Mardi'), ('Wednesday','Mercredi')]

dict( zip(english, french) )
>>> {'Monday':'Lundi', 'Tuesday':'Mardi', 'Wednesday':'Mercredi'}
```

## 숫자 시퀀스 생성 : range( )

특정 범위의 숫사 스트림을 반환.
순회 가능한 객체 변환.
``` python
# range( start , end , step)
for x in range(0,3):
  print(x)
>>> 0 1 2

list( range(0,6,2) )
>>> [0, 2, 4]

list( range(2,-1,-1) )
>>> [2, 1, 0]
```