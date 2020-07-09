## 堆内存溢出

因为堆内存的是实例对象  所以我们无限创建对象并保持gcroot可达就可以快速占满内存

参数设置 

`-Xms20M` `-Xmx20M` `-XX:+HeapDumpOnOutOfMemoryError`
初始堆与最大堆等大避免内存自动扩展

```java

public class HeapOOM
{
    static class OOMObject{}
    public static void main(String[] args) {
        ArrayList<int[]> list = new ArrayList<int[]>();

        while (true){
            list.add(new int[1000000000]);
        }
    }
}

```

## 栈内存溢出

栈只需要无限递归使得新的栈帧超出线程的允许的深度


```java

public class StackOverFlow {

    int count = 0;

    public void stackTest(){
        count++;
            //无限调用自己
        stackTest();
    }

    public static void main(String[] args) throws Throwable{
        StackOverFlow stack = new StackOverFlow();
        try {
           stack.stackTest();
        }catch (Throwable e){
            System.out.println(stack.count);
            throw e;
        }


    }
}

```
一般情况下`hotspot`虚拟机只会发生`StackOverFlow`，不会发生`oom`是由于hotspot不会动态扩容内存大小，一个线程栈的大小是确定的，在运行期不不会扩展，所以我们通过无限创建线程来达到OOM

```java
for(;;){
    new thread()
}
```


## 方法区运行时常量池溢出

方法区由于在本地内存中，不太好溢出，但是字符串常量池所在的堆内存是可以溢出的

