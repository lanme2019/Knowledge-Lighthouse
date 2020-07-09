面试中总问new 一个string类会产生几个对象

##  基础字符串分析
```java
        String s1 = "lanme2019";
        String s2 = "lanme2019";
        System.out.println( s1 == s2 ); // true
```

分析 
- 产生一个对象 在字符串常量池
- `lanme2019`因为是字面量所以在编译期就会加载到常量池中，字符串常量池生成一个`lanme2019`对象，s1 s2返回同一个对象的引用


```java
        String s1 = new String("lanme2019");
        String s2 = new String("lanme2019");
        System.out.println( s1 == s2 ); // false
```
分析 
- 产生1个或2个对象 如果字符串常量池中没有则添加，有则返回地址指向堆中对象
- 会先检查字符串常量池中是否有`lanme2019`,如果没有则添加到字符串常量池，再将引用指向string对象

```java
        String s1 = "lanme" + "2019";
        String s2 = "lanme2019";
        System.out.println( s1 == s2 ); // true
```
分析 
- 产生一个对象
- jvm会优化指令把 `"lanme" + "2019"` 视为 `lanme2019`


```java
        String s1 = "lanme";
        String s2 = "2019";
        String s3 = s1 + s2 ;
        String s4 = "lanme2019";
        System.out.println( s3 == s4 ); // false
```
分析 
- 产生4个对象 s1 s2 stringbuild对象 lanme2019
- 这种引用变量相加底层会调用stringbuild方法进行append 这种方式只会在堆生成一个对象不会再常量池添加

##  进阶 intern 分析

```java
        
        String s1 = new String("lanme")+new String("2019");;
        System.out.println( s1.intern() == s1 ); //true
        String s2 = "lanme2019";
        System.out.println( s1 == s2 ); //true

```

调换 s2 语句的位置结果完全相反
```java
        String s2 = "lanme2019";
        String s1 = new String("lanme")+new String("2019");;
        System.out.println( s1.intern() == s1 ); //false       
        System.out.println( s1 == s2 ); //false
```


intern的作用是把string对象的引用加入到常量池中，如果存在就返回常量池中对象的引用，不存在就创建一个指向string对象的引用在常量池中
