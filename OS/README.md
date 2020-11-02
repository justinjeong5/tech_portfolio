# OPERATING SYSTEM

## Process Synchronization

process는 병행하게 또는 병렬적으로 실행될 수 있다. process scheduling에는 여러가지 방식이 알려져 있고 cpu scheduler가 process 사이에서 빠르게 오가며 각 프로세스를 실행하여 모든 프로세스를 병행 실행시킨다. 이는 한 프로세스는 다른 프로세스가 스케줄되기 전에 일부분만 진행할 수 있다는 것을 의미한다. 이렇게 **프로세스가 병행 또는 병렬로 실행** 될 때 여러 프로세스가 공유하는 **데이터의 무결성(integrity)에 문제** 가 발생할 수 있다.

생산자-소비자 문제(consumer-producer problem)의 유한버퍼 문제를 통해 프로세스 동기화에서 발생하는 문제를 떠올려보자. 생산자 소비자 알고리즘에서 0으로 초기화 되어있는 counter변수를 추가하자. 생산자와 소비자는 버퍼에 새 항목을 추가시킬때 마다 counter값을 증가시키고 버퍼에서 한 항목을 꺼낼 때마다 counter값을 감소시킨다. 

```c++
//  producer
while(true){
    /* produce an item in nextProduced */
    while(counter == BUFFER_SIZE){
      ; // do nothing
    }
    buffer[in] = nextProduced;
    in = (in + 1) % BUFFER_SIZE;
    counter++;
}
```

```c++
//  consumer
while(true){
    while(counter == 0){
      ; // do nothing
    }
    nextConsumed = buffer[out];
    out = (out + 1) % BUFFER_SIZE;
    counter--;
    /* consume the item in nextConsumed */
}
```

위에 보인 **생산자와 소비자 코드는** 각각 옳게 동작하더라도, 병행적으로 수행시키면 **동작하지 않는다.** 예를 들어 counter의 현재 값은 5이고 생잔사와 소비자가 'counter++'와 'counter--'를 병행하게 실행하면 counter의 값은 4, 5, 6 중에 하나가 될 것이다. 이중 유일한 올바른 결과는 counter == 5인 상황이고 생산자와 소비자의 실행이 분리 되었을 때 가능한 결과이다. 

문장 'counter++'는 다음과 같은 assembly language로 구현될 수 있다.

  register~1~ = counter
  register~1~ = register~1~ + 1
  counter = register~1~


소비자와 생산자가 'counter++'와 'counter--'를 병행하게 실행하면 아래와 같은 assembly language에 의해 연산이 수행된다

1. 생산자가 register~1~ = counter를 수행(register~1~ = 5)
2. 생산자가 register~1~ = register~1~ + 1을 수행(register~1~ = 6)
3. 소비자가 register~2~ = counter를 수행(register~2~ = 5)
4. 소비자가 register~2~ = register~2~ - 1을 수행(register~2~ = 4)
5. 생산자가 counter = register~1~을 수행(counter = 6)
6. 소비자가 counter = register~2~을 수행(counter = 4)


실제로는 버퍼에 5개가 채워져 있지만 4개의 버퍼가 채워져 있는 것을 의미하는 counter = 4인 부정확한 상태가 된다. 혹은 counter = 6인 부정확한 상태가 된다. 이런 부정확한 상황이 발생하는 이유는 여러개의 프로세스가 **동일한 자료에 접근하여 조작** 하고 그 실행 결과가 접근이 발생한 **특정 순서에 의존** 하는 **Race Condition**이 발생 했기 떄문이다.

### Critical Section Problem

**임계구역(critical section)** 은 다른 프로세스와 공유하는 변수를 변경하거나, 테이블을 갱신하거나 파일을 쓰거나 하는 동작을 수행하는 코드 부분을 말한다. 임계구역 문제를 해결하기 위해서는 다음 3가지 조건을 충족해야한다.

1. 상호베제(Mutual Exclusion): 어느 한 프로세스가 임계구역에서 실행되면, 다른 프로세스들은 실행될 수 없다.
2. 진행(progress): 임계구역에서 실행중인 프로세스가 없고 임계구역으로 진입하려고 하는 프로세스가 있다면 일을 하지 않는 프로세스만 임계구역에 진입할 순서를 정할 수 있고, 이 선택은 무기한 연기될 수 없다.
3. 한정된 대기(bounded waiting): 프로세스가 임계구역에 진입하려는 요청을 한 후부터 그 요청이 허용될 때까지 다른 프로세스들이 임계구역에 진입하도록 허용되는 횟수에 한계가 있어야 한다.


