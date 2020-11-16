# spring生命周期
（1）Core

container

核心容器包括了Core、Beans、Context和Expression Language模块。

Core和Beans模块是框架的基础部分，提供了IoC和DI特性，这里的基础概念是BeanFactory，它提供对Factory模式的经典实现来消除对程序性单例模式的需要，并真正地允许你从程序逻辑中分离出依赖关系和配置。

ØCore主要包含Spring框架的核心工具类

ØBeans主要包含访问配置文件、创建和管理bean以及DI相关的一些类

ØContexts基于Core和Beans模块之上，为Spring提供大量的扩展，提供了国际化，资源加载等

ØExpression Language提供了一个强大的表达式语言用于在运行时查询和操作对象。

(2)Data

access/Integration

Data access/Integration层包含了JDBC、ORM、OXM、JMS和Transation模块，主要是和数据库相关的操作。

(3)Web

Web上下文模块建立在应用程序上下文模块之上，为基于Web的应用程序提供了上下文。

(4)AOP

AOP模块提供了一个符合AOP联盟标准的面向切面编程实现，可以降低耦合性。

(5)Test

Test模块支持各种测试框架。


2.依赖注入（DI）和面向切面编程（AOP）
控制反转（IoC）又称依赖注入。控制反转概念包含两个层面的意思，“控制”是接口实现类的选择控制权；而“反转”是指这种控制权从调用类转移到外部第三方类或容器的手中。Spring使用控制反转技术实现了松耦合。依赖被注入到对象，而不是创建或寻找依赖对象。

所有的类都会在Spring容器中登记，告诉Spring你是个什么东西，你需要什么东西，然后Spring会在系统运行到适当的时候，把你要的东西主动给你，同时也把你交给其他需要你的东西。所有的类的创建、销毁都由Spring来控制，也就是说控制对象生存周期的不再是引用它的对象，而是Spring。对于某个具体的对象而言，以前是它控制其他对象，现在是所有对象都被Spring控制，所以这叫控制反转。

在运行时，动态地将代码切入到类的指定方法、指定位置上的编程思想就是面向切面的编程。Spring支持面向切面编程，把应用的业务逻辑与系统的服务分离开来。

2.依赖注入有哪些方式
接口注入：指的就是在接口中定义要注入的信息，并通过接口完成注入。

Set注入：指的就是在接受注入的类中定义一个Set方法，并在参数中定义需要注入的元素。

构造注入：指的就是在接受注入的类中定义一个构造方法，并在参数中定义需要注入的元素。

3.SpringMVC的请求处理流程
（1）用户请求发送到前端控制器DispatcherServlet。

（2）DispatcherServlet接收发过来的请求，交给HandlerMapping处理器映射器

（3）HandlerMapping根据请求路径找到相应的HandlerAdapter处理器适配器（处理器适配器就是那些拦截器或Controller）

（4）HandlerAdapter处理一些功能请求，返回一个ModelAndView对象（包括模型数据、逻辑视图名）

（5）ViewResolver视图解析器，先根据ModelAndView中设置的View解析具体视图，然后再将Model模型中的数据渲染到View上

这些过程都是以DispatcherServlet为中轴线进行的。


4.Bean的生命周期
实例化一个Bean时

1）实例化

2）调用set方法

3）后置处理器（调用initMethod之前）

4）initMethod

5）后置处理器（调用initMethod之后）

6）bean的使用

7）destroyMethod方法

Spring Bean详细生命周期

1)  Spring对Bean进行实例化。

2)  Spring将值和Bean的引用注入进Bean对应的属性中。

3)如果Bean实现了BeanNameAware接口，Spring将bean的ID传递给setBeanName()接口方法。

4)如果Bean实现了BeanFactoryAware接口，Spring将调用setBeanFactory()接口方法，将BeanFactory容器实例传入。

5)如果Bean实现了ApplicationcontextAware接口Spring将调用setApplicationContext()接口方法，将应用上下文的引用传入。

6)如果Bean实现了BeanPostProcessor接口Spring将调用它们的postProcessBeforeInitialization接口方法。

7)如果Bean实现了InitializingBean接口，Spring将调用它们的afterPropertiesSet()接口方法．类似地，如果Bean使用init-method声明了初始化方法，该方法也会被调用。

8)如果Bean实现了BeanPostProcessor接口，Spring将调用它们的postPoressAfterInitilization方法。

