# js中的继承小结
构造超类-->
```
    function Super() {
        this.name="AA"
    }
    Super.prototype.getName=function () {
        console.log(this.name)
    };
```

## 1.原型继承
让子类的原型指向 超类的一个实例，就会拥有 实例私有的属性。通过 实例的__proto__找到超的原型，里面的属性和方法一样可以找到。
即 var sub=new Sub(); sub拥有Super的私有和原型上的属性和方法。
核心理念: 通过 增加sub子类和super超类之间的原型链 链接，让 Sub的实例能找到Super的原型和私有属性，
以后Super的原型上添加方法。sub依然可以找到

   
```
    function Sub() {

    }
    Sub.prototype=new Super();

    var sub=new Sub();
    console.log(sub.name);
    sub.__proto__.name='BB';
    sub.getName();
    console.log(Sub.prototype);
    var sub1=new Sub();
    console.log(sub1.name);
    console.dir(sub.__proto__);
    
```

## 2.call继承  

只会继承私有的  实例化的时候调用父类的构造函数

```
    function Sub() {
        Super.call(this);
    }
    var sub=new Sub();
    console.log(sub.name)

```
## 3. 对象冒充继承   
实例化的时候，把父级私有的公有的都遍历出来，都加到子级的实例私有属性上。没有原型链的联系

```
  function Sub() {
        var super1=new Super;
        for(var key in super1){
            this[key]=super1[key];
        }
        super1=null;
    }
    var sub=new Sub();
    console.log(sub.name);
    sub.getName()*/
```
## 4.组合继承

我们的call继承 继承了私有的。缺少公有的方法。那我们可以用
    call+原型继承来实现公有属性的继承。 好处是 私有的还在         实例的私有属性上，每一个实例都有自己的。通过 原型链也可以找到公有的属性。缺点是 原型上 多了一份 私有的属性。 超级的构造函数也执行了两次

## 5. 中间类继承
一种在 超类和 子类加了一层原型链的方式 .当然只是继承原型上的方法，也就是公有的。  最坑的地方就是不兼容  不然用这个 + call继承 可以完美实现 继承

```
function Sub() {

    }
    Sub.prototype.__proto__=Super.prototype;
    var sub=new Sub();
    console.log(sub.getName);
```

## 6. 寄生组合 
综合以上的继承方法，我们想实现那种 私有的属性是私有的，公有的就在原型上。只能用 组合式的继承来实现。把共享的属性、方法用原型链继承实现，独享的属性、方法用借用构造函数实现，所以组合继承几乎完美实现了js的继承.但是一个bug就是 原型上多了 私有的属性。而且让超类构造函数执行两次。
    // 现在的ECMAscript提供了一个 方法，  Object.create(proto,[propertiesObject])
    // 返回一个对象。该对象的原型 指向  Super.prototype
```    
   function Sub() {

    }
    Sub.prototype=Object.create(Super.prototype);

    console.log(Sub.prototype); 
    这样子类就继承了超类 公有的方法
```

但是ES5 兼容性 我们需要自己写一个 Objectcreate方法

```
    function objectCreate(o) {
        function Temp() {}    
        Temp.prototype=o;
        return new Temp();
    }
    function Sub() {

    }
    Sub.prototype=objectCreate(Super.prototype);
    var sub=new Sub;
    console.log(sub.getName)
```
我创建一个仅用于 封装继承的函数  用这个当做Super类的副本 实现共享原型, 但是由于这个 副本 不会被操作，也就不用担心 超类的原型被修改。然后让子类 去继承这个副本的原型。 创建一个实例，让子类的原型指向这个实例。即实现了原型链的继承，还不会影响超类的原型。 而且以后调用 父类的构造函数了，只需要执行这个副本函数。


```
end by SYJ
```