### Mutex Locks
**임계구역 문제(Critical Section Problem)** 을 해결하는 가장 간단한 도구가 **Mutex(Mutual Exclusion) Lock** 이다. 임계구역을 보호하고 경쟁 조건(Race Condition)을 방지하기 위해 Mutex Lock를 활용한다. 프로세스는 임계구역에 들어가기 전에 반드시 lock을 획득하고 임계구역을 빠져나올 때 Lock을 반환한다. 


```c++
do {
  // lock를 획득
  acquire( );
  // critical section
  // lock를 반환
  release( );
  // 나머지 구역 

} while(true)

acquire( ) {
  while(!available){
    ; /* busy waiting */
  }
  // lock를 획득
  available = false;
}

release( ) {
  // lock를 반환
  available = true;
}

```

**mutex lock** 의 **단점은 바쁜 대기(busy waiting)** 을 해야 한다는 것이다. 프로세스가 임계구역에 들어가 있는 동안 다른 프로세스들은 acquire( )함수를 호출하는 반복문을 계속 실행해야 한다. 이는 **spinlock** 이라고 부른다. 
지속적인 반복은 많은 프로세스들이 CPU를 공유하는 실제 다중 프로그래밍 시스템에서는 분명한 문제이다. 바쁜대기는 프로세스가 더 생산적인 작업에 사용할 수 있었던 **CPU 사이클을 낭비** 한다. 하지만 락을 기다리는 동안 소모하는 **문맥 교환(Context Swicthing)을 전혀 필요로 하지 않는 것** 이 spinlock의 장점이다. 따라서 짧은 시간 동안만 락을 소유한다는 것이 에상되면 spinlock는 매우 유용하다.


### Semaphore
Semaphore는 mutex와 비슷하게 동작하지만 행동을 조금 더 정교하게 동기화하는 방법을 제공한다. **세마포(Semaphore)** 는 정수 변수로 초기화를 제외하고는 단지 두개의 표준 atomic한 연산인 wait( )와 signal( )로만 접근이 가능하다. wait( )와 signal( )의 정의는 아래와 같다.
```c++
wait(S) {
  while(S <= 0>){
    ; // busy waiting
  }
  S--;
}

signal(S){
  S++;
}
```

wait( )와 signal( ) 연산시 세마포의 정수 값을 변경하는 연산은 반드시 분리되지 않고 수행되어야 한다. 한 스레드가 세마포 값을 변경하면, 다른 어떤 스레드도 동시에 동일한 세마포 값을 변경할 수 없다. 부가하여, wait(S)의 경우, S의 정수 값을 검사하는 작업(S <= 0)과 그에따라 실행될 수 있는 변경(S--)햐는 작업 또한 인터럽트 되지 않고 실행되어야 한다.

운영체제는 종종 **counting semaphore** 와 **binary semaphore** 를 구분한다. **카운팅 세마포** 의 값은 제한 없는 영역(domain)을 갖는다. **이진 세마포** 의 값은 0또는 1의 값만 가능하다. 따라서 이진 세마포는 Mutex lock과 유사하게 동작한다. 

**카운팅 세마포(counting semaphore)** 는 유한한 개수를 가진 자원에 대한 접근을 제어하는데 사용될 수 있다. 세마포는 가용한 자원의 개수로 초기화된다. 각 **자원을 사용** 하려는 프로세스는 세마포에 **wait( )** 연산을 수행하며, 이때 **세마포의 값은 감소**  된다. 프로세스가 **자원을 방출** 할 때는 **signal( )** 연산을 수행하고 **세마포의 값은 증가** 하게 된다. **세마포의 값이 0** 이 되면 **모든 자원이 사용중** 임을 나타낸다. 이후 자원을 사용하려는 프로세스는 세마포 값이 0보다 커질때까지 봉쇄된다.

mutex lock은 **바쁜 대기(busy waiting)** 을 해야한다는 점에서 문제가 되기도 했다. 위에서 설명한 wait( )와 signal( )또한 busy waiting이 발생하고 cpu 사이클을 낭비하기 때문에 바람직하지 않다. busy waiting이 일어나지 않도록 하기 위해서 **blocking** 연산을 활용한다. 

wait( )와 signal( )은 다음과 같이 변경할 수 있다. wait( ) 연산을 실행하고 세마포 값이 양수가 아닌것을 발견하면 프로세스는 반드시 대기해야한다. 이때 바쁜 대기 대신에 프로세스는 자신을 **봉쇄(blocking)** 시킬 수 있다. blocking연산은 프로세스를 세마포에 연관된 **대기 큐(queue)** 에 넣고 프로세스의 상태를 대기 상태로 전환한다. 그 후에 **제어(control)** 이 CPU 스케줄로 넘어가고, 스케줄러는 다른 프로세스를 실행하기 위해서 선택한다.
세마포 S를 대기하면서 봉쇄된 프로세스는 다른 프로세스가 **signal( )** 연산을 실행하면 재시작되어야 한다. 프로세스는 **wakeup( )** 연산에 의해 재시작되는데 이것은 프로세스의 상태를 대기상태에서 **준비완료 상태** 로 변경한다. 그리고 프로세스는 **준비 완료 큐(queue)** 에 넣어진다. 