9)此时此刻Bean已经准备就绪，可以被应用程序使用了，它们将一直驻留在应用上下文中．直到该应用上下文补销毁。

10)如果Bean实现了DisposableBean接口，Spring将调用它的destroy()接口方法。同样，如果Bean使用destroy-method声明了销毁方法，方法也会被调用。


5.Bean的作用域
Spring定义了几种作用域，下面是常用的四种：

Ø单例（Singleton）：整个应用中只创建一个bean的实例。

Ø原型（Prototype）：每次注入或者通过Spring上下文获取的时候，创建一个新的bean实例。

Ø会话（Session）：在Web应用中，为每个会话创建一个bean实例。

Ø请求（Request）：在Web应用中，为每个请求创建一个bean实例。

6.BeanDefinition类
BeanDefinition是一个接口，在Spring中存在三种实现：RootBeanDefinition、ChildBeanDefinition以及GenericBeanDefinition。三种实现均继承了AbstractBeanDefinition，其中BeanDefinition是配置文件元素标签在容器中的内部表现形式。

在配置文件中可以定义父和子，父用RootBeanDefinition表示，而子用ChildBeanDefinition表示，而没有父的就使用RootBeanDefinition表示。AbstractBeanDefinition对两者共同的类信息进行抽象。

Spring通过BeanDefinition将配置文件中的配置信息转换为容器的内部表示，并将这些BeanDefinition注册BeanDefinitionRegistry中。Spring容器的BeanDefinitionRegistry就是配置信息的内存数据库，主要是以map的形式保存，后续操作直接从BeanDefinitionRegistry中读取配置信息。

7.Bean的解析（XML配置文件的解析）
Resource类

Spring则采用Resource来对各种资源进行统一抽象，Resource是一个接口，定义了资源的基本操作，包括是否存在、是否可读、是否已经打开等等。由继承关系可以看到Resource继承了InputStreamSource，InputStreamSource封装任何可以返回InputStream的类，并仅声明了一个方法：getInputStream()，用于返回一个新的InputStream对象。对不同来源的资源文件都有相应的Resource实现：文件（FileSystemResource）、Classpath资源（ClassPathResource）、URL资源（UrlResource）、InputStream资源（InputStreamResource）、Byte数组（ByteArrayResource）。

解析过程

1）配置文件的封装

Resource接口抽象了所有Spring内部使用到的底层资源：File、URL、ClassPath等。

2）加载Bean

A）封装资源文件。当进入XMLBeanDefinitionReader后首先对参数Resource使用EncodedResource类进行封装。

B）获取输入流。从Resource中获取对应的InputStream并构造InputSource。

C）通过构造的InputSource实例和Resource实例继续调用函数doLoadBeanDefinition。

a）获取对XML文件的验证模式

b）加载XML文件，并得到对应的Document

c）根据返回的Document解析并注册BeanDefinition

在解析的过程中，对于根节点或者子节点如果是默认命名空间的话则采用parseDefaultElement方法进行解析，否则使用delegate.parseCustomElement方法对自定义命名空间进行解析。

总结：

1）将配置文件封装成Resource资源文件类

2）利用EncodedResource二次包装资源文件

3）获取资源输入流，并构造InputSource对象

4）获取XML文件的实体解析器和验证模式（常见的验证模式有DTD和XSD两种）

5）加载XML文件，获取对应的Document对象

6）由Document对象解析并注册BeanDefinition

到这里我们已经完成了静态配置到动态BeanDefinition的解析，并注册到容器中，接下去将探究如何创建并初始化bean实例的过程。

8.Spring
Bean加载流程

在AbstractBeanFactory中doGetBean（getBean方法中调用该方法）对加载bean的不同情况进行拆分处理，并做了部分准备工作。

（1）转换对应的beanName

这里需要注意传入的name可能不是beanName，传入的name可能是别名，也可能是FactoryBean。解析过程就是去除FactoryBean的前缀修饰符，如果是alias找到最终的beanName。所以需要进行一系列的解析，这些解析内容包括如下内容：

1）去除FactoryBean的修饰符，也就是如果name=“&aa”，那么会首先去除&而使name=“aa”。

2）取指定alias所表示的最终beanName，例如别名A指向名称为B的bean则返回B；若别名A指向别名B，别名B又指向名称为C的bean则返回C。

（2）尝试从缓存中加载单例

