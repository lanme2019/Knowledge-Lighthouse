spring传播行为


假设有两个方法A（） B（）
```

@transaction(propagation = Propagation.REQUIRED)
method A();

@transaction(propagation = Propagation.REQUIRED)
method B();


method A(){
     method B()
};
```


|传播行为|含义|
|-------|---------|
|REQUIRED|A如果有事务，B将使用该事务；如果A没有事务，B将创建一个新的事务。|
|REQUIRES_NEW|如果A有事务，将A的事务挂起，B创建一个新的事务；如果A没有事务，B创建一个新的事务。|
|SUPPORTS|A如果有事务，B将使用该事务；如果A没有事务，B将以非事务执行。|
|NOT_SUPPORTED|如果A有事务，将A的事务挂起，B将以非事务执行；如果A没有事务，B将以非事务执行。|
|MANDATORY|A如果有事务，B将使用该事务；如果A没有事务，B将抛异常。|
|NEVER|如果A有事务，B将抛异常；如果A没有事务，B将以非事务执行。|
|NESTED|A和B底层采用保存点机制，形成嵌套事务。|











