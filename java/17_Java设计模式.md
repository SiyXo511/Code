# Java设计模式

本教程将学习Java中的常用设计模式，包括单例、工厂、观察者、策略等模式。

## 1. 单例模式（Singleton）

### 饿汉式

```java
// 饿汉式：类加载时就创建实例
class EagerSingleton {
    private static final EagerSingleton instance = new EagerSingleton();
    
    private EagerSingleton() {
        // 私有构造函数
    }
    
    public static EagerSingleton getInstance() {
        return instance;
    }
}
```

### 懒汉式（线程安全）

```java
// 懒汉式：第一次使用时创建实例
class LazySingleton {
    private static LazySingleton instance;
    
    private LazySingleton() {}
    
    // 线程安全的懒汉式
    public static synchronized LazySingleton getInstance() {
        if (instance == null) {
            instance = new LazySingleton();
        }
        return instance;
    }
}
```

### 双重检查锁定

```java
// 双重检查锁定：既懒加载又线程安全
class DoubleCheckSingleton {
    private static volatile DoubleCheckSingleton instance;
    
    private DoubleCheckSingleton() {}
    
    public static DoubleCheckSingleton getInstance() {
        if (instance == null) {
            synchronized (DoubleCheckSingleton.class) {
                if (instance == null) {
                    instance = new DoubleCheckSingleton();
                }
            }
        }
        return instance;
    }
}
```

### 静态内部类（推荐）

```java
// 静态内部类：既懒加载又线程安全（推荐）
class InnerClassSingleton {
    private InnerClassSingleton() {}
    
    private static class SingletonHolder {
        private static final InnerClassSingleton instance = new InnerClassSingleton();
    }
    
    public static InnerClassSingleton getInstance() {
        return SingletonHolder.instance;
    }
}
```

### 枚举单例（最佳）

```java
// 枚举单例：最简单、线程安全、防止反序列化（Java最佳实践）
enum EnumSingleton {
    INSTANCE;
    
    public void doSomething() {
        System.out.println("单例方法");
    }
}

public class SingletonDemo {
    public static void main(String[] args) {
        EnumSingleton.INSTANCE.doSomething();
    }
}
```

## 2. 工厂模式（Factory）

### 简单工厂

```java
// 产品接口
interface Product {
    void use();
}

// 具体产品
class ConcreteProductA implements Product {
    @Override
    public void use() {
        System.out.println("使用产品A");
    }
}

class ConcreteProductB implements Product {
    @Override
    public void use() {
        System.out.println("使用产品B");
    }
}

// 简单工厂
class SimpleFactory {
    public static Product createProduct(String type) {
        if ("A".equals(type)) {
            return new ConcreteProductA();
        } else if ("B".equals(type)) {
            return new ConcreteProductB();
        }
        throw new IllegalArgumentException("未知的产品类型");
    }
}

public class SimpleFactoryDemo {
    public static void main(String[] args) {
        Product product1 = SimpleFactory.createProduct("A");
        product1.use();
        
        Product product2 = SimpleFactory.createProduct("B");
        product2.use();
    }
}
```

### 工厂方法模式

```java
// 抽象工厂
abstract class Factory {
    public abstract Product createProduct();
    
    public void doSomething() {
        Product product = createProduct();
        product.use();
    }
}

// 具体工厂
class ConcreteFactoryA extends Factory {
    @Override
    public Product createProduct() {
        return new ConcreteProductA();
    }
}

class ConcreteFactoryB extends Factory {
    @Override
    public Product createProduct() {
        return new ConcreteProductB();
    }
}

public class FactoryMethodDemo {
    public static void main(String[] args) {
        Factory factoryA = new ConcreteFactoryA();
        factoryA.doSomething();
        
        Factory factoryB = new ConcreteFactoryB();
        factoryB.doSomething();
    }
}
```

## 3. 观察者模式（Observer）

### 使用Java内置Observer

```java
import java.util.Observable;
import java.util.Observer;

// 主题（Observable）
class NewsAgency extends Observable {
    private String news;
    
    public void setNews(String news) {
        this.news = news;
        setChanged();  // 标记状态改变
        notifyObservers(news);  // 通知观察者
    }
    
    public String getNews() {
        return news;
    }
}

// 观察者
class NewsChannel implements Observer {
    private String name;
    
    public NewsChannel(String name) {
        this.name = name;
    }
    
    @Override
    public void update(Observable o, Object arg) {
        System.out.println(name + " 收到新闻: " + arg);
    }
}

public class ObserverDemo {
    public static void main(String[] args) {
        NewsAgency agency = new NewsAgency();
        
        NewsChannel channel1 = new NewsChannel("频道1");
        NewsChannel channel2 = new NewsChannel("频道2");
        NewsChannel channel3 = new NewsChannel("频道3");
        
        agency.addObserver(channel1);
        agency.addObserver(channel2);
        agency.addObserver(channel3);
        
        agency.setNews("重大新闻：Java 17发布了！");
    }
}
```

### 自定义观察者模式

```java
import java.util.*;

// 观察者接口
interface Observer {
    void update(String message);
}

// 主题接口
interface Subject {
    void attach(Observer observer);
    void detach(Observer observer);
    void notifyObservers(String message);
}

// 具体主题
class ConcreteSubject implements Subject {
    private List<Observer> observers = new ArrayList<>();
    
    @Override
    public void attach(Observer observer) {
        observers.add(observer);
    }
    
    @Override
    public void detach(Observer observer) {
        observers.remove(observer);
    }
    
    @Override
    public void notifyObservers(String message) {
        for (Observer observer : observers) {
            observer.update(message);
        }
    }
}

// 具体观察者
class ConcreteObserver implements Observer {
    private String name;
    
    public ConcreteObserver(String name) {
        this.name = name;
    }
    
    @Override
    public void update(String message) {
        System.out.println(name + " 收到消息: " + message);
    }
}

public class CustomObserver {
    public static void main(String[] args) {
        ConcreteSubject subject = new ConcreteSubject();
        
        subject.attach(new ConcreteObserver("观察者1"));
        subject.attach(new ConcreteObserver("观察者2"));
        subject.attach(new ConcreteObserver("观察者3"));
        
        subject.notifyObservers("状态已更新");
    }
}
```

## 4. 策略模式（Strategy）

```java
// 策略接口
interface SortStrategy {
    void sort(int[] array);
}

// 具体策略
class BubbleSortStrategy implements SortStrategy {
    @Override
    public void sort(int[] array) {
        System.out.println("使用冒泡排序");
        for (int i = 0; i < array.length - 1; i++) {
            for (int j = 0; j < array.length - i - 1; j++) {
                if (array[j] > array[j + 1]) {
                    int temp = array[j];
                    array[j] = array[j + 1];
                    array[j + 1] = temp;
                }
            }
        }
    }
}

class QuickSortStrategy implements SortStrategy {
    @Override
    public void sort(int[] array) {
        System.out.println("使用快速排序");
        Arrays.sort(array);  // 简化实现
    }
}

// 上下文
class Sorter {
    private SortStrategy strategy;
    
    public Sorter(SortStrategy strategy) {
        this.strategy = strategy;
    }
    
    public void setStrategy(SortStrategy strategy) {
        this.strategy = strategy;
    }
    
    public void sortArray(int[] array) {
        strategy.sort(array);
    }
}

public class StrategyDemo {
    public static void main(String[] args) {
        int[] array = {5, 2, 8, 1, 9, 3};
        
        Sorter sorter = new Sorter(new BubbleSortStrategy());
        sorter.sortArray(array);
        
        System.out.println("排序后: " + Arrays.toString(array));
        
        array = new int[]{5, 2, 8, 1, 9, 3};
        sorter.setStrategy(new QuickSortStrategy());
        sorter.sortArray(array);
        
        System.out.println("排序后: " + Arrays.toString(array));
    }
}
```

## 5. 适配器模式（Adapter）

