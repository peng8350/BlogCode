---
title: ReactNative入坑记-this指针理解
date: 2018-03-20 20:17:03
categories: 分析
tags: [ReactNative,Mac OS]
---

# this指针
 <p>刚入门ReactNative半个月,在前期学习的过程中,总是对这个this指针的概念不是很清晰和理解,每次调用这个this也要想一想它到底是指向谁?所以,很有必要仔细研究这个this指向。
 <!--more-->

## What is "this"?
 <p>众所周知,在Java面向对象语言里,它就是指向当前类的对象,Object-C,C++和JAVA都是指向当前的类对象,但是Javascript里面的this指针和其他语言的this不是一个概念。简单一句话来说,JavaScript里面的this就是指向调用它的那个对象,或许这么一说,你还是有点模糊。不过往下看!
 
## 例子

### 例子一
```
	 constructor(){
        super()

        this.state = {
     
            aaa: 'we'
        }
    }
	
	ButtonClick(){
		this.setState({...})
		//alert(this.state.visiable)
	}
	
	render(){
		return(
			<View>....
				<Button onPress={this.ButtonClick} title={....}/>
			</View>
		)
	}

```
<p>比如,我想实现一个功能,就是点击按钮隐藏某个组件,试图去利用回调函数来改变当前组件的state,最后报错,"setState is not a function",大概就是说没有这个方法的意思吧,也尝试alert输出sate,也是报错underfined。当时,对this不理解所犯下的一个错误,原因很简单,因为this压根就不是指向当前类对象。



### 例子二
```
		//类一
		class Person1{
		    constructor(){
		        this.age = 18
		    }
		
		    printAge(){
		        alert(this.age)
		    }
		}
		//类二
		class Person2{
		    constructor(){
		        this.age = 15
		    }
		
		    printAge(){
		        alert(this.age)
		    }
		}

  		let p1 = new Person1()
        let p2 = new Person2()
        p1.printAge()//输出18
        var method = p1.printAge
        method.call(p2)//输出15


```
<p>如上,这里我有两个类,里面都有一个共同属性age,当他们作为构造函数使用new关键字时,默认指向当前类对象。p1.printAge()利用p1来调用printAge方法输出18,看得出来this指向的就是p1,但通过引用p1的printAge方法来交给p2这个对象调用的时候,输出就是p2里面的age里的值,证明this指针指向的就是调用它的那个对象,p1调用了它,就是指p1,p2调用了它,就是p2

## 特殊情况
### 箭头函数
() => {},使用这种箭头函数时,this指针和普通函数this指针的指向不同,在箭头函数里,this是和其他面向对象语言的this指向一样,都是指向当前类的对象,不会因为调用对象而改变指向。

### call和apply
上面已经演示了call改变this的作用,所以这两个方法可以改变调用这个方法的对象,也就是说会改变方法里的this

### 构造函数

 如果在一个函数前面加上new关键字来调用，那么就会创建一个连接到该函数的prototype成员的新对象，同时，this会被绑定到这个新对象上。这种情况下，这个函数就可以成为此对象的构造函数。例如：

```
function obj(){  
  this.name = "obj";  
}  
var o = new obj();  
alert(o.name); //"obj"  
```
使用new创建对象o时，在构造函数中this指向了o，因此o的name属性被赋值为“obj”。


# 总结完毕!
