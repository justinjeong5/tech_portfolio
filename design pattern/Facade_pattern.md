## Facade pattern

오늘도 카페에 가서 아이스 아메리카노를 주문합니다. 점원에게 아이스 아메리카노 한잔을 주문하고 카드로 결제를 하면 주문은 간단하게 끝납니다. 커피의 재고가 있는지, 테이크아웃 용기는 충분한지는 중요한 일이 아닙니다. 점원에게 재고를 파악하고 커피를 만들수 있는 방법이 있기 때문에 고객의 입장에서 주문은 간단합니다. 이와 같은 패턴이 소프트웨어어도 적용됩니다. 복잡하고 내부적인 내용은 감추고 간단한 겉모습만 제공하는 design pattern을 말합니다. 

<img src="https://user-images.githubusercontent.com/44011462/90099244-f84d9400-dd74-11ea-8e00-a9d9b20924a0.png" width="400px">  

*Facade는 건물의 외관을 뜻하는 프랑스어로, 복잡한 내부가 아닌, 세련된 외관을 중요시한 design pattern을 Facade pattern이라고 한다.*


```javascript
// order.js

class Order{
    constructor(){
        this.coffeeOrderArr = [];
        this.coffeeStock = 10;
        this.container = 5;
    }

    validCoffeeStock(){
        return this.coffeeStock > 0;
    }

    vaildContainer(){
        return this.container > 0;
    }

    makeCoffee(){
        if(validCoffeeStock() && vaildContainer()){
            this.coffeeOrderArr.shift();
            this.coffeeStock--;
            this.container--;
            return true;
        }
        return false;
    }

    orderCoffee(item){
        this.coffeeOrderArr.push(item);
        return makeCoffee();
    }   
}
```

고객의 입장에서는 커피의 재고가 있는지, 테이크아웃 용기는 충분한지 고민할 필요가 없습니다. 그저 커피를 달라고 요청하면 복잡하고 세부적인 절차가 수행됩니다. 
```javascript
// main.js
import Order from './order.js'

const order = new Order();
const res = order.orderCoffee();
if(res){
    console.log("주문하신 커피가 준비되었습니다.");
} else {
    console.log("재고가 부족합니다.");
}
```

이처럼 프로그램을 잘 모르는 사용자에게 최소한의 API만 노출하는 것을 **Facade pattern**이라고 합니다. 