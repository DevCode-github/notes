Many application has layered approach, typically:
1. Web layer
2. Business layer
3. Data layer

Each layer has its own responsibility, however each layer has few common aspect that applies to all layers, ex:
- Security
- Logging
- Performance 
- etc.

These common aspects are known as **Cross Cutting Concerns**. Aspect oriented programming can be used to implement cross cutting concerns.

### What actually goes in AOP
- Implement cross cutting concern as an aspect
- Define point cut to indicate where the aspect should be applied
- Two popular frameworks:
	1. Spring AOP:
		- Not a Complete AOP solution but very popular
		- Only works with spring beans
		- Ex. Intercept method call to spring beans
	1. AspectJ:
		- Complete AOP solution but rarely used
		- Ex. Intercept any method call on any java class
		- Ex. Intercept any value in change of java fields

### Example
```
//Spring AOP
@Configuration
@Aspect
@Slf4j
public class LogginAspect{
	//pointcut = when ?
	@Before("execution(* com.pratice.aop.*.*.*(..))")
	public void logMethodCall(JoinPoint join_point) {
		log.info("BEFOR ASPECT - method " + join_point);
	}

}
```
In this code the aspect is being executed for the sub packages class's methods for package aop, before they are called.