>函数是对象，函数名是指针

****
**1.函数声明和函数表达式不同**
>**1.1.**  函数声明
```
alert(sum(10,10));
function sum(num1,num2){
    return num1 + num2;
}
```

以上代码完全正常运行。代码开始执行之前，解析器就已经通过一个名为函数声明提升的过程，读取并将函数声明添加到执行环境中。
>**1.2.**  函数表达式
```
alert(sum(10,10));
var sum  = function(num1,num2){
    return sum1 + sum2;
}
```

以上代码运行会产生错误，函数位于一个初始化语句中，而不是一个函数声明。在执行到函数所在语句之前，变量sum中不会保存对函数的引用；而且，由于第一行代码就报错，实际上也不会执行到下一行。
****
**2.函数内部属性**

    函数内部有两个特殊对象：arguments和this

>**2.1**.  arguments主要用途是保存函数参数，但这个对象还有一callee属性。该属性是一个指针，指向这个arguments对象的函数。
****
>**严格模式不支持**

    例子
```
function factorial(num){
    if(num<=1){
        return 1;
    }else{
        return num*factorial(num-1)
    }
}
```
以上函数的执行和函数名factorial耦合，为了解耦，可以如下写：
```
function factorial(num){
    if(num<=1){
        return 1;
    }else{
        return num*agruments.callee(num-1)
    }
}
```
**3.函数属性和方法**

    每个函数都含两个属性：length和prototype。
>**3.1.**    length属性表示函数希望接受的命名参数的个数（形参个数）。
****
>**3.2.**    对于引用类型而言，prototype是保存他们所有实例方法的真正所在。
诸如toString()和valueOf()等方法实际上都保存在prototype名下，只不过是通过各自对象的实例访问罢了。

    每个函数都包含两个非继承而来的方法：apply()和call().
    两个方法都是在特定的作用域中调用函数，实际上等于设置函数体内this对象的值。
>**3.3.**      apply()方法接受两个参数：一个是在其中运行函数的作用域，另一个是参数数组。
(第一个参数就是thisthis对象的值，第二个参数可以是array的实例，也可以是arguments对象)
****
>**3.4.**    call()与apply()作用相同，区别在于接受参数的方式不同。对于call()而言，第一个参数是this值没有变化，变化的是其余参数都直接传递给参数。（传递给函数的参数必须逐个列举出来）

---
>**3.5.**    方法bind()

    ECMAScript5还定义了一个方法bind()。
    这个方法会创建一个函数的实例，其this值会被绑定到传给bind()函数的值。
```
window.color = 'red';
var o = { color: "blue" };
function sayColor(){
    alert(this.color);
}
var objectSayColor = sayColor.bind(o);
objectSayColor();      //blue
```
以上代码sayColor()调用bind()并传入对象o，创建了objectSayColor函数。
objectSayColor函数的this值等于o，因此即使是在全局作用域中调用这个函数，也会看到“blue”。（**优点见后**）

>每个函数继承的  **toLocaleString()，toString()，valueOf()**方法同样只返回函数代码。
