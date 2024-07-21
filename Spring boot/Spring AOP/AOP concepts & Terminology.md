### Compile Time
1. Advice: ^e25c71
	- What code to execute. Ex. Logging, Authentication
2. Pointcut: ^ae5e89
	- Expression that identifies method calls to be intercepted
	- Ex. ```execution(package_name.*.*.(..))```
3. Aspect: ^7394db
	- Advice + Pointcut
4. Weaver:
	- Weaver is the framework that implements AOP
	- Ex. AspectJ, Spring AOP
5. 
### Runtime
1. JoinPoint:
	- When point cut condition is true, the advice is executed. A specific execution instance of an advice is called a joinPoint.