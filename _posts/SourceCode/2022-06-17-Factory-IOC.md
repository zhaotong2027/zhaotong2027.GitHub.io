---
layout: post
title: sping里的工厂模式BeanFactory --IOC
---

## 一、工厂的核心功能

生活中的工厂是什么？

工厂里有不同车间，同一车间生产同一种类的商品，不同的车间生产不同种类的商品。我们需要什么商品，就去工厂里购入，工厂再通知该商品的车间生产。如果没有这些车间呢？就成立一个车间，再生产。

spring的基本对象就是bean（商品），Bean太常用，每次声明都不好管理。就用BeanFactory来管理，但BeanFactory里的Bean大而全，没必要每次都声明全部Bean，因此用到什么类型的Bean，就在BeanFactory里声明。

## 二、实际例子

举例说明一次调用 BeanFactory 实现 IOC 的过程。准备（工程结构）：

```
IOC
 |---- java.fun.zhaotong.IOC
          |---- A.java
          |---- ZTBeanFactoryPostProcessor.java
          |---- ZTTest.java
 |---- resource
          |---- zttest.xml
```

##### 1、设置Bean原型

```java
public class A {
    private String name;

    public void init() {
        System.out.println("init:" + this.name);
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

##### 2、zttest.xml声明要调用到的Bean，并实例化，填充属性值

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="a" class="fun.zhaotong.A" init-method="init">
        <property name="name" value="zhangsan"></property>
    </bean>
    <bean class="fun.zhaotong.ZTBeanFactoryPostProcessor"></bean>
</beans>
```

##### 3、构造器声明实例化BeanFactory，初始化Bean

```java
public class ZTBeanFactoryPostProcessor implements BeanFactoryPostProcessor {
    @Override
    public void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory) throws BeansException {
        BeanDefinition a = beanFactory.getBeanDefinition("a");
        System.out.println("设置BeanDefinition");
    }
}
```

##### 4、实例化Bean

```java
public class ZTTest {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("zttest.xml");
        A bean = context.getBean(A.class);
        System.out.println("ZTTest:" + bean.getName());
    }
}
```

##### 5、调用，打印结果

```
> Task :IOC:ZTTest.main()
设置BeanDefinition
init:zhangsan
ZTTest:zhangsan
```

## 三、Bean的生命周期

实例化 

-> 填充属性（populate）

 -> 设置Aware接口属性 

-> BeanPostProcessor:before 

-> 执行init-method方法 

-> BeanPostProcessor:after

-> 完整Bean对象（context.getBean()）

```java
/*
 * <p>Bean factory implementations should support the standard bean lifecycle interfaces
 * as far as possible. The full set of initialization methods and their standard order is:
 * <ol>
 * <li>BeanNameAware's {@code setBeanName}
 * <li>BeanClassLoaderAware's {@code setBeanClassLoader}
 * <li>BeanFactoryAware's {@code setBeanFactory}
 * <li>EnvironmentAware's {@code setEnvironment}
 * <li>EmbeddedValueResolverAware's {@code setEmbeddedValueResolver}
 * <li>ResourceLoaderAware's {@code setResourceLoader}
 * (only applicable when running in an application context)
 * <li>ApplicationEventPublisherAware's {@code setApplicationEventPublisher}
 * (only applicable when running in an application context)
 * <li>MessageSourceAware's {@code setMessageSource}
 * (only applicable when running in an application context)
 * <li>ApplicationContextAware's {@code setApplicationContext}
 * (only applicable when running in an application context)
 * <li>ServletContextAware's {@code setServletContext}
 * (only applicable when running in a web application context)
 * <li>{@code postProcessBeforeInitialization} methods of BeanPostProcessors
 * <li>InitializingBean's {@code afterPropertiesSet}
 * <li>a custom init-method definition
 * <li>{@code postProcessAfterInitialization} methods of BeanPostProcessors
 * </ol>
 *
 * <p>On shutdown of a bean factory, the following lifecycle methods apply:
 * <ol>
 * <li>{@code postProcessBeforeDestruction} methods of DestructionAwareBeanPostProcessors
 * <li>DisposableBean's {@code destroy}
 * <li>a custom destroy-method definition
 * </ol>
 */
public interface BeanFactory {
```

