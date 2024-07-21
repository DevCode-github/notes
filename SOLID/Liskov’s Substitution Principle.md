It states that if you have a class, you should be able to replace it with a subclass without causing any problems.

### Incorrect way

```
// Incorrect implementation violating LSP  
public class Bird {  
	public void fly() {  
		// I can fly  
	}  
	  
	public void swim() {  
		// I can swim  
	}  
	}  
	  
	public class Penguin extends Bird {  
	  
	// Penguins cannot fly, but we override the fly method and throws Exception  
	@Override  
	public void fly() {  
		throw new UnsupportedOperationException("Penguins cannot fly");  
	}  
}
```

### Correct way

```
// Correct implementation for LSP  
public class Bird {  
	// methods  
}  
  
public interface Flyable {  
	void fly();  
}  
  
public interface Swimmable {  
	void swim();  
}  
  
  
public class Penguin extends Bird implements Swimmable {  
	// Penguins cannot fly, therefore we only implement swim interface  
	@Override  
	public void swim() {  
		System.out.println("I can swim");  
	}  
}  
	  
public class Eagle extends Bird implements Flyable {  
	@Override  
	public void fly() {  
		System.out.println("I can fly");  
	}  
}
```

- `Bird`class serves **as a base class for birds** and includes common properties or methods shared among all birds.
- Introduced `Flyable` and `Swimmable` interfaces **to represent specific behaviors.**
- In the `Penguin` class, implemented the `Swimmable` interface to reflect penguins' swimming ability.
- In the `Eagle` class, implemented the `Flyable` interface to reflect eagles' flying ability.