单例在Spring的同一个容器内只会被创建一次，后续再获取bean，就直接从单例缓存中获取了。这里只是尝试加载，首先尝试从缓存中加载，如果加载不成功则再次尝试从singletonFactories中加载，因为在创建单例bean的时候会存在依赖注入的情况，而在创建依赖的时候为了避免循环依赖，在Spring中创建bean的原则是不等bean创建完成就会将创建bean的ObjectFactory提早曝光加入到缓存中，一旦下一个bean创建时候需要依赖上一个bean则直接使用ObjectFactory。

具体过程：首先尝试从singletonObjects里面获取实例，如果获取不到再从earlySingletonObjects里面获取，如果还获取不到，再尝试从singletonFactories里面获取beanName对于的ObjectFactory，然后调用这个ObjectFactory的getObject来创建bean，并放到earlySingletonObjects里面去，并且从singletonFactories里面remove掉这个ObjectFactory，而对于后续的所有内存操作都只为了循环依赖检测时候使用，也就是在allowEarlyReference为true的情况下才会使用。

这里涉及用于存储bean的不同的map，简单解释如下：

singletonObjects：用于保存BeanName和创建bean实例之间的关系，bean name->bean

instance，这边缓存的是实例

singletonFactories：用于保存BeanName和创建bean的工厂之间的关系，bean

name->ObjectFactory，这边缓存的是为解决循环依赖而准备的ObjectFactory

earlySingletonObjects：也是保存BeanName和创建bean实例之间的关系，与singletonObjects的不同之处在于，当一个单例bean被放到这里面后，那么当bean还在创建过程中，就可以通过getBean方法获取到了，其目的是用来检测循环引用。这边缓存的也是实例,只是这边的是为解决循环依赖而提早暴露出来的实例,其实是ObjectFactory。registeredSingletons：用来保存当前所有已注册的bean从bean的实例中获取对象

（3）bean的实例化

如果从缓存中得到了bean的原始状态，则需要对bean进行实例化。这里有必要强调一下，缓存中记录的是原始的bean状态，并不一定是我们最终想要的bean。举个例子，假如我们需要对工厂bean进行处理，那么这里得到的其实是工厂bean的初始状态，但是我们真正需要的是工厂bean中定义的factory-method方法中返回的bean，而getObjectForBeanInstance就可以完成这个工作的。

GetObjectForBeanInstance所做的工作：

1）对FactoryBean正确性的验证。

2）对非FactoryBean不做任何处理

3）对bean进行转换

4）将从Factory中解析bean的工作委托给getObjectFromFactoryBean，如果bean声明为FactoryBean类型，则当提取bean时提取的并不是FactoryBean，而是FactoryBean中对应的getObject方法返回的bean。

（4）原型模式的依赖检查

只有在单例情况下才会尝试解决循环依赖，如果存在A中有B的属性，B中有A的属性，那么当依赖注入的时候，就会产生当A还未创建完的时候因为对于B的创建再次返回创建A，造成循环依赖，也就是情况：isPrototypeCurrentlyInCreation(beanName)判断true。（5）检测parentBeanFactory

从代码上看，如果缓存中没有数据的话直接转到父类工厂上去加载了，这是为何呢？条件是parentBeanFactory

!= null && !containsBeanDefinition(beanName)。关键在于containsBeanDefinition方法，它是在检测如果当前XML中没有配置对应的beanName，只能到parentBeanFactory中进行一下尝试。

（6）将存储XML配置文件的GenericBeanDefinition转换为RootBeanDefinition

因为从XML配置文件中读取Bean信息是放在GenericBeanDefinition中的，但是所有的Bean后续处理都是针对RootBeanDefinition的。这里做一下转化，转换的同时如果父类bean不为空的话，则会一并合并父类的属性。

（7）寻找依赖

因为Bean的初始化过程中，很可能会用到某些属性，而某些属性很可能是动态配置的，并且配置成依赖于其他的bean，那么这个时候就有必要先加载依赖的bean，所以在Spring的加载顺序中，在初始化某一个bean的时候首先初始化这个bean的依赖。

（8）针对不同的scope进行bean的创建

我们知道在Spring中可以指定各种scope，默认singleton。但是还有些其他的配置诸如prototype、Request之类的。在这个步骤中，Spring会根据不同的配置进行不同的初始化策略。

（9）类型转换