实例化对象

```java
public static void main(String[] args) {
  ApplicationContext context = new ClassPathXmlApplicationContext("zttest.xml");


public class ClassPathXmlApplicationContext
  ...
	public ClassPathXmlApplicationContext(
			String[] configLocations, boolean refresh, @Nullable ApplicationContext parent)
			throws BeansException {
		// 调用父类构造方法，进行相关的对象创建等操作,包含属性的赋值操作
		super(parent);
		setConfigLocations(configLocations);
		if (refresh) {
			refresh();
		}
	}

public abstract class AbstractApplicationContext
  ...
	@Override
	public void refresh() throws BeansException, IllegalStateException {
  ...
  				// Register bean processors that intercept bean creation.
				// 注册bean处理器，这里只是注册功能，真正调用的是getBean方法
				registerBeanPostProcessors(beanFactory);

				// Initialize message source for this context.
				// 为上下文初始化message源，即不同语言的消息体，国际化处理,在springmvc的时候通过国际化的代码重点讲
				initMessageSource();

				// Initialize event multicaster for this context.
				// 初始化事件监听多路广播器
				initApplicationEventMulticaster();
  ...
    		// Instantiate all remaining (non-lazy-init) singletons.
				// 初始化剩下的单实例（非懒加载的）
				finishBeanFactoryInitialization(beanFactory);
  ...
    
	protected void finishBeanFactoryInitialization(ConfigurableListableBeanFactory beanFactory) {
    ...
		// Instantiate all remaining (non-lazy-init) singletons.
		// 实例化剩下的单例对象
		beanFactory.preInstantiateSingletons();
  }
  
  
public interface ConfigurableListableBeanFactory
    ...
    void preInstantiateSingletons() throws BeansException;

  
public class DefaultListableBeanFactory
	@Override
	public void preInstantiateSingletons() throws BeansException {
  ...
    					getBean(beanName);
```



销毁

## 四、spring里的PostProcessor

生产BeanFactory时实现的接口是BeanFactoryPostProcessor，BeanFactory是Bean工厂的意思，PostProcessor是后置处理器，是Bean工厂的增强器，以实现更丰富的功能。

BeanFactoryPostProcessor增强BeanDefinition信息，只有一个后置方法；

```java
public interface BeanFactoryPostProcessor {
  void postProcessBeanFactory
}
```

BeanPostProcessor增强Bean信息，有一个前置方法一个后置方法。

```java
public interface BeanPostProcessor {
  @Nullable
	default Object postProcessBeforeInitialization
  
  @Nullable
	default Object postProcessAfterInitialization
}
```

## 五、spring里的Aware

Aware字面意义是感知的意思，单纯的Bean是无知觉的，声明Aware可以让该Bean感知到自身的BeanName（对应Spring容器的BeanId属性）。Aware系列接口，主要用于辅助Spring bean访问Spring容器。举个例子：

##### 1、Bean原型中添加Aware

```java
public class A implements ApplicationContextAware {

    private String name;

    public void init() {
        System.out.println("init:" + this.name);
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    private ApplicationContext applicationContext;

    @Override
    public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
        this.applicationContext = applicationContext;
    }

    public ApplicationContext getApplicationContext() {
        return applicationContext;
    }
}
```

##### 2、在实例化Bean时就可直接调用方法

```
public class ZTTest {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("zttest.xml");
        A bean = context.getBean(A.class);
        System.out.println(bean.getApplicationContext());
        System.out.println("ZTTest:" + bean.getName());
    }
}
```

##### 3、再看源码

