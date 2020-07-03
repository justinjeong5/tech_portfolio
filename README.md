# tech_portfolio

컴공 주요 5과목에 대한 주요 개념 정리  

- [tech_portfolio](#tech_portfolio)
- [DATABASE](#database)
  - [Transaction](#transaction)
    - [ACID](#acid)
      - [Atomicity](#atomicity)
      - [Consistency](#consistency)
      - [Isolation](#isolation)
      - [Durability - Persistency](#durability---persistency)


# DATABASE

## Transaction

transaction은 한 단위로 간주되어야 하는 작업의 단위를 말한다. 주로 은행거래의 예시를 들어 설명하는데, 만약 고객1이 고객2에게 100만원을 전송하려는 시도를 한다고 가정하자. 고객1의 계좌에서 잔액이 100만원이 줄어들면서 고객2의 계좌에서는 잔액이 100만원이 올라야한다. 어떤 특정한 이유에서 고객1의 잔액을 줄었지만 고객2의 잔고가 늘지 않는다면 이는 큰 문제가 된다. 이는 마치 고객1에서 빠져나간 돈이 아무에게도 전해지지 않고 사라지는 것과 같은 상황이다. 위 예시보다 합리적이라면 실제로 100만원이 이체되거나 혹은 아무일도 일어나지 않아야 한다. 정보를 생성하고 참조하며 수정하거나 지우는 역할을 수행하는 Database에서도 이와 동일한 이슈가 있다. 

### ACID

#### Atomicity
atomicity는 여러가지 논리적으로 의미있는 단위의 일을 하나로 묶어서 반드시 한번에 일어나거나 전혀 일어나지 않아야 하는 일의 속성을 말한다. 어떤 행위에 atomicity가 있다고 하면 더이상 쪼개어질수 없다는 것과 같은 의미이다. 이는 database뿐 아니라 operating system의 critical section에서도 적용되는 개념이다. atomicity가 보장되려면 다음과 같은 일이 하나의 작업으로 보장되어야 한다.
1. 고객1이 10만원을 출금하려고 한다.
2. database에서 잔고를 확인하고 10만원을 뺸다
3. 고객1에게 10만원을 지급한다.

위 모든 과정이 마치 하나의 일로 발생해야하고, 중간에 어떤 특정한 이유로 모두 마무리 지을 수 없다면 아주 없던일로 해야한다. 

#### Consistency
data는 constraint와 rule이 있어서 의미가 발생한다. 통장계좌는 항상 0와 같거나 큰 정수라고 정해져 있어야만 있지도 않은 돈을 주는 일을 막을 수 있다. 따라서 transaction간에 해당 data가 rule과 constraint를 지키도록 보장하는 것을 말한다. 만약 고객1이 있지도 않은 금액을 출금하려고 한다면 해당 trasaction은 중단되어야햔다.
1. 고객1의 잔고는 100만원이다.
2. 고객1은 200만원을 출금하려고 시도한다.
3. 2번은 1번에대한 consistancy를 위반한다.
4. 해당 transaction은 중단된다


#### Isolation
isolation은 transaction의 수행이 다른 transaction의 영향을 받지 않아야 한다는 것을 의미한다. 만약 고객1과 고객2가 동일한 계좌에서 금액을 출금하려고 한다고 하자. 계좌에는 100만원이 들어있고 고객1과 고객2는 각각 10만원, 25만원을 출금하려는 상황이다. 다음과 같은 일이 순서대로 일어났다고 가정하자. 
1. 고객1이 10만원을 출금한다.
2. 계좌에서 10만원이 줄어들고 이 내용이 database에 반영되려고 한다.
3. 2의 내용이 반영되기 전에 고객2가 25만원을 출금하려 시도했다.
4. 고객2가 25만월 지급받았다.
5. 고객2가 출금한 25만큼 database에서 잔액이 줄어들었다.
6. 2에서 반영된 내용이 이제서야 database에 반영된다.
7. 고객1과 고객2가 모두 출금하였고 은행 잔고는 90만원이다.

고객1이 10만원을 출금하는 transaction이 isolation을 보장받지 못한다면 고객2가 25만원을 출금하더라도 잔액이 여전히 90만원일 수 있다. 이런 일이 일어나지 않도록 보장하는 속성이 ioslation이다. 

#### Durability - Persistency