程序到这里返回bean基本结束了，通常对该方法的调用参数requiredType是为空的，但是可能会存在这种情况，返回的bean其实是个String，但是requiredType却传入Integer类型，那么这个时候这个步骤就起作用了，它的功能是将返回的bean转换为requiredType所指定的类型。当然，Spring转换为Integer是最简单的一种转换，在Spring中提供了各种各样的转换器，用户也可以自己扩展转换器来满足需求。

总结

1）获取参数name对应的真正的beanName

2）检查缓存或者实例工厂中是否有对应的单例，若存在则进行实例化并返回对象，否则继续往下执行

3）执行prototype类型依赖检查，防止循环依赖

4）如果当前beanFactory中不存在需要的bean，则尝试从parentBeanFactory中获取

5）将之前解析过程返得到的GenericBeanDefinition对象合并为RootBeanDefinition对象，便于后续处理

6）如果存在依赖的bean，则递归加载依赖的bean

7）依据当前bean的作用域对bean进行实例化

8）如果对返回bean类型有要求，则进行检查，按需做类型转换

9）返回bean实例

9.BeanFactory和FactoryBean
BeanFactory是个Factory，也就是IOC容器或对象工厂，FactoryBean是个Bean。在Spring中，所有的Bean都是由BeanFactory(也就是IOC容器)来进行管理的。但对FactoryBean而言，这个Bean不是简单的Bean，而是一个能创建或者修饰对象生成的工厂Bean。

BeanFactory是IOC最基本的容器，负责生产和管理bean，它为其他具体的IOC容器提供了最基本的规范。

FactoryBean是一个接口，当在IOC容器中的Bean实现了FactoryBean后，通过getBean(String

BeanName)获取到的Bean对象并不是FactoryBean的实现类对象，而是这个实现类中的getObject()方法返回的对象。要想获取FactoryBean的实现类，就要getBean(&BeanName)即在BeanName之前加上&。

10.Spring如何实现IOC
Spring的两种IoC容器

1）BeanFactory

Ø基础类型的IoC容器；

Ø采用延迟初始化策略（容器初始化完成后并不会创建bean的对象，只有当收到初始化请求时才进行初始化）；

Ø由于延迟初始化，因此启动速度较快，占用资源较少；

2）ApplicationContext

Ø在BeanFactory的基础上，增加了更为高级的特定：事件发布、国际化等；

Ø在容器启动时便完成所有bean的创建；

Ø启动时间较长，占用资源更多；

IoC容器的初始化过程

Spring IoC容器的初始化过程分为三个阶段：Resource定位、BeanDefinition的载入和向IoC容器注册BeanDefinition。Spring把这三个阶段分离，并使用不同的模块来完成，这样可以让用户更加灵活的对这三个阶段进行扩展。

1）Resource定位指的是BeanDefinition的资源定位，它由ResourceLoader通过统一的Resource接口来完成，Resource对各种形式的BeanDefinition的使用都提供了统一的接口。

2）BeanDefinition的载入是把用户定义好的Bean表示成IoC容器内部的数据结构，而这个容器内部的数据结构就是BeanDefinition，BeanDefinition实际上就是POJO对象在IoC容器中的抽象。通过BeanDefinition，IoC容器可以方便的对POJO对象进行管理。

3）向IoC容器注册BeanDefinition是通过调用BeanDefinitionRegistry接口的实现来完成的，这个注册过程是把载入的BeanDefinition向IoC容器进行注册。实际上，在IoC容器内部维护着一个HashMap，而这个注册过程其实就将BeanDefinition添加至这个HashMap。

11.Spring如何解决循环依赖
循环依赖就是循环引用，就是两个或多个Bean相互之间的持有对方，比如CircleA引用CircleB，CircleB引用CircleC，CircleC引用CircleA，则它们最终反映为一个环。

构造器循环依赖：表示通过构造器注入构成的循环依赖，此依赖是无法解决的，只能抛出BeanCurrentlyInCreationException异常表示循环依赖。

setter循环依赖：表示通过setter注入方式构成的循环依赖。对于setter注入造成的依赖是通过spring容器提前暴露刚完成构造器注入但未完成其他步骤（如setter注入）的Bean来完成的，而且只能解决单例作用域的Bean循环依赖。对于“prototype”作用域Bean，Spring容器无法完成依赖注入，因为“prototype”作用域的Bean，Spring容器不进行缓存，因此无法提前暴露一个创建中的Bean。对于“singleton”作用域Bean，可以通过“setAllowCircularReferences(false);”来禁用循环引用。





