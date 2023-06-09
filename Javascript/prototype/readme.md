<h1 align="center">Prototype</h1>
prototype holo JS er one of the most important concept. Javascript language tar ashol moja nite hole prototype er concept bujhte hobe. JS e amra joto function create kori each function er ekta built in object thake jeta holo `prototype`. Lets check is it true or not.

```js
function fun() {}
console.dir(fun);
```

![](../Pasted%20image%2020230406224032.png)

As function is a special types of object, so object also has this `prototype` .

**`Prototype` keno dorkar?**
In simple word, memory save korar jonne.
Amra jodi java or c++ er kotha chinta kori tahole dekhte parbo ei language gula object oriented paradigm follow kore. Tai eshob language by default `inheritance` support kore. Kintu javascript er khetre `inheritance` support kore kina ask korle ans both **no** and **yes** . Karon, java or c++ er moto ekhane `classical` inheritance support kore na. Ekhane `prototype` use kore inheritance achieve kora jaay.

```java
class Veyron {
  String engine = "w-16";
  void racingMode() {
    System.out.println("racing mode enabled");
  }
}

class Chiron extends Veyron {
  void offRodeMode() {
    System.out.println("going offroad");
  }
}

class Main {
  public static void main(String[] args) {
    var chiron = new Chiron();
    System.out.println(chiron.engine); // w-16
    chiron.racingMode(); // racing mode enabled
  }
}
```

uporer Java code er example ta jodi dekhi. Java object oriented language. Tar mane eta by default inheritance support kore. Kheyal kori, `Veyron` er **engine** ar **racingMode** functionality `Chiron` inherit kore. Tai `chiron` e egula alada bhabe implement korar dorkar hoy na.

erokom similar example javascript e kibhabe achieve kora jaay prototype use kore sheta dekhar aage amra dekhbo prototype kivabe ashlo.

```js
function Food(name, cost) {
  let food = {
    name: name,
    cost: cost,
  };

  return food;
}

let pizza = Food("pepperoni", 15);

console.dir(pizza);
```

![](../Pasted%20image%2020230407032057.png)

`Food` function create korlam jaa ekta object return kore. Jotbor e function ta call kora hobe totobar e notun notun object create hobe. Ekhon jodi `momos` naam e arekta object create kori.

```js
let momos = Food("chicken momo", 20);
console.dir(momos);
```

![](../Pasted%20image%2020230407032351.png)

ekhane `pizza` and `momos` duita object er jonnei `cost` and `name` er alada alada copy create hoyeche memory te sheta amra dekhtei parchi. Eta make sense kore karon different food er name and cost alada hotei pare. But jodi duita object e emon ekta method ke hold kore jar purpose same . i.e `serve`

```js
function Food(name, cost) {
  let food = {
    name: name,
    cost: cost,
    serve: function () {
      console.log(this.name, "is being served");
    },
  };

  return food;
}

let pizza = Food("pepperoni", 15);
let momos = Food("chicken momo", 20);

console.dir(pizza);
console.dir(momos);
```

![](../Pasted%20image%2020230407032743.png)

duita object e same `serve` function er alada alada copy memory te create kore. Eta mainly redundant . As purpose same so alada alada copy create kore space nosto korar kono mane hoy na. Lets fix it.

```js
function Food(name, cost) {
  let food = {
    name: name,
    cost: cost,
    serve: method.serve,
  };

  return food;
}

let method = {
  serve: function () {
    console.log(this.name, "is being served");
  },
};

let pizza = Food("pepperoni", 15);
let momos = Food("chicken momo", 20);

console.dir(pizza);
console.dir(momos);
```

ekhane `serve` function alada ekta object er moddhe rakha hoyeche. And `Food` constructor er `food` object er property te refer kora hoyeche. So `pizza` ar `momos` instance jokhon create kora hoy tokhon `serve` er alada alada copy create hobe na rather ektai copy thakbe jetake both `pizza` and `momos` er `food` property refer korte pare.

<h3 align="center">Object.create</h3>

**Object.create** kono ekta object er prototype use kore new ekta object create kore return kore. For example -

```js
let smoothie = {
  name: "fruit smoothie",
  price: 20,
};

let shake = Object.create(smoothie);
console.log(shake);
console.log(shake.name);
```

![](../Pasted%20image%2020230413181546.png)

console.log kore dekha gelo ekta empty object. Othocho `shake.name` correct output dicche. Eta tahole kibhabe hocche. Ektu aagei bollam eta object er prototype use kore.

