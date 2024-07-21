1. ```@Aspect```  ^9e61b0
	- Aspect declaration, to declare the class will contain AOP
```
	@Configuration
	@Aspect
	public class SomeClass{
		//code
	}
```

2. ```@Before``` ^d4a87f
	- Do something before a method is called.
	- For the  advice a specific execution instance is created called ```JoinPoint``` i.e., passed as an argument.
```
	@Before("execution(* com.pratice.aop.*.*.*(..))")
	public void logMethodCall(JoinPoint join_point) {
		log.info("BEFOR ASPECT - method " + join_point);
	}
```

3. ```@After``` ^ad1cbf
	- Do something after a method is executed irrespective of whether method was successful or exception was thrown.
	- For the  advice a specific execution instance is created called ```JoinPoint```, i.e., passed as an argument.
```
	@After("execution(* com.pratice.aop.*.*.*(..))")
	public void logMethodCall(JoinPoint join_point) {
		log.info("AFTER ASPECT - method " + join_point);
	}
```

4. ```@Around``` ^15c10c
	- Do something only when a method executes successfully
	- For the  advice a specific execution instance is created called ```ProceedingJoinPoint```, i.e., passed as an argument.
```
	@Around("execution(* com.pratice.aop.*.*.*(..))")
	public Object logMethodCall(JoinPoint join_point) {
		
	}
```

5. ```@AfterReturning``` ^88659e
	- Do something only when a method executes successfully
```
	@AfterReturning(
		pointcut = "execution(* com.pratice.aop.*.*.*(..))",
		returning = "result"
	)
	public void logMethodCallAfterReturning(JoinPoint join_point, Object result) {
		log.info("AFTER RETURNING ASPECT - {} method with result - {}", join_point, result);
	}
```

6. ```@AfterThrowing``` ^f74507
	- Do something only when a method throws an exception
```
	@AfterReturning(
		pointcut = "execution(* com.pratice.aop.*.*.*(..))",
		returning = "result"
	)
	public void logMethodCallAfterReturning(JoinPoint join_point, Object result) {
		log.info("AFTER RETURNING ASPECT - {} method with result - {}", join_point, result);
	}
```