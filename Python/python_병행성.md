[참고](https://hamait.tistory.com/752?category=79136)
# 병행성
다음 두 용어는 병행성과 관련 있다.
* 동기 - 한 작업은 다른 작업을 따름
* 비동기 - 각 작업들은 독립적

multiprocessing등을 통해 병행성을 확보해주어도 병목 현상 등으로 병렬처리의 복잡성이 증가하고 성능저하 혹은 작업 실패로 이어질 수 있다. 이러한 복잡성을 해결하는 한 가지가 Queue이다.

## 스레드
``` python
import threading
```
스레드는 한 프로세스 내에서 실행된다.
### deamon
파이썬에 스레드는 기본적으로 deamon이 False이다.
daemon = False : 메인 스레드가 종료되어도 자신의 작업이 끝날때까지 지속된다.
daemon = True : 메인 스레드가 종료되면 즉시 종료됨.
daemon 속성은 반드시 start 전에 호출되어야 함.

**Threading 모듈의 객체**
* Thread : 단일 실행 스레드 객체
* Lock : 기본 Lock 객체
* RLcok : 재진입 Lock 객체
* Condition : 다른 스레드의 신호를 기다릴 수 있는 컨디션 변수 객체
* Event : 컨디션 변수 객체의 일반화
* Semaphore : 정해진 갯수만큼 스레드를 혀옹하는 동기화 객체
* BoundedSemaphore
* Timer : 실행전 정해진 시간만큼 대기하는 스레드
* Barrier : 정해진 수의 스레드가 해당 지점에 도달해야 스레드 지속

**Thread 클래스**

* daemon
* `__init__`
* start
* run
* jon



**스레드 생성하기**