![](../Pasted%20image%2020230413181638.png)

object empty holeo er prototype er moddhe properties gula thie e ache.

Ekhon ekhane jodi server er moto aro kichu method thakto tahole amra obossjoi chai na ebhabe individually `method` gulo property hishebe assign korte. Er jonne amra **Object.create** use korbo.

```js
function Food(name, cost) {
  let food = Object.create(method);
  food.name = name;
  food.cost = cost;

  return food;
}

let method = {
  serve: function () {
    console.log(this.name, "is being served");
  },
};

let pizza = Food("pepperoni", 15);
let momos = Food("chicken momo", 20);

console.dir(pizza);
console.dir(momos);
```

![](../Pasted%20image%2020230412181440.png)

Notice that `server` method ta ekhon ar object er part na, eta ekhon `prototyper` er moddhe chole geche. Ei way te joto khushi toto method create kora jabe ja prototype er moddhe thakbe.

object.create giving blank object but can access properties. It inherits from parent object

<h3 align='center'>new and this keyword</h3>

```js
function Food(name, cost) {
  this.name = name;
  this.cost = cost;
}

Food.prototype = {
  serve() {
    console.log(this.name, "is being served");
  },
};

let pizza = new Food("pepperoni", 15);
let momos = new Food("chicken momo", 20);

console.dir(pizza);
console.dir(momos);
```

![](../Pasted%20image%2020230414174921.png)

this emon ekta object ja JS nije nije create kore jokhon e amra `new` use kore kono object instance assign kori. And `Food` function nije nijei `this` ke return kore. Previous example amra jemon ta korechilam alada ekta `food` object create kore (using Object.create) return korechilam.

Amra already jani je JS e shob kicuhi object. Ekohon dekha jaak `array` te kibhabe object er implementation hoye thake.

```js
function List() {
  this.len = 0;
  Object.defineProperty(this, "len", {
    enumerable: false,
  });
  Object.defineProperty(List.prototype, "push", {
    enumerable: false,
  });
}

List.prototype = {
  push(value) {
    this[this.len] = value;
    this.len++;
  },
};

const list = new List();

list.push("mercedes");
list.push("ferrari");
list.push("mclaren");

for (let i = 0; i < list.len; i++) {
  console.log(list[i]);
}

console.dir(list);
```

```
output:

mercedes
ferrari
mclaren
```

![](../Pasted%20image%2020230414180324.png)

notice korle dekhbo `len` ar `push` little bit light color hoye ache. Er karon amra ei property gula ke **enumerable false** defined korechi. Jodi `list` ke **foor in** loop diye enumerate kori tahole dekho ei duita property print hocche na, only car er `key` gula print hocche. Ei key guloi mainly array er _index_ bujhay.

```js
for (let prop in list) {
  console.log(prop);
}
```

```
0
1
2
```

amra just ekta `push` method dekhlam. JS er je built in `Array` ache er prototype er moddhe aro onek `method` ache. Jemon `map`, `filter`, `find`, `reduce` ,`findIndex`, `forEach`, `sort`, `indexOf`, `shift` etc.

<h3 align='center'>Es6 classes</h3>

JavaScript e year 2015 e ES6 class introduce kora hoy. Ja shudhui ekta syntax matro ja other object oriented er syntax ke mimic kore. Under the hood eta `prototype` er mechanism use use kore.

lets replicate one of prototype example with classes.

```js
function Food(name, cost) {
  this.name = name;
  this.cost = cost;
}

Food.prototype = {
  pay() {
    console.log(`${this.name} cost ${this.cost}$`)
  },
  eat() {
    console.log(`eating ${this.name}`)
  }
}

let pizza = new Food('pepperoni pizza', 10)
let burger = new Food('veg burger', 5)

pizza.pay() // pepperoni pizza cost 10$
burger.eat() // eating veg burger

console.dir(pizza)
```

![](../Pasted%20image%2020230416144344.png)

now lets take a look how we can convert this constructor function to es6 classes.

```js
class Food {
  constructor(name, cost) {
    this.name = name;
    this.cost = cost;
  }
  pay() {
    console.log(`${this.name} cost ${this.cost}$`);
  }
  eat() {
    console.log(`eating ${this.name}`);
  }
}

let pizza = new Food("pepperoni pizza", 10);

pizza.pay(); // pepperoni pizza cost 10$
console.dir(pizza);
```

![](../Pasted%20image%2020230416144727.png)

[next topic (later) >>](https://google.com)
