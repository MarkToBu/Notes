### Java中八种基本数据类型，都不是Object的子类，所以比较应该用连等。（==）     不能使用 equals方法比较

###  JAVA语言中有个名词叫自动拆箱、装箱，那这个自动拆箱、装箱到底是指啥？

自动拆箱、装箱是从JDK1.5开始才有的特性，其实它主要就是指基本类型与包装类的自动转换。

如int 与Integer类型。 
int 是基本类型，而Integer是int的包装类，在JDK1.5之前，int类型的值是不能直接赋给Integer类型的值 的，也就是说 
Integer integer = 5; 会报错，因为5是基本类型，而Integer是包装类，Integer的正确定义方式为： 
Integer integer = new Integer(5); 但是，从基本类型转换成包装类是经常使用的操作，尤其是Integer与int的转换很是频繁。所以在JDK1.5开始，它们之间的转换不在须要程序员再去进行转换了，JDK已经将它自动进行了转换，这种操作就叫自动拆箱、装箱。

int i = 5;
Integer ii = i;     //这种写法在JDK1.5及以后的版本是正确的，因为系统会自动将int向Integer进行转换，这种操作就叫自动装箱。

int j = ii;         //这种写法是将Integer的值自动转换成了int基本类型，这种自动转换的方法就叫自动拆箱。

不只是int与Integer可以自动转换，八大基本类型都可以, 以下是八大基本类型及对应的包装炻

基本类型 byte short int long float double char boolean 
包装类型 Byte Short Integer Long Float Double Character Boolean

其中，int与Integer的转换最多也最频繁，所以有一点要注意，也是面试时常问到的问题： 
int与Integer的区别： 
1.int的默认值 为0，而Integer的默认值为null,在使用Integer前需要初始化。 
2.int是基本类型，而Integer是包装类，可以自动 拆箱、拆箱，Integer封装了很多的方法.

    Java没有全局变量
-------------------------------------------------

      Object类是每个类的先祖(基类）

```java
    集合只能容纳对象引用。
```

### java中的符号，二进制，左移，右移。

在xml中使用转移字符传智     &amp;



## List s = new ArrayList();

更广的使用范围，             s.add(555);     s = new LinkedList(s);           s = new ArrayList(3);

##  方法重载

​      方法名必须相同

​      返回值可以不同

​      参数列表必须不同



##  修饰字符

  public    protected    defalut    private

##          mvc

## 高内聚 低耦合



## Oracle 和Mysql的不同

  当前时间，

  分页。

##        如何对数据库进行备份

##  设置临时classpath

    ```
set classpath=.;G:\document\易途\自用\阶段提升\mysql.jar;
    ```