对于单例对象来说，在Spring的整个容器的生命周期内，有且只存在一个对象，很容易想到这个对象应该存在Cache中，Spring大量运用了Cache的手段，在循环依赖问题的解决过程中甚至使用了“三级缓存”。

singletonObjects指单例对象的cache，singletonFactories指单例对象工厂的cache，earlySingletonObjects指提前曝光的单例对象的cache。以上三个cache构成了三级缓存，Spring就用这三级缓存巧妙的解决了循环依赖问题。

分析getSingleton的整个过程，Spring首先从singletonObjects（一级缓存）中尝试获取，如果获取不到并且对象在创建中，则尝试从earlySingletonObjects(二级缓存)中获取，如果还是获取不到并且允许从singletonFactories通过getObject获取，则通过singletonFactory.getObject()(三级缓存)获取。

singletonObjects和earlySingletonObjects的区别

两者都是以beanName为key，bean实例为value进行存储，区别在于singletonObjects存储的是实例化完成的bean实例，而earlySingletonObjects存储的是正在实例化中的bean，所以两个集合的内容是互斥的。

12.Spring容器的功能扩展
（1）增加了对Spring表达式（SpEL）的支持。

（2）增加了对属性编辑器的支持。

（3）增加了对一些Aware类的自动装配支持。

（4）增加了对ApplicationListener类型bean的发现支持。

（5）增加了对AspectJ的支持。+

（6）注册了几个系统环境变量相关的bean。

（7）BeanPostProcessor后处理器

（8）不同语言的消息体国际化处理


13.SpingAOP
AOP原理三句话就能概括：

（1）对类生成代理使用CGLIB

（2）对接口生成代理使用JDK原生的Proxy

（3）可以通过配置文件指定对接口使用CGLIB生成代理


14.Spring框架中都用到了哪些设计模式？
工厂模式—BeanFactory用来创建各种不同的Bean。

单例模式—在spring配置文件中定义的bean默认为单例模式。

模板方法—用来解决代码重复的问题。比如：JdbcTemplate, JmsTemplate, JpaTemplate。

代理模式—AOP、事务等都大量运用了代理模式。

原型模式—特点在于通过“复制”一个已经存在的实例来返回新的实例，而不是新建实例。被复制的实例就是我们所称的“原型”，这个原型是可定制的。

职责链模式—在SpringMVC中，我们会经常使用一些拦截器(HandlerInterceptor),当存在多个拦截器的时候，所有的拦截器就构成了一条拦截器链。

观察者模式—Spring中提供了一种事件监听机制，即ApplicationListener，可以实现Spring容器内的事件监听。


15.Spring支持的事务，Spring如何管理事务
编程式事务指的是通过编码方式实现事务，即类似于JDBC编程实现事务管理。

声明式事务

ØXML实现

Ø注解实现@Transactional

编程式事务允许用户在代码中精确定义事务的边界，而声明式事务（基于AOP）有助于用户将操作与事务规则进行解耦。简单地说，编程式事务侵入到了业务代码里面，但是提供了更加详细的事务管理；而声明式事务由于基于AOP，所以既能起到事务管理的作用，又可以不影响业务代码的具体实现。

Spring对事务管理是通过事务管理器来实现的。Spring提供了许多内置事务管理器实现。他们将事务管理的职责委托给hibernate或者JTA等持久化机制所提供的相关平台框架的事务来实现。

Spring事务管理器的接口是org.springframework.transaction.PlatformTransactionManager，通过这个接口，Spring为各个平台如JDBC、Hibernate等都提供了对应的事务管理器，但是具体的实现就是各个平台自己的事情了。具体的事务管理机制对Spring来说是透明的，它并不关心那些，那些是对应各个平台需要关心的，所以Spring事务管理的一个优点就是为不同的事务API提供一致的编程模型，如JTA、JDBC、Hibernate、JPA。

事务属性可以理解成事务的一些基本配置，描述了事务策略如何应用到方法上。事务属性包含了5个方面。

传播行为：当事务方法被另一个事务方法调用时，必须指定事务应该如何传播。

隔离级别：定义了一个事务可能受其他并发事务影响的程度。

是否只读事务：如果事务只对后端的数据库进行该操作，数据库可以利用事务的只读特性来进行一些特定的优化。，

事务超时：为了使应用程序很好地运行，事务不能运行太长的时间。因为事务可能涉及对后端数据库的锁定，所以长时间的事务会不必要的占用数据库资源。事务超时就是事务的一个定时器，在特定时间内事务如果没有执行完毕，那么就会自动回滚，而不是一直等待其结束。

