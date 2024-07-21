Part of spring framework context module, providing a means of application wise broadcasting of information allowing beans to interact without being tightly coupled.

### Basic Spring Boot Event
- Events are published using ```applicationeventpublisher```
#### custom event

```
public class userregistrationevent extends applicationevent {  
	private string username;  
  
	public userregistrationevent(object source, string username) {  
		super(source);  
		this.username = username;  
	}  
	  
	public string getusername() {  
		return username;  
	}  
}
```

#### Event Publisher
to publish this we will use ```applicationeventpublisher```

```
@service  
public class userregistrationservice {  
	@autowired  
	private applicationeventpublisher eventpublisher;  
	  
	public void registeruser(string username) {  
		// ... registration logic ...  
	  
		eventpublisher.publishevent(new userregistrationevent(this, username));  
	}  
}
```

#### Event Listener

1. by extending ```applicationlistener```
```
	@component  
	public class userregistrationlistener implements applicationlistener<userregistrationevent> {  
		@override  
		public void onapplicationevent(userregistrationevent event) {  
			// ... handle event ...  
		}  
	}
```

2. using annotation
```
	@component  
	public class userregistrationlistener {  
		@eventlistener  
		public void handleuserregistrationevent(userregistrationevent event) {  
			// ... handle event ...  
		}  
	}
```


### Asynchronous Event

- by default all events are synchronous.

to use asynchronous events, we need to enable async in application configs
```
	@configuration  
	@enableasync  
	public class springasyncconfig {}
```
then add ```@async``` to event listener
```
	@component  
	public class userregistrationlistener {  
		@async  
		@eventlistener  
		public void handleuserregistrationevent(userregistrationevent event) {  
			// ... handle event ...  
		}  
	}
```

### Conditional Event

- Spring boot also allows to conditionally handle events based on certain criteria.
- We can use ```@eventlistener```'s condition attribute to define spel expression whether event should handled or not

```
	@Component  
	public class UserRegistrationListener {  
		@EventListener(condition = "#event.username.startsWith('admin')")  
		public void handleAdminRegistrationEvent(UserRegistrationEvent event) {  
			// ... handle event ...  
		}  
	}
```

### Transactional Events
- Event's are tied to transaction management

1. To public transactional event, we use ```TransactionalApplicationEventPublisher```
	- Only published after successful completion of transaction
```
	@Service  
	public class UserRegistrationService {  
		@Autowired  
		private TransactionalApplicationEventPublisher eventPublisher;  
		  
		@Transactional  
		public void registerUser(String username) {  
			// ... registration logic ...  
			  
			eventPublisher.publishEvent(new UserRegistrationEvent(this, username));  
		}  
	}
```

2. Using ```@TransactionalEventListener```
	- Spring allow event to bind to a phase of a transaction
```
	@Component
	class UserRemovedListener {
	
	  @TransactionalEventListener(phase=TransactionPhase.AFTER_COMPLETION)
	  void handleAfterUserRemoved(UserRemovedEvent event) {
	    // handle UserRemovedEvent
	  }
	}
```
- A Transaction has following:
	1. AFTER_COMMIT - Indicates if the transaction committed successfully, means transaction is successful.
	2. AFTER_COMPLETION - Indicates if the transaction is successfully committed or rolled-back, means transaction completion.
	3. AFTER_ROLLBACK - Indicates if the transaction has rolled-back successfully.
	5. BEFOR_COMMIT - Indicates transaction is about to perform commit.


### Order of Events
- There is no specific order defined in spring boot
- We can do so using ```@Order``` annotation

```
	@Component  
	@Order(1)  
	public class FirstUserRegistrationListener implements ApplicationListener<UserRegistrationEvent> {  
		@Override  
		public void onApplicationEvent(UserRegistrationEvent event) {  
			// ... handle event ...  
		}  
	}  
	  
	@Component  
	@Order(2)  
	public class SecondUserRegistrationListener implements ApplicationListener<UserRegistrationEvent> {  
		@Override  
		public void onApplicationEvent(UserRegistrationEvent event) {  
			// ... handle event ...  
		}  
	}
```

### Generics Event
- In latest version of spring boot we can use generic event, with the flexibility to publish arbitrary event without extending ```ApplicationEvent```

```
	public class GenericSpringEvent<T> {
	    private T what;
	    protected boolean success;
	
	    public GenericSpringEvent(T what, boolean success) {
	        this.what = what;
	        this.success = success;
	    }
	    // ... standard getters
	}
```


### Error Handling in Event Listeners
1. Using `@EventListener`'s `mode` attribute
```
	@Component  
	public class UserRegistrationListener {  
		@EventListener(mode = EventListenerMode.ASYNC)  
		public void handleUserRegistrationEvent(UserRegistrationEvent event) {  
			// ... handle event ...  
		}  
	}
```
In this snippet the listener is set to asynchronous and if an exception occurs it won't stop other listener or propagate to the publisher.

2. Using dedicated error handling listener
```
	@Component  
	public class UserRegistrationErrorListener {  
		@EventListener(condition = "#root.cause instanceof SomeException")  
		public void handleUserRegistrationError(DataAccessException exception) {  
			// ... handle error ...  
		}  
	}
```
In this, the ```handleUserRegistrationError``` will be called whenever ```DataAccessException``` is thrown during handling of event.

### Spring Boots Application Event's
1. ApplicationStartingEvent
2. ApplicationEnvironmentPreparedEvent
3. ApplicationInitializedEvent
4. ContextRefereshedEvent
5. WebServletInitializedEvent
6. ApplicationStartedEvent
7. ApplicationReadyEvent
8. ApplicationFailedEvent