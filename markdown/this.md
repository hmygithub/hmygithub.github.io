# 跨过this这道坎
掌握了this,js面向对象就攻克了一半。今天总结一下this的用法。
### 一、在全局作用域中（所有函数外）出现的this,指全局对象
`console.log(this) // window`
### 二、在函数内部出现的this,要看this所在的函数的被调用方式
#### 1、普通函数中被直接调用,this指全局对象window
#### function声明函数
````
function foo(){
    console.log(this)
}
foo()
````
//function声明函数赋给变量
````
var foo=function(){
    console.log(this)
}
foo()
````
// 自执行函数
`(function(){console.log(this)})`
#### 2、对象方法调用
`````
var person={
    name:'person',
    run:function(){return this}
}
person.run()
`````
//这里this就是指调用它的对象，当结构比较复杂时，比如：
`````
var animal={
    name:'animal',
    returnThis(){ return person.run()}
}
console.log(animal.returnThis())
`````
// 使用this的函数是person.run，所以this绑定到person；只要看使用this的那个函数。
// 总结：被作为某个对象A的方法调用时，this指方法所属的对象A。即属于谁，被谁直接调用，
// this就指向谁
// 3、事件绑定：
````
var btn = document.querySelector("button")
    btn.onclick = function () {
    console.log(this) // btn
}
````
// 4、事件监听：
````
var btn = document.querySelector("button")
btn.addEventListener('click', function () {
console.log(this) //btn
})
````
#### 5、在构造函数或者构造函数原型对象中this指向构造函数的实例
不使用new指向window
`````
function Person(name){
    console.log(this)//window
    this.name=name
}
Person("小明")
`````
使用new
``````
function Person(name){
    this.name=name
    self = this
}
var people = new Person('xiaoming')
console.log(self === people)// true
``````
// 这里new改变了this指向，将this由window指向Person的实例对象people，
// people相当于复制了构造函数的值
// 当用new 运算符调用函数时，该函数总会返回一个对象，通常情况this就是指向这个对象
还有一种特殊情况:如果构造器显式的返回一个object类的对象，运算结果最终返回的是那个对象而不是this
``````
function Person2(name){
    this.name=name
    return{name:"xiaowang"}
}
var obj = new Person2('xiaoming')
console.log(obj.name)
``````
#### 5、jquery的ajax
``````````
$.ajax({
    self: this,
    type:"get",
    url: url,
    async:true,
    success: function (res) {
        console.log(this) // this指向传入$.ajxa()中的对象
        console.log(self) // window
    }
});
``````````
//这里说明一下，将代码简写为$.ajax（obj） ，this指向obj,在obj中this指向window，
// 因为在在success方法中，独享obj调用自己，所以this指向obj

## 改变this的指向问题
###  一、  Function.prototype.call或者Function.prototype.apply可以动态改变this
//    两者的功能完全一样，只是参数不同
//    call第一个参数是this要指向的上下文，后面跟着的参数不固定，都是当成函数的参数传递进去
//    apply第一个参数也是this要指向的上下文，后面是一个数组或者类数组，数组的每一项按照顺序当成是函数的参数传递进去，也可以这样理解这个数组就是函数的arguments
      ````
      var obj1={
          name:'小明',
          fruit:'apple'
      }
      ````
      ````
      var obj2={
          name:'小刚',
          toy:'car'
      }
      ````
      ````
      var getToy=function(x,y){
          console.log(x,y)
          console.log(this.toy)
      }
      ````
      ````
      var getFruit=function(x,y){
          console.log(x,y)
          console.log(this.fruit)
      }
      ````
//        getToy("x","y")
       ``
       getFruit.call(obj1,"x","y")
       getToy.apply(obj2,["x","y"])
        ``

// 为什么要改变向上下文呢?当一个对象没有某个方法的时候，而另一个对象有这个方法，
// 这个时候就可以通过改变上下文来使用自己没有的方法了。
// 那什么时候用call什么时候用apply？当参数个数是固定不变的时候可以用call，当参数个数不确定可以用apply
//使用函数的apply或call方法调用时，this指第一个参数B。
//    func.apply(B, [m, n, ...]);
//    func.call(B, m, n, ...);

### 二、Object.prototype.bind，它不但通过一个新函数来提供永久的绑定，还会覆盖call和aplply的绑定。
    ```
    function returnThis(){
        return this
    }
    ```
    ``````
    var s1={name:'s1'}
    var s2={name:'s2'}
    var s1returnThis = returnThis.bind(s1)
    console.log(s1returnThis())
    s1returnThis.call(s2)
    console.log(s1returnThis())
    ``````
### 三、一个比较容易忽略的会绑定this的地方就是new。当我们new一个函数时，就会自动把this绑定在新对象上，然后再调用这个函数。
// 它会覆盖bind的绑定。
```
    function showThis () {
        console.log(this)
    }
    ```
    ``
    showThis() // window
    new showThis() // showThis
    ``
    ``````
    var boss1 = { name: 'boss1' }
    showThis.call(boss1) // boss1
    new showThis.call(boss1) // TypeError
    var boss1showThis = showThis.bind(boss1)
    boss1showThis() // boss1
    new boss1showThis() // showThis
    ``````
### 四、箭头函数，绑定将不起作用
//  对于箭头函数，只要看它在哪里创建
    ````
    function callback (cb) {
        cb()
    }
    callback(() => { console.log(this) }) // window
    ````
    ``````
    var boss1 = {
        name: 'boss1',
        callback: callback,
        callback2 () {
        callback(() => { console.log(this) })
    }
    ``````
    ``
    boss1.callback(() => { console.log(this) }) // still window
    boss1.callback2(() => { console.log(this) }) // boss1
    ``