回滚规则：这些规则定义了哪些异常会导致事务回滚而哪些不会。默认情况下，事务只有遇到运行期异常时才会回滚，而在遇到检查型异常时不会回滚（这一行为与EJB的回滚行为是一致的）但是你可以声明事务在遇到特定的检查型异常时像遇到运行期异常那样回滚。同样，你还可以声明事务遇到特定的异常不回滚，即使这些异常是运行期异常。


16.Spring常见注解
@Controller

在SpringMVC中，控制器Controller负责处理由DispatcherServlet分发的请求，它把用户请求的数据经过业务处理层处理之后封装成一个Model，然后再把该Model返回给对应的View进行展示。在SpringMVC中提供了一个非常简便的定义Controller的方法，你无需继承特定的类或实现特定的接口，只需使用@Controller标记一个类是Controller，然后使用@RequestMapping和@RequestParam等一些注解用以定义URL请求和Controller方法之间的映射，这样的Controller就能被外界访问到。

@RequestMapping

可以使用@RequestMapping来映射URL到控制器类，或者是到Controller控制器的处理方法上。method的值一旦指定，那么，处理方法就只对指定的http method类型的请求进行处理。可以为多个方法映射相同的URI，不同的http method类型，Spring MVC根据请求的method类型是可以区分开这些方法的。

@RequestParam和@PathVariable区别

在spring MVC中，两者的作用都是将request里的参数的值绑定到control里的方法参数里的，区别在于，URL写法不同。

使用@RequestParam时，URL是这样的：

http://host:port/path?参数名=参数值

使用@PathVariable时，URL是这样的：

http://host:port/path/参数值

@Autowired

@Autowired可以对成员变量、方法和构造函数进行标注，来完成自动装配的工作。

@Service @Controller@Repository @Component

@Repository、@Service和@Controller其实这三个跟@Component功能是等效的，用在实现类上

Ø@Service用于标注业务层组件（我们通常定义的service层就用这个）

Ø@Controller用于标注控制层组件（如struts中的action）

Ø@Repository用于标注数据访问（持久层）组件，即DAO组件

Ø@Component泛指组件，当组件不好归类的时候，我们可以使用这个注解进行标注。

这几个注解是当你需要定义某个类为一个bean，则在这个类的类名前一行使用@Service("XXX"),就相当于讲这个类定义为一个bean，bean名称为XXX;这几个是基于类的，我们可以定义名称，也可以不定义，不定义会默认以类名为bean的名称（类首字母小写）。

@Value

在spring 3.0中，可以通过使用@value，对一些如xxx.properties文件中的文件，进行键值对的注入

@ResponseBody

该注解用于将Controller的方法返回的对象，通过适当的HttpMessageConverter转换为指定格式后，写入到Response对象的body数据区


17.Spring有几种配置方式
将Spring配置到应用开发中有以下三种方式：

Ø基于XML的配置

Ø基于注解的配置

Ø基于Java的配置

18.Spring Boot介绍
Spring Boot简化Spring应用程序开发。从本质上来说，Spring Boot就是Spring，它做了那些没有它你也会去做的Spring Bean配置。它使用“习惯优于配置”（项目中存在大量的配置，此外还内置了一个习惯性的配置，让你无需手动进行配置）的理念让你的项目快速运行起来。

（1）自动配置：针对很多Spring应用程序常见的应用功能，Spring Boot能自动提供相关配置。当开发人员在pom文件中添加starter依赖后，maven或者gradle会自动下载很多jar包到classpath中。当Spring Boot检测到特定类的存在，就会针对这个应用做一定的配置，自动创建和织入需要的spring bean到程序上下文中。@SpringBootApplication开启Spring的组件扫描和Spring Boot的自动配置功能。

（2）起步依赖：告诉Spring Boot需要什么功能，它就能引入需要的库，利用了传递依赖解析，把常用库聚合在一起，组成了几个为特定功能而定制的依赖，不需要担心版本不兼容的问题。

（3）命令行界面：这是Spring Boot的可选特性，借此你只需写代码就能完成完整的应用程序，无需传统项目构建。

（4）Actuator：让你能够深入运行中的Spring Boot应用程序，一探究竟，它提供在运行时检视应用程序内部情况的能力。比如：查看Spring应用程序上下文里所有的Bean、查看自动配置决策、查看线程活动等。
。