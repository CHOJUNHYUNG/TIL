# 객체
객체 = 데이터(변수, 속성) + 함수(메서드)

## 객체 선언
``` python
class class_():
	def __init__(self, x):
		self.x = x
	def exclaim(self):
		print("I'm class_")

c = class_('creat_class')
>>> c.x
creat_class
```
self는 객체 자신을 가리킨다. 객체의 속성 x에 접근하기 위해서는 self.x로 접근할 수 있다. 
`__init__` 메서드는 같은 클래스에서 생성된 다른 객체를 구분하기 위해 필요한 뭔가를 수행한다. 따라서 객체 선언시 항상 필요한 것은 아니다. 

## 상속
상속은 기존 클래스에서 일부를 추가하서나 변경하여 새로운 클래스를 생성할 수 있게 해준다.
이는 기존 클래스의 행동을 `오버라이드(재정의)`한다. 기존의 클래스는 **부모 클래스, 슈퍼 클래스, 베이스 클래스**라고 부르며, 새 클래스는 **자식 클래스, 서브 클래스, 파생된 클래스**라고 부른다.

``` python
class class_2(class_):
  def __init__(self, x): # 오버라이드
		self.x = x
	def exclaim(self): # 오버라이드
		print("I'm class_2")
	def new_func(self): # 새로운 메서드 추가
		print("Add new method!")

c2 = class_2('creat_class2')
>>> c2.x
creat_class2
>>> c2.new_func
Add new method!
```

## Super
super는 자식 클래스에서 부모 클래스의 메서드를 호출할 수 있도록 해준다.
``` python
class class_3(class_):
	def __init__(self, x, y):
		super().__init__(x)
		self.y = y
```
물론,
``` python
class class_3(class_):
	def __init__(self, x, y):
		self.x = x
		self.y = y
```
다음과 같이 정의할 수도 있다. 하지만 이러한 정의는 상속을 사용할 수 없다. class_3의 `__init__`은 `class_`의 `__init__`과 독립된 자신만의 메서드이다. 
super() 메서드는 부모로부터 지속적인 상속이 가능하게 한다. 즉, `class_`의 속성/메서드가 변경되면 `class_3`에도 그 변경사항이 적용된다.

## get / set 속성값과 프로퍼티
어떤 객체 지향 언어들은 외부에서 바로 접근이 불가능한 private 객체 속성을 지원한다. 이러한 속성에 접근하기 위해서는 getter와 setter 메서드를 사용한다.
파이썬의 모든 속성과 메서드는 기본적으로 public이다. private하게 작성하고 싶다면 getter, setter메서드를 작성하면 되고 이를 위해서 파이썬에서는 프로퍼티를 사용하면 된다.
### property( )
``` python
class Duck():
	def __init__(self, input_name):
		self.hidden_name = input_name
	def get_name(self):
		print('inside the getter')
		return self.hidden_name
	def set_name(self, input_name):
		print('inside the setter')
		self.hidden_name = input_name
	name = property(get_name, set_name)
# get
fowl = Duck('Howard')
>>> fowl.name
inside the getter
'Howard'
# set
>>> fowl.name = 'Daffy'
inside the setter
>>> fowl.name
inside the getter
'Daffy'
```
### 데커레이터
``` python
class Duck():
	def __init__(self, input_name):
		self.hidden_name = input_name
	@property  # @property = getter 메서드
	def name(self):
		print('inside the getter')
		return self.hidden_name
	@name.setter # @메서드.setter = setter 메서드
	def name(self, input_name):
		print('inside the setter')
		self.hidden_name = input_name
```
물론 get_name()과 set_name() 메서드를 직접 호출할 수 있다.  만약 hidden_name 속석을 알고 있다면 직접 읽고 쓸 수 있다. 이를 방지하기 위해 **네임 맹글링**을 할 수 있다.

프로퍼티는 계산 된 값을 참조할 수 있다.
``` python
class Circle():
	def __init__(self, radius):
		self.radius = radius
	
	@property
	def diameter(self):
		return 2 * self.radius
		
c = Circle(5)
>>> c.radius
5
>>> c.diameter
10
```
위 Circle 클래스처럼 프로퍼티는 속성처럼 참조가 가능하다. 
또한 setter가 없다면 외부에서 속성으로 접근이 불가능하다. 즉, 이 경우는 읽기 전용 속성이다. 

프로퍼티를 통해 속성에 접근하면 속성의 정의를 변경해야할 때 모든 호출자를 수정할 필요 없이 해당되는 정의만 수정하면 된다는 이점이 있다.

### private 네임 맹글링
``` python
class Duck():
	def __init__(self, input_name):
		self.__name = input_name
	@property  # @property = getter 메서드
	def name(self):
		print('inside the getter')
		return self.__name
	@name.setter # @메서드.setter = setter 메서드
	def name(self, input_name):
		print('inside the setter')
		self.__name = input_name
```
이 네이밍 컨벤션은 속성을 private로 만들어주는 것은 아니다. 하지만 의도치 않게 외부에서 접근하는 것을 방지해준다. 의도적인 접근을 위해서는 아래와 같은 접근이 필요하다.
``` python
>>> fowl._Duck__name
'Daffy'
```

## 메서드 타입
* 인스턴스 메서드 : 인스턴스에 따라 다르게 동작하는 메서드. 정의 시  첫 인자로 self를 붙임.

* 클래스 메서드 : 클래스에 직접 접근하는 메서드. 정의 시 `@classmetho`를 붙이고 클래스를 인자로 줌.
이 클래스의 매개변수로 보통 cls를 사용한다.
``` python
class A():
	count = 0
	def __init__(self):
		A.count += 1
	@classmethod
	def kids(cls):
		print('A has', cls.count, 'little objects.')
    
a1 = A()
a2 = A()
a3 = A()
>>> A.kids()
A has 3 little objects.
```
* 정적 메서드 :  `@staticmethod`를 붙이고 별도의 인자는 없음.
``` python
class CoyoteWeapon():
	@staticmethod
	def commercial():
		print('This CoyoteWeapon has been brought to you by Acme')
>>> CoyoteWeapon.commercial()
This CoyoteWeapon has been brought to you by Acme
```

## 특수 메서드

## 컴포지션
``` python
class class_a():
	def __init__(self, description):
		self.description = description
class class_b():
		def __init__(self, description):
		self.description = description

class Class_():
	def __init__(self, comp_a, comp_b):
		self.a = comp_a
		self.b = comp_b
	def about(self):
		print("This class have two compositions:", self.a.description, self.b.description)
```

# 네임드 튜플
튜플의 서브클래스. 이름(.name)과 위치([offset])으로 값에 접근.
``` python
from collections import namedtuple
Duck = namedtuple('Duck','bill tail')
duck = Duck('wide orange', 'long')

>>> duck
Duck(bill='wide orange', tail='long')
>>> duck.bill
'wide orange'
>>> duck.tail
long
```
딕셔너리를 네임드 튜플로 변경할 수 있다.
``` python
parts = {'bill':'wide orange', 'tail':'long'}
duck2 = Duck(**parts)

>>> duck2
Duck(bill='wide orange', tail='long')
```
네임드 튜플은 불변한다. 하지만 필드를 바꿔 새로운 네임드 튜플을 만들 수 있다.
``` python
duck3 = duck2._replace(bill='crushing',tail='magnificent')

>>> duck3
Duck(bill='crushing', tail='magnificent')
```