```c++
typedef struct {
    int value;
    struct process *list;
} semaphore;
```
세마포는 한개의 정수 valuem와 프로세스 리스트 list를 가진다. 프로세스가 세마포를 기다려야 한다면, 이 프로세스를 세마포의 프로세스 리스트에 추가한다. signal( ) 연산은 프로세스 리스트에서 한 프로세스를 꺼내서 그 프로세스를 꺠운다(wakeup)

```c++
void wait(semaphore *S) {
    S->value--;
    if(S->value < 0){
        이 프로세스를 S->list에 넣는다;
        block( );
    }
}

void signal(semaphore *S) {
    S->value++;
    if(S->value <= 0) {
        S->list로부터 하나의 프로세스 P를 꺼낸다;
        wakeup(P);
    }
}
```

**block( )** 연산은 자기를 호출한 프로세스를 중지시킨다. **wakeup(P)** 연산은 봉쇄된 프로세스 P의 실행을 재개시킨다. 이들 두 연산들은 운영체제의 기본적인 **시스템 호출로 제공** 된다.
바쁜 대기를 하는 세마포의 고전적인 정의에서는 세마포의 값은 음수를 가질 수 없으나, 이와 같이 구현하면 세마포가 음수를 갖을 수 있게 된다. 세마포 값이 음수일 때 그 **절대 값** 은 세마포를 **대기하고 있는 프로세스의 수** 이다. 음수가 나오며 그 값이 대기하는 프로세스의 숫자가 될 수 있는 것은 wait( ) 연산에서 block( ) 상태에 넣기전에 S->value의 값을 줄이는 결과이다. 

세마포가 **원자적(atomic)** 으로 실행되어야 하는 것은 매우 중요하다. 같은 프로세스에 대해 두 프로세스가 동시에 wait( )와 signal( )연산을 실행할 수 없도록 보장해야 한다. 이런 상황은 **임계구역 문제(critical section problem)** 에 해당한다. 단일 처리기(single CPU) 환경에서는 단순히 wait( )와 signal( )이 실행되는 동안 인터럽트를 금지시킴으로써 간단하게 해결 할 수 있다. 이 방법은 인터럽트가 금지되면, 다른 프로세스들의 명령어들이 끼어 들 수 없기 때문에 단일 처리기 환경에서는 올바르게 동작한다. 

하지만 다중 처리기 환경에서는 모든 처리기에서 인터럽트를 금지해야한다. 그렇지 않으면 다른 처리기에서 실행되는 프로세스들의 명령어들이 임계 구역 문제를 발생시킬 수 있다. 모든 처리기에서 인터럽트를 금지시키는 것은 매우 어려운 작업일 수 있고, 더욱이 성능을 심각하게 감소시킨다는 어려움이 있다. 

### deadlock and starvation

대기 큐를 가진 세마포의 구현은 두 개 이상의 프로세스들이, 오로지 대기 중인 프로세스들 중 하나에 의해서만 야기될 수 있는 사건을 무한정 기다리는 상황이 발생할 수 있다. 이런 사건을 **교착 상태(deadlock)** 라고 한다. 

두 개의 프로세스 p~0~과 p~1~로 구성되고, 이들이 1로 지정된 세마포 S와 Q를 접근하는 시스템을 생각해보자. 

p~0~ : wait(S); wait(Q); ...  wait(S); wait(Q);
p~1~ : wait(Q); wait(S); ...  wait(Q); wait(S);  

p~0~이 wait(S)를 싱핼하고, p~1~이 wait(Q)를 실행한다고 가정하자. p~0~이 wait(Q)를 실행할 때, p~0~은 p~1~이 signal(Q)를 실행할 떄까지 기다려야 한다. 마찬자기로  p~1~이 wait(S)를 실행할 때, p~1~은 p~0~이 signal(S)를 실행할 떄까지 기다려야 한다. 이들 시그널 연산은 실핼될 수 없기 떄문에 p~0~과 p~1~은 교착상태가 된다. 한 집합 내의 **모든 프로세스들이** 그 집합 내의 다른 프로세스들이 유발할 수 있는 **사건을 기다릴 때,** 이 프로세스들이 **교착상태** 에 있다고 말한다. 