```java
// 目标接口
interface Target {
    void request();
}

// 需要适配的类
class Adaptee {
    public void specificRequest() {
        System.out.println("适配者的特殊请求");
    }
}

// 适配器
class Adapter implements Target {
    private Adaptee adaptee;
    
    public Adapter(Adaptee adaptee) {
        this.adaptee = adaptee;
    }
    
    @Override
    public void request() {
        adaptee.specificRequest();  // 调用适配者的方法
    }
}

public class AdapterDemo {
    public static void main(String[] args) {
        Adaptee adaptee = new Adaptee();
        Target target = new Adapter(adaptee);
        target.request();
    }
}
```

## 6. 装饰器模式（Decorator）

```java
// 组件接口
interface Component {
    void operation();
}

// 具体组件
class ConcreteComponent implements Component {
    @Override
    public void operation() {
        System.out.println("具体组件的操作");
    }
}

// 装饰器基类
abstract class Decorator implements Component {
    protected Component component;
    
    public Decorator(Component component) {
        this.component = component;
    }
    
    @Override
    public void operation() {
        component.operation();
    }
}

// 具体装饰器
class ConcreteDecoratorA extends Decorator {
    public ConcreteDecoratorA(Component component) {
        super(component);
    }
    
    @Override
    public void operation() {
        super.operation();
        System.out.println("装饰器A的额外功能");
    }
}

class ConcreteDecoratorB extends Decorator {
    public ConcreteDecoratorB(Component component) {
        super(component);
    }
    
    @Override
    public void operation() {
        super.operation();
        System.out.println("装饰器B的额外功能");
    }
}

public class DecoratorDemo {
    public static void main(String[] args) {
        Component component = new ConcreteComponent();
        Component decoratedA = new ConcreteDecoratorA(component);
        Component decoratedB = new ConcreteDecoratorB(decoratedA);
        
        decoratedB.operation();
    }
}
```

## 7. 综合示例

### 完整的观察者模式实现

```java
import java.util.*;

// 事件接口
interface Event {
    String getMessage();
}

// 具体事件
class NewsEvent implements Event {
    private String news;
    
    public NewsEvent(String news) {
        this.news = news;
    }
    
    @Override
    public String getMessage() {
        return news;
    }
}

// 事件监听器接口
interface EventListener {
    void onEvent(Event event);
}

// 主题
class EventPublisher {
    private Map<Class<? extends Event>, List<EventListener>> listeners = new HashMap<>();
    
    public <T extends Event> void subscribe(Class<T> eventType, EventListener listener) {
        listeners.computeIfAbsent(eventType, k -> new ArrayList<>()).add(listener);
    }
    
    public <T extends Event> void unsubscribe(Class<T> eventType, EventListener listener) {
        List<EventListener> list = listeners.get(eventType);
        if (list != null) {
            list.remove(listener);
        }
    }
    
    public <T extends Event> void publish(T event) {
        List<EventListener> list = listeners.get(event.getClass());
        if (list != null) {
            for (EventListener listener : list) {
                listener.onEvent(event);
            }
        }
    }
}

// 具体监听器
class NewsChannel implements EventListener {
    private String name;
    
    public NewsChannel(String name) {
        this.name = name;
    }
    
    @Override
    public void onEvent(Event event) {
        System.out.println(name + " 收到新闻: " + event.getMessage());
    }
}

public class EventSystemDemo {
    public static void main(String[] args) {
        EventPublisher publisher = new EventPublisher();
        
        NewsChannel channel1 = new NewsChannel("频道1");
        NewsChannel channel2 = new NewsChannel("频道2");
        
        publisher.subscribe(NewsEvent.class, channel1);
        publisher.subscribe(NewsEvent.class, channel2);
        
        publisher.publish(new NewsEvent("Java 17发布了！"));
        
        publisher.unsubscribe(NewsEvent.class, channel1);
        publisher.publish(new NewsEvent("Java 18即将发布！"));
    }
}
```

## 练习

1. 实现一个线程安全的单例日志类（使用枚举）
2. 使用工厂模式创建一个图形库
3. 实现一个完整的事件系统（观察者模式）
4. 使用策略模式实现多种排序算法
5. 实现一个装饰器模式的文本格式化系统