```java
abstract class AbstractApplicationContext
  
  @Override
	public void refresh() 
    
    prepareBeanFactory(beanFactory);  // 跳转代码在下个代码片段

  protected void prepareBeanFactory(ConfigurableListableBeanFactory beanFactory) {
    ...
    beanFactory.ignoreDependencyInterface(EnvironmentAware.class);
		beanFactory.ignoreDependencyInterface(EmbeddedValueResolverAware.class);
		beanFactory.ignoreDependencyInterface(ResourceLoaderAware.class);
		beanFactory.ignoreDependencyInterface(ApplicationEventPublisherAware.class);
		beanFactory.ignoreDependencyInterface(MessageSourceAware.class);
		beanFactory.ignoreDependencyInterface(ApplicationContextAware.class);
    ...
  }
```

```java
	protected void prepareBeanFactory(ConfigurableListableBeanFactory beanFactory) {
	  ...
		beanFactory.ignoreDependencyInterface(EnvironmentAware.class);
		beanFactory.ignoreDependencyInterface(EmbeddedValueResolverAware.class);
		beanFactory.ignoreDependencyInterface(ResourceLoaderAware.class);
		beanFactory.ignoreDependencyInterface(ApplicationEventPublisherAware.class);
		beanFactory.ignoreDependencyInterface(MessageSourceAware.class);
		beanFactory.ignoreDependencyInterface(ApplicationContextAware.class);
		...
	}
```

这些Aware之所以忽略，是因为Spring里的Aware容器是靠初始化实例化时，靠set方法实现的。

## 六、动态代理

```java
public abstract class AbstractAutoProxyCreator extends ProxyProcessorSupport
		implements SmartInstantiationAwareBeanPostProcessor, BeanFactoryAware {
  

public interface SmartInstantiationAwareBeanPostProcessor extends InstantiationAwareBeanPostProcessor {



public interface InstantiationAwareBeanPostProcessor extends BeanPostProcessor {
  // 有before和after两个方法，因此AbstractAutoProxyCreator必有这两个方法
```

```java
public abstract class AbstractAutoProxyCreator extends ProxyProcessorSupport
		implements SmartInstantiationAwareBeanPostProcessor, BeanFactoryAware {

  
  ...
    
    
	@Override
	public Object postProcessAfterInitialization(@Nullable Object bean, String beanName) {
		if (bean != null) {
			// 获取当前bean的key：如果beanName不为空，则以beanName为key，如果为FactoryBean类型，
			// 前面还会添加&符号，如果beanName为空，则以当前bean对应的class为key
			Object cacheKey = getCacheKey(bean.getClass(), beanName);
			// 判断当前bean是否正在被代理，如果正在被代理则不进行封装
			if (this.earlyProxyReferences.remove(cacheKey) != bean) {
				// 如果它需要被代理，则需要封装指定的bean
				return wrapIfNecessary(bean, beanName, cacheKey);
			}
		}
		return bean;
	}
  
  
  ... 13行
	
    
  protected Object wrapIfNecessary(Object bean, String beanName, Object cacheKey) {
		...
      
			Object proxy = createProxy(
					bean.getClass(), beanName, specificInterceptors, new SingletonTargetSource(bean));
    
			// 缓存生成的代理bean的类型，并且返回生成的代理bean
			this.proxyTypes.put(cacheKey, proxy.getClass());
			return proxy;
		}

		this.advisedBeans.put(cacheKey, Boolean.FALSE);
		return bean;
	}
  ...
}
```

```java
protected Object createProxy(Class<?> beanClass, @Nullable String beanName,
			@Nullable Object[] specificInterceptors, TargetSource targetSource) {
		...
		// 真正创建代理对象
		return proxyFactory.getProxy(getProxyClassLoader());

  
  
  
 public class ProxyFactory extends ProxyCreatorSupport {
	public Object getProxy(@Nullable ClassLoader classLoader) {
		// createAopProxy() 用来创建我们的代理工厂
		return createAopProxy().getProxy(classLoader);
	}
   
   
   
public interface AopProxy {
  Object getProxy(@Nullable ClassLoader classLoader);
  // 可看出有两个实现方式Cglib AOP，JDK AOP
```

