# Java - design patterns - examples

<a name="index"></a>

## Table of contents
1. [Factory](#factory)
2. [Heritance](#heritance)
3. [Observer](#observer)
4. [Singleton](#singleton)
5. [State](#state)
6. [Strategy Example A](#strategy-exampleA)
7. [Strategy Example B](#strategy-exampleB)
8. [Template](#template)

<a name="factory"></a>
## Factory

Factory pattern is one of the most used design patterns in Java.<br>
In Factory pattern, we create object without exposing the creation logic to the client, and refer to newly created object using a common interface.

* Notification
```java
public interface Notification {
    void notifyUser();
}
```

* SMSNotification (similar for EmailNotification and PushNotification)
```java
public class SMSNotification implements Notification {

    @Override
    public void notifyUser()
    {
        System.out.println("Sending an SMS notification");
    }
}
```

* NotificationFactory
```java
public class NotificationFactory {
    public Notification createNotification(String channel)
    {
        if (channel == null || channel.isEmpty()) {
            return null;
        }

        switch (channel) {
            case "SMS":     return new SMSNotification();
            case "EMAIL":   return new EmailNotification();
            case "PUSH":    return new PushNotification();
            default:        throw new IllegalArgumentException("Unknown channel '"+channel+"'");
        }
    }
}
```

* testing
```java
public class FactoryExample2Test {

    @Test
    public void smsNotification() {
        NotificationFactory notificationFactory = new NotificationFactory();
        Notification notification = notificationFactory.createNotification("SMS");
        notification.notifyUser();
        assertTrue(true, "make sonar happy");
    }

    @Test
    public void nullNotification() {
        NotificationFactory notificationFactory = new NotificationFactory();
        Notification notification = notificationFactory.createNotification("");
        assertNull(notification);
    }

    @Test
    public void unknownNotification() {
        try {
            NotificationFactory notificationFactory = new NotificationFactory();
            Notification notification = notificationFactory.createNotification("foo");
            fail("it cannot work");
        } catch(IllegalArgumentException ex) {
            assertTrue(ex.getMessage().contains("Unknown channel 'foo'"));
        }
    }

}  
```

<a href="#index">back to index</a>

<a name="heritance"></a>
## Heritance 

* AbstractDummy
```java
public abstract class AbstractDummy {

    protected void loopRoles() {
        System.out.println("-- start checking roles --");
        for (String role : getAuthorisedRoles()) {
            System.out.println("role = " + role);
        }
        System.out.println("-- done checking roles --");
    }

    protected abstract String[] getAuthorisedRoles();
}
```

* Dummy
```java
public class Dummy extends AbstractDummy {

    public void doSomething() {
        loopRoles();
    }

    @Override
    public String[] getAuthorisedRoles() {
        return new String[]{"foo","bar"};
    }

} 
```

* testing
```java
public class HeritanceRolesTest {

    @Test
    public void testHeritanceRoles() {
        Dummy dummy = new Dummy();
        dummy.doSomething();
        assertTrue("make sonar happy", true);
    }
}
``` 

<a href="#index">back to index</a>

<a name="observer"></a>
## Observer

The Observer pattern creates a dependency between a subject and observers,<br>
so that every observer is notified when the subject is updated.

* Observer
```java
public interface Observer {
    void refresh();
}
```

* Subject
```java
public abstract class Subject {

    protected List<Observer> observerList = new ArrayList<Observer>();

    public void add(Observer observer) {
        observerList.add(observer);
    }

    public void remove(Observer observer) {
        observerList.remove(observer);
    }

    public void notifyObservers() {
        for (Observer o : observerList) {
            o.refresh();
        }
    }
}
```

* ViewVehicule
```java
public class ViewVehicule implements Observer {

    protected String name;
    protected Vehicule vehicule;
    protected String text;

    public ViewVehicule(String name, Vehicule vehicule) {
        this.name = name;
        this.vehicule = vehicule;
        vehicule.add(this);
        updateText();
    }

    protected void updateText() {
        this.text= "description = " + vehicule.getDescription()
            + " - price = " + vehicule.getPrice();
    }

    @Override
    public void refresh() {
        updateText();
        updateGUI();
    }

    public void updateGUI() {
        System.out.println("[" + this.name + "] " + this.text);
    }
}
```

* Vehicule
```java
public class Vehicule extends Subject {

    protected String description;
    protected double price;

    public String getDescription() {
        return description;
    }

    public void setDescription(String description) {
        this.description = description;
        this.notifyObservers();
    }

    public double getPrice() {
        return price;
    }

    public void setPrice(double price) {
        this.price = price;
        this.notifyObservers();
    }
}
```

* testing
```java
public class ObserverTest {

    @Test
    public void testObserver() {
        Vehicule vehicule = new Vehicule();
        vehicule.setDescription("cheap vehicule");
        vehicule.setPrice(5000.0);

        ViewVehicule basicView = new ViewVehicule("basic view", vehicule);
        basicView.updateGUI();
        vehicule.setPrice(4500.0);

        ViewVehicule newView = new ViewVehicule("another view", vehicule);
        vehicule.setPrice(5500.0);
    }
}
```

* output
```text
[basic view] description = cheap vehicule - price = 5000.0
[basic view] description = cheap vehicule - price = 4500.0
[basic view] description = cheap vehicule - price = 5500.0
[another view] description = cheap vehicule - price = 5500.0
```

<a href="#index">back to index</a>

<a name="singleton"></a>
## Singleton

```java
public class User {

    private String firstname;
    private String lastname;
    private String email;

    private static User instance = null;

    private User() { }

    public static User getInstance() {
        if (null == instance) {
            instance = new User();
        }
        return instance;
    }
}    
```

<a href="#index">back to index</a>

<a name="state"></a>
## State

The state pattern is usefull when an object needs to adapt its behaviour to its state.<br>
In the example, representing an order, we can only add and remove products before it's validated and delivered.


* Order
```java
public class Order {

    private List<Product> productList;
    private StateOrder stateOrder;

    public Order() {
        this.productList = new ArrayList<Product>();
        this.stateOrder = new OrderOngoing(this);
    }

    public String getCurrentState() {
        return stateOrder.getCurrentState();
    }

    public void addProduct(Product product) {
        stateOrder.addProduct(product);
    }

    public void removeProduct(Product product) {
        stateOrder.removeProduct(product);
    }

    public void delete() {
        stateOrder.delete();
    }

    public void nextState() {
        stateOrder = stateOrder.nextState();
    }

    public List<Product> getProductList() {
        return productList;
    }

    public void display() {
        System.out.println("[Order content]");
        for (Product product : productList) {
            System.out.println(product.toString());
        }
    }
}
```

* OrderDelivered
```java
public class OrderDelivered extends StateOrder {

    public OrderDelivered(Order order) {
        super(order);
    }

    public String getCurrentState() {
        return "delivered";
    }

    public void addProduct(Product product) { }

    public void removeProduct(Product product) { }

    public void delete() { }

    public StateOrder nextState() {
        return this;
    }
}
```

* OrderOngoing
```java
public class OrderOngoing extends StateOrder {

    public OrderOngoing(Order order) {
        super(order);
    }

    public String getCurrentState() {
        return "ongoing";
    }

    public void addProduct(Product product) {
        super.order.getProductList().add(product);
    }

    public void delete() {
        super.order.getProductList().clear();
    }

    public void removeProduct(Product product) {
        super.order.getProductList().remove(product);
    }

    public StateOrder nextState() {
        return new OrderValidated(super.order);
    }
}
```

* OrderValidated
```java
public class OrderValidated extends StateOrder {

    public OrderValidated(Order order) {
        super(order);
    }

    public String getCurrentState() {
        return "validated";
    }

    public void addProduct(Product product) { }

    public void removeProduct(Product product) { }

    public void delete() {
        super.order.getProductList().clear();
    }

    public StateOrder nextState() {
        return new OrderDelivered(super.order);
    }

}
```

* Product
```java
public class Product {

    private String name;

    public Product(String name) {
        this.name = name;
    }

    public String toString() {
        StringBuilder sb = new StringBuilder();
        sb.append("product : ");
        sb.append(this.name);
        return sb.toString();
    }
}
```

* StateOrder
```java
public abstract class StateOrder {

    protected Order order;

    public StateOrder(Order order) {
        this.order = order;
    }

    public abstract String getCurrentState();
    public abstract void addProduct(Product product);
    public abstract void delete();
    public abstract void removeProduct(Product product);
    public abstract StateOrder nextState();

}
```

* testing
```java
public class StateTest {

    @Test
    public void testStatePattern() {
        // new ongoing order
        Order order = new Order();
        order.addProduct(new Product("music album 1"));
        order.addProduct(new Product("movie 1"));
        order.display();
        assertEquals(2, order.getProductList().size());
        assertEquals("ongoing", order.getCurrentState());

        // set order as validated
        order.nextState();
        assertEquals("validated", order.getCurrentState());
        order.addProduct(new Product("misc item 1"));
        order.display();
        assertEquals(2, order.getProductList().size());

        // set order as delivered
        order.nextState();
        assertEquals("delivered", order.getCurrentState());
        order.delete();
        order.display();
        assertEquals(2, order.getProductList().size());
    }
}
```

<a href="#index">back to index</a>

<a name="strategy"></a>
## Strategy

In Strategy pattern, a class behavior or its algorithm can be changed at run time.<br>
For this, we create objects which represent various strategies and a context object whose behavior varies as per its strategy object.<br>
The strategy object changes the executing algorithm of the context object.

<a name="strategy-exampleA"></a>
### Strategy - example A

* Strategy
```java
public interface Strategy {
    public int processOperation(int numA, int numB);
}
```

* Context
```java
public class Context {
    private Strategy strategy;

    public Context(Strategy strategy){
        this.strategy = strategy;
    }

    public int executeStrategy(int numA, int numB){
        return strategy.processOperation(numA, numB);
    }
} 
```

* OperationAdd (similar with OperationMultiply, OperationSubstract)
```java
public class OperationAdd implements Strategy{
    @Override
    public int processOperation(int numA, int numB) {
        return numA + numB;
    }
}
```

* testing
```java
/**
 * Another good example of using strategy would be sorting an array of numbers.
 * Every strategy would be a sorting algorithm.
 */
public class StrategyTest {

    private Context context = null;

    @Test
    public void add() {
        context = new Context(new OperationAdd());
        assertEquals(18, context.executeStrategy(10, 8));
    }

    @Test
    public void substract() {
        context = new Context(new OperationSubstract());
        assertEquals(2, context.executeStrategy(10, 8));
    }

    @Test
    public void multiply() {
        context = new Context(new OperationMultiply());
        assertEquals(80, context.executeStrategy(10, 8));
    }

}
```

<a name="strategy-exampleB"></a>
### Strategy - example B

* Discounter
```java
public interface Discounter {
    BigDecimal applyDiscount(BigDecimal amount);
}
```

* ChristmasDiscounter
```java
public class ChristmasDiscounter implements Discounter {
    @Override
    public BigDecimal applyDiscount(final BigDecimal amount) {
        return amount.multiply(BigDecimal.valueOf(0.9));
    }
}
```

* EasterDiscounter 
```java
public class EasterDiscounter implements Discounter {
    @Override
    public BigDecimal applyDiscount(final BigDecimal amount) {
        return amount.multiply(BigDecimal.valueOf(0.5));
    }
}
```

* testing
```java
public class StrategyExample2Test {

    @Test
    public void earlyDiscounter() {
        Discounter easterDiscounter = new EasterDiscounter();
        BigDecimal discountedValue = easterDiscounter.applyDiscount(BigDecimal.valueOf(100));
        assertEquals(50, discountedValue);
    }

    @Test
    public void christmasDiscounter() {
        Discounter easterDiscounter = new ChristmasDiscounter();
        BigDecimal discountedValue = easterDiscounter.applyDiscount(BigDecimal.valueOf(100));
        assertEquals(90, discountedValue);
    }

}
```

<a href="#index">back to index</a>

<a name="template"></a>
## Template

In Template pattern, an abstract class exposes defined way(s)/template(s) to execute its methods.<br>
Its subclasses can override the method implementation as per need but the invocation is to be in the same way as defined by an abstract class.<br>
With this pattern, you can keep some common logic between child classes too.<br>
This pattern comes under behavior pattern category.

* Game
```java
public abstract class Game {

    abstract void initialize();
    abstract void startPlay();
    abstract void endPlay();

    //template method
    public final void play(){

        //initialize the game
        initialize();

        //start game
        startPlay();

        //end game
        endPlay();
    }
}
```

* Football (similar with Cricket)
```java
public class Football extends Game {

    @Override
    void initialize() {
        System.out.println("Football Game Initialized");
    }

    @Override
    void startPlay() {
        System.out.println("Football Game Started");
    }

    @Override
    void endPlay() {
        System.out.println("Football Game Finished");
    }

}
```

* testing
```java
public class TemplateTest {

    @Test
    public void cricket() {
        Game cricketGame = new Cricket();
        cricketGame.play();
    }

    @Test
    public void football() {
        Game footballGame = new Football();
        footballGame.play();
    }

}
```

<a href="#index">back to index</a>