# ArrayList源码阅读

## 概念

**ArrayList**是List接口的具体实现类。  
其底层是基于一个动态扩容的对象数组实现的。

*jdk版本：openjdk version "11.0.12" 2021-07-20*

## 成员变量篇

> **静态变量**
> INITIAL_CAPACITY = 10
> 数组默认的 ***初始容量*** ：10

> **静态变量**
> DEFAULT_EMPTY_ELEMENTDATA
> 一个空数组对象。  
> 何时：**无参构造**，实例的elementData指向它。
> ``` mermaid
> graph LR
> A("ArrayList( )")
> ```

> **静态变量**
> EMPTY_ELEMENTDATA
> 一个空数组对象。
> + **构造函数(容量)**，容量 =0 时，elementData指向它；  
> + **构造函数(集合)**，集合长度 =0 时，elementData指向它。
>```mermaid
>    graph LR
>    A("ArrayList(int initialCapacity)")-->B(initialCapacity == 0)
>    C("ArrayList(Collection < ? extends E> c)")-->D("a = c.toArray(); a.length == 0")
>``` 



## 构造函数篇

ArrayList共提供了三种构造函数


### ArrayList( ) 无参构造函数

实例的对象数组引用默认空数组**DEFAULT_EMPTY_ELEMENTDATA** *(静态变量)*。


``` JAVA
public ArrayList(){
    //  elementData引用默认空数组DEFAULT_EMPTY_ELEMENTDATA
    this.elementData = DEFAULT_EMPTY_ELEMENTDATA;

}
```
### ArrayList(int initialCapacity) 初始容量
根据 initialCapacity的值 分三种情况
> 1. initialCapacity > 0  
>    + new一个新的对象数组赋值给当前实例的数组对象 elementData

> 2. initialCapacity == 0
>    + 将默认空数组EMPTY_ELEMENTDATA赋值给elementData

> 3. else 负数
>    + 抛出异常

``` JAVA
public ArrayList(int initialCapacity){
    // 嗯！真好，有长度标准了，省的自己去制定
    // 就你了，initialCapacity
    if(initialCapacity > 0){
        this.elementData = new Object[initialCapacity];

    // 严重怀疑你是来交作业的。
    // 这似有标准又没标准的样子，像极了老师让我做题，我只能马虎凑数先糊弄一下。
    // 给我个0，那我只能默认空数组了。
    }else if(initialCapacity == 0){
        this.elementData = EMPTY_ELEMENTDATA;

    // 啥？啥？啥？你给俺的这是啥？长度：-1 ？ -2 ？
    // 直接摆烂。异常，走你！！!
    }else{
        throw new IllegalArgumentException("Illegal initialCapacity: "+ initialCapacity);
    }

}
```

### ArrayList(Collection<? extends E> c) 带集合初始化

> 1. 获得集合的数组a

> 2. 数组a长度不为 0
>    + 集合为ArrayList
>        + 直接将a赋值给elementData
>    + 集合不为ArrayList
>        + 根据数组a复制一个新的对象数组赋值给elementData

> 3. 数组长度为0
>    + 默认空数组赋值给elementData

``` JAVA
public ArrayList(Collection<? Extends E> c)
    // 获得集合对应的数组
    Object[] a = c.toArray();

    // 数组长度 != 0,有戏！！！
    if((size = a.length)!=0){

        // 集合c就是ArrayList集合
        // 老乡见老乡，别见外，直接来吧！！
        if(c.getClass() == ArrayList.class){
            elementData = c;
        
        // 咦！！！不是ArrayList阵营
        // 必须给你同化一下
        }else{
            elementData = Arrays.copyof(a,size,Object[].class);
        }
    // 数组长度 == 0，默认空数组
    // elementData : 给我整个空的集合。拿来吧你！EMPTY_ELEMENTDATA
    }else{
        elementData = EMPTY_ELEMENTDATA;
    }
```