# DESIGN PATTERN

## Adaptor pattern

실생활에서 adaptor는 쉽게 볼 수 있다. 대표적으로 USB2.0의 micro 5pin 충전기를 USB3.0의 c-type로 바꿔주는 스마트폰 adaptor가 있고 해외 여행을 가려면 가장 먼저 준비하는 110v - 250v **돼지코**가 있다. 이 adaptor를 사용하는 이유는 간편하기 때문이다. 휴대폰이 바뀔때마다 충전기를 바꿔야하는 불편함은 어느정도 해소가 가능하지만, *해외여행을 갈따마다 110v용 전자기기를 구입하는 것은 억지스럽다*.

<img src="https://user-images.githubusercontent.com/44011462/90095919-f7b0ff80-dd6c-11ea-9735-a45e9e7f4120.png" width="200px">
<img src="https://user-images.githubusercontent.com/44011462/90095893-e4059900-dd6c-11ea-8a36-d1e24fd7d3ad.png" width="200px">


소프트웨어에서도 동일한 일이 발생한다. 여러가지 이유로 인해 기존의 구조를 새로운 구조로 변경하려고 할때마다 기존의 것을 이용하지 못하고 새로 만들어야 한다면 *앞서 살펴본 억지스러운 일*이 될 수 있다. 소프트웨어에서 **돼지코**의 역할을 하는 design pattern이 **Adaptor pattern**이다. 

흔히 사용하는 adaptor 예제인 **Printer class**를 살표보자. 기본적인 생성자, 글자를 입력하는 함수, 출력하는 함수가 있는 class이다.
```javascript
// Printer.js
class Printer{
    constuctor(){
        this.textArr = [];
    }

    pushText(text){
        this.textArr.push(text);
    }

    print(){
        return this.textArr.join(' ');
    }
}
```

위에서 Printer를 만들었고 다음과 같은 코드를 이용하여 문자를 console에 출력 할 수 있다.
```javascript
import Printer from './Printer.js'

const printer = new Printer();
printer.pushText("hello");
printer.pushText("Adapter");
printer.pushText("design");
printer.pushText("pattern");

const result = printer.print();
console.log(result);    // hello Adpater design pattern
```

위와 같은 소프트웨어를 사용하다가 인스타그램이 널리 이용되게 되면서 '#'(hash tag)를 붙이는 Printer로 바꾸어야 한다고 가정하자. 아래를 새롭게 만들어야하는 **HashTagPrinter**의 Interface(명세)이다.
```javascript
// HashTagPrinter.js 
class HashTagPrinter { 
    constructor() { 
        this.textArr = []; 
    } 

    pushText(text) { 
        this.textArr.push(text); 
    }

    printWithHashTag() { 
        // print -> printWithHashTag로 변경 
        return this.textArr.map(text => `#${text}`).join(' '); 
    }
}
```

위에서 전달받은 새로운 HashTagPrinter를 이용하여 기존에 가지고 있던 main.js에 **그대로 붙여서 사용**하려고 한다. *마치 갤럭시 S10e에 USB 2.0 micro 5pin 충전기를 사용하려는 것처럼*
```javascript
import HashTagPrinter from './HashTagPrinter.js'

const printer = new HashTagPrinter();
printer.pushText("hello");
printer.pushText("Adapter");
printer.pushText("design");
printer.pushText("pattern");

const result = printer.print();     // interface가 들어맞지 않아서 에러가 발생한다.
console.log(result);   
```

이 상황에서 USB 2.0 micro 5pin 충전기를 USB 3.0 c-type 충전기로 바꾸기에는 *시간과 비용적인 측면에서 부담*스럽다. 이때 필요한 **Adaptor**를 코드로 구현해보자. 다시 말해서 *Printer에서 print()가 사용되었을때, HashTagPrinter의 printWithHashTag()를 동작*하도록 만들면 된다.

```javascript
// HashTagAdapter.js
class HashTagAdapter{
    constructer(hashTagPrinter){
        this.printer = hashTagPrinter;
    }

    pushText(item){
        this.printer.pushText(text);
    }

    print(){
        return this.printer.printWithHash();
    }
}
```

아래와 같은 코드가 앞서 만든 Adapter를 이용하여 기존의 print()를 가지고 원하는 결과를 출력할 수 있다. client 입장에서는 아무런 변화없이 기존의 서비스를 이용하는 것과 같은 효과를 낼 수 있다. 코드가 바뀔 떄 마다 client도 변화한다면 이는 매우 억지스러운 일이다.

```javascript
// main.js
import HashTagPrinter from './HashTagPrinter.js'
import HashTagAdapter from './HashTagAdapter.js'

const printer = new HashTagAdapter(new HashTagPrinter());
printer.pushText("hello");
printer.pushText("Adapter");
printer.pushText("design");
printer.pushText("pattern");

const result = printer.print();     
console.log(result);   // #hello #Adapter #design #pattern
```


https://dev-momo.tistory.com/entry/Adapter-Pattern-%EC%96%B4%EB%8C%91%ED%84%B0-%ED%8C%A8%ED%84%B4  
https://www.zerocho.com/category/JavaScript/post/57babe9f5abe0c17006fe230  


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