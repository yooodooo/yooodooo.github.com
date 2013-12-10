---
layout: post
title: Spring AOP Overview
tagline: [spring] 
group: spring
---
{% include JB/setup %}

AOP(Aspect-Oriented Programming)面向方面编程，提到这个概念，我们脑海中就会蹦出一些列的场景：日志记录、安全检查、事务控制等。而在日常
的工作中，特别是使用spring框架的开发中是必不可少的部分。但很多时候都停留在会使用的阶段，而对其原理也没做系统的研究与记录。这里我将自己学
习的一些记录

## 1、静态与动态 ##
AOP经历了静态AOP、动态AOP的发展历程。静态AOP的代表是[AspectJ](http://www.eclipse.org/aspectj/),简单的说就是需要通过ajc的编译器修
改java字节码将aspect植入。这种方式的优点是高效但是也不够灵活，编译的工作也挺大量，学习成本也高。  

随着技术的发展，特别是动态代理(DynamicProxy)的引进以及一些字节码操作技术的实现(cglib,asm)AOP也发展到了动态AOP的阶段。动态AOP主要是通过类加载或系统运行期间，对字节码进行操纵
的方式将aspect植入，这也带来一些性能问题，但是随着JVM的发展这些性能的损失也越来越小。


## 2、JAVA的实现 ##
- 动态代理  
JDK1.3之后引入了动态的机制，可以在运行时对目标接口生成代理对象。当然这里也只能是接口，这也是SpringAOP默认提供的代理实现。下面的例子展示了动态代理使用。  

>需要代理的对象  

		public interface HelloService {
	
		    public void method1(int topicId);
	
		    public void method2(int forumId);
	
		}

>aspect逻辑需要实现`InvocationHandler`接口

	public class PerformanceHandler implements InvocationHandler {
	
		private Object target;
	
		public PerformanceHandler(Object target) {
			this.target = target;
		}
	
		public Object invoke(Object proxy, Method method, Object[] args)
				throws Throwable {
			StopWatch watch = new StopWatch(method.getName());
			watch.start();
			Object obj = method.invoke(target, args);
			watch.stop();
			System.out.println(watch.shortSummary());
			return obj;
		}
	}

>Proxy获取代理对象

	public class TestAOPService {
	    public static void main(String[] args) {
	
	        //希望被代理的目标业务类
	        HelloService target = new HelloServiceImpl();
	
	        //将目标业务类和横切代码编织到一起
	        PerformanceHandler handler = new PerformanceHandler(target);
	
	        //根据编织了目标业务类逻辑和性能监视横切逻辑的InvocationHandler实例创建代理实例
	        HelloService proxy = (HelloService) Proxy.newProxyInstance(target.getClass().getClassLoader(), target
	                .getClass().getInterfaces(), handler);
	
	        //调用代理实例
	        proxy.method1(100);
	        proxy.method2(1012);
	    }
	}

- 动态字节码增强(CGLIB)  
我们可以采用asm,cglib等字节码增强技术，在程序运行期间动态修改class文件。SpringAOP在处理Class的代理采用Cglib的动态代理来实现。  

>Cglib通过生成子类来代理对象

		public class CglibProxy implements MethodInterceptor {
		    private Enhancer enhancer = new Enhancer();
		
		    public Object getProxy(Class<?> clazz) {
		        enhancer.setSuperclass(clazz); //设置需要创建子类的类
		        enhancer.setCallback(this);
		        return enhancer.create(); //通过字节码技术动态创建子类实例
		    }
		
		    //拦截父类所有方法的调用
		    public Object intercept(Object obj, Method method, Object[] args, MethodProxy proxy) throws Throwable {
		        StopWatch watch = new StopWatch(obj.getClass().getName() + "." + method.getName());
		        watch.start();
		        Object result = proxy.invokeSuper(obj, args); 
		        watch.stop();
		        System.out.println(watch.shortSummary());
		        return result;
		    }
		}

	
>从实际运行的类来看已经不是我们本身的对象：  

	method1: 100
	StopWatch 'org.java.codelib.proxy.HelloServiceImpl$$EnhancerByCGLIB$$d8d08640.method1': running time (millis) = 73
	method1: 23
	StopWatch 'org.java.codelib.proxy.HelloServiceImpl$$EnhancerByCGLIB$$d8d08640.method1': running time (millis) = 20

- 其他的方式  
常见的其他方式还有自定义类加载器、动态代码生成等

## 3、基本概念 ##
说到基本概念，关于AOP相信大家都会在各种资源上看到各种名词的解释：目标、切面、代理、通知、植入等反正一大堆，越说越混乱。其实想想AOP要做的无非是以下几个步骤：  

1. 找到需要改变的类(Joinpoint)，
2. 找到后何时改变：方法执行前？后？返回？等(Advice)。 
>这其中可能还有几个问题：  

3. 一是如何找到(Pointcut),
4. 再是找到后做何改变,如记录日志？事务？而这些记录日志事务的类就是(TargetObject)，
5. 最后让这些点统一起来工作就是植入(Weave)
