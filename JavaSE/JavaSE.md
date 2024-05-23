# JavaSE

## 经典代码

```java
List<String> list = new ArrayList<>();//面向接口编程，只能调用ArrayList中实现List中的方法
LinkedList<String> queue = new LinkedList<>();
Map<String, Integer> map = new HashMap<>(); 
//初始化String和数组
String str = new String("Hello World"); // 通过构造函数传入参数进行初始化
int[] arr = new int[5]; // 创建长度为5的整型数组
double[] dArr = new double[]{1.0, 2.0, 3.0}; // 直接指定元素值进行初始化
```

## PriorityQueue

**PriorityQueue< ListNod > pq = new PriorityQueue<>(length, (a, b)->(b.val - a.val)) ; //大顶堆**

- 采用的是堆排序（**不指定Comparator时默认为最小堆**），头是按指定排序方式的最小元素。堆排序只能保证根是最大（最小），整个堆并不是有序的。方法iterator()中提供的迭代器可能只是对整个数组的依次遍历。也就只能保证数组的第一个元素是最小的。
- 队列既可以根据元素的自然顺序来排序，也可以根据 Comparator来设置排序规则。优先队列的头是基于自然排序或者Comparator排序的最小元素。如果有多个对象拥有同样的排序，那么就可能随机地取其中任意一个。当我们获取队列时，返回队列的头对象。
- PriorityQueue类在**Java1.5**中引入并作为Java Collections Framework的一部分。优先队列中的元素可以默认自然排序或者通过提供的Comparator比较器在队列实例化的时排序。
- 该队列是用**数组**实现，但是数组大小可以动态增加，容量无限。但在创建时可以指定初始大小。当我们向优先队列增加元素的时候，队列大小会自动增加。
-  PriorityQueue是非线程安全的，所以Java提供了PriorityBlockingQueue（实现BlockingQueue接口）用于Java多线程环境。
- **offer()、poll()、remove() 、add() 方法**时间复杂度为**O(logn)** ；**remove(Object) 和 contains(Object)** 时间复杂度为**O(n)**；检索方法**（peek、element 和 size）**时间复杂度为常量。
- 方法iterator()中提供的迭代器并不保证以有序的方式遍历优先级队列中的元素。（原因可参考PriorityQueue的内部实现）如果需要按顺序遍历，可用**Arrays.sort(pq.toArray())**。



PriorityQueue<ListNode> pq = new PriorityQueue<>(length, (a, b)->(a.val - b.val));//小顶堆

### add(E e) & offer(E e)

都是向优先队列中插入元素，只是Queue接口规定二者对插入失败时的处理不同，前者在插入失败时抛出异常，后则则会返回false。

### element() & peek()

- 都是**获取但不删除队首元素**，也就是队列中权值最小的那个元素，二者唯一的区别是当方法失败时前者抛出异常，后者返回null。
- 根据小顶堆的性质，堆顶那个元素就是全局最小的那个；由于堆用数组表示，根据下标关系，0下标处的那个元素既是堆顶元素。所以直接返回数组0下标处的那个元素即可。

### remove() & poll()

都是**获取并删除队首元素**，区别是当方法失败时前者抛出异常，后者返回null。

### remove(Object o)

方法用于删除队列中跟o相等的某一个元素（如果有多个相等，只删除一个），该方法不是Queue接口内的方法，而是Collection接口的方法。由于删除操作会改变队列结构，所以要进行调整；又由于删除元素的位置可能是任意的，所以调整过程比其它函数稍加繁琐。
具体来说，remove(Object o)可以分为2种情况：

- 删除的是最后一个元素。直接删除即可，不需要调整。
- 删除的不是最后一个元素，从删除点开始以最后一个元素为参照调用一次siftDown()即可。此处不再赘述。



## 基本数据类型

8种：byte、short、int、long、float、double、boolean、char

整数型：byte（8位1字节）、short（16位2字节）、int（32位4字节）、long（64位8字节）后缀L

浮点型：float（32位4字节）后缀F、double（64位8字节）**避免使用浮点数进行比较，用BigDecimal**

布尔型：boolean（8位1字节）

字符型：char（16位2字节）

二进制前缀**0b** 八进制前缀**0** 十六进制前缀**0x**

## 输入Scanner

```java
Scanner scanner = new Scanner(System.in);
//

scanner.close();
```

## 值传递和引用传递

**值传递**
传递对象的一个副本，即使副本被改变，也不会影响源对象，因为值传递的时候，实际上是将实参的值复制一份给形参。

**引用传递**
传递的并不是实际的对象，而是对象的引用，外部对引用对象的改变也会反映到源对象上，因为引用传递的时候，实际上是将实参的**地址值**复制一份给形参。引用类型的参数传输的是存储的地址值。

对象传递（数组、类、接口）是引用传递，原始类型数据（整形、浮点型、字符型、布尔型）传递是值传递。

## 可变参数

是一种特殊形参，定义在方法，构造器的形参列表里，格式：`数据类型...参数名称`

- 可以不传数据给它，可以传一个或者同时传多个数据给它，也可以传一个数组给它
- 可变参数在方法内部就是一个数组，一个形参列表中可变参数只能有一个，可变参数必须放在形参列表的最后面

## 方法重载

1. 方法名必须相同
2. 参数列表必须不同(参数的个数不同、参数的类型不同、类型的次序必须不同)
3. 与返回值类型是否相同无关,不影响重载

## Java内存分析*

堆：存放new的对象和数组（包括对象的成员变量），可以被所有的线程共享

栈：存放基本变量类型（包含这个基本类型的具体数值），引用对象的变量（会存放这个引用在堆里面的具体地址），方法运行时所进入的内存

方法区：可以被所有的线程共享，包含了所有的class和static变量，放class文件的

## 类与对象

- 成员变量本身存在默认值，整型是0，浮点型是0.0，引用类型是null，布尔型是false，不需要赋初始值
- 堆内存中的对象没有被任何变量引用时，就会被判定为内存中的“垃圾”。java存在自动垃圾回收机制，会自动清除掉垃圾对象。
- this指针就是一个变量，可以用在方法中，拿到当前对象。用来解决对象的成员变量与方法内部变量的名称一样时，导致访问冲突问题的
- 一个代码文件中可以写多个class类，但是只能用一个public修饰，且public修饰的类名必须和代码文件同名

## 构造器

一旦定义了有参构造器，Java不会自动生成无参构造器，建议自己手写一个无参构造器

## 封装

合理隐藏，合理暴露。成员变量私有，再使用`set`和`get`方法操作成员变量

## 实体类（javabean）

- 类中成员变量都要私有，并且要对外提供相应的`get`和`set`方法

- 类中必须有一个公共的无参的构造器

- 实体类只负责数据存取，而对数据的处理交给其他类来完成，以实现数据和业务处理相分离


## 包

- 同一个包下的程序，可以直接访问。访问其他包下的程序，必须导包才能访问
- Java.lang包下的程序不需要导包可以直接使用
- 访问多个其他包下的程序，名字相同的情况下，默认只能导一个程序，另一个必须带包名和类名来访问

## String

![1662609378727](JavaSEimages\1662609378727.png)

- 只要是以`“”`方式写出的字符串对象，会在堆内存中的**字符串常量池**中存储，且相同内容的字符串只存储一份。不包含变量+运算生成的字符串对象会被编译优化直接变为字符串放进**字符串常量池**
- 但通过`new`方式创建字符串对象，每new一次都会产生一个新的对象放在堆内存中。通过包含变量+运算生成的字符串对象也会放在堆内存中
- 通过print打印出来的不是string的地址，而是数据
- String重写了equals方法，比较的是内容不是地址
- public boolean matches(String regex) 判断字符串是否匹配**正则表达式**，匹配返回true，不匹配返回false

## static

- 类变量：有static修饰，属于类，被类的全部对象共享，在内存中只有一份，一般用public修饰，只需要通过类名就可以调用：**`类名.静态变量`**。访问自己类中的类变量，可以省略类名不写，访问其他类里的类变量必须带类名访问

- 实例变量（对象的变量）：无static修饰，属于每个对象，需要通过对象才能调用：**`对象.实例变量`**

- 有static修饰的方法，是属于类的，称为**类方法**；调用时直接用类名调用即可。main方法就是一个类方法，执行Java文件时，jvm会直接调用`类名.main()`

  如果一个类中的方法全都是静态的，那么这个类中的方法就全都可以被类名直接调用，由于调用起来非常方便，就像一个工具一下，所以把这样的类就叫做**工具类**。工具类里的方法全都是静态的，推荐用类名调用为了防止使用者用对象调用。我们可以把工具类的构造方法私有化。

- 类方法中可以直接访问类的成员，不可以直接访问实例成员

- 实例方法中既可以直接访问类成员，也可以直接访问实例成员

- 实例方法中可以出现this关键字，类方法中不可以出现this关键字

## Random

Random类位于`java.util`包中，因此在使用前需要先导入该包`import java.util.Random;`

- 如果需要生成指定范围内的随机整数，可以使用`nextInt(int bound)`方法。该方法会生成一个从0到bound-1之间的随机整数（左闭右开）
- Random类提供了`nextDouble()`方法来生成一个0.0到1.0之间的随机浮点数

如果需要生成指定范围内的随机浮点数，可以使用如下公式：

```java
double min = 0.0;
double max = 1.0;
double randomNumber = min + (max - min) * random.nextDouble();
```

## 代码块

- 静态代码块`static{}`：随着类的加载而执行，而且**只执行一次**。可以完成类的初始化，例如对类变量初始化赋值
- 实例代码块`{}`：实例代码块的作用和构造器的作用是一样的，用来给对象初始化值（不建议这样用）；而且**每次创建对象之前都会先执行**实例代码块。如果在有参无参或多个构造器中重复出现的代码可以放到实例代码块中。

## 设计模式*

一类问题可能会有多种解决方案，而设计模式是在编程实践中，多种方案中的一种最优方案。

### 单例设计模式

确保一个类只有一个对象

应用场景：任务管理器对象、获取运行时对象。在这些业务场景下使用单例模式可以避免浪费内存





#### 饿汉式单例

在获取类的对象时，对象已经创建好了

把类的构造器私有，定义一个类变量记住类的一个对象，定义一个类方法，返回对象

```java
public class classA{
    private static classA a =new classA();
    private classA(){
    }
    public static classA getObject(){
        return a;
    }
}
```

可以使用枚举实现单例

```java
public enum classA{
    X;
}
```



#### 懒汉式单例

拿对象时，才开始创建对象

把类的构造器私有，定义一个类变量用于存储对象，提供一个类方法，保证返回的是同一个对象

```java
public class classB{
    private static classB b;
    private classB(){
    }
    public static classB getInstance(){
        if(b == null){
            b =  new classB();
        }
        return b;
    }
}
```

### 模板方法设计模式

 模板方法模式主要解决方法中存在重复代码的问题

第1步：定义一个抽象类，把子类中相同的代码写成一个模板方法，建议使用final修饰模板方法。
第2步：把模板方法中不能确定的代码写成抽象方法，并在模板方法中调用。
第3步：子类继承抽象类，只需要重写父类抽象方法就可以了



## 继承extends

子类继承父类的非私有成员，继承可以提高代码的复用性

### 权限修饰符

![](JavaSEimages\1664012151488.png)

- protected只在**子类**中可以访问，**子类对象**不可访问，即`子类对象.父类protected方法`不行
- 在子类中访问其他成员（成员变量、成员方法），是依据就近原则的。特别想访问子类成员变量或方法用`this`，访问父类成员变量或方法用`super`

### 单继承

- Java语言只支持单继承，不支持多继承，但是可以多层继承。

### 方法重写

- 重写的方法上面，可以加一个注解`@Override`，用于标注这个方法是复写的父类方法

- 子类复写父类方法时，访问权限必须大于或者等于父类方法的权限public > protected > 缺省
- 重写的方法返回值类型，必须与被重写的方法返回值类型一样，或者范围更小
- 私有方法、静态方法不能被重写，如果重写会报错。
- 实际上我们实际写代码时，只要和父类写的一样就可以（ 总结起来就8个字：**声明不变，重新实现**）
- Object类中有一个toString()方法，直接通过Student对象调用Object的toString()方法，会得到对象的地址值。子类**重写Object的toString()方法**，以便返回对象的内容。

### 子类构造器的特点

- 子类全部构造器，都会先调用父类无参构造器，再执行自己。 
- 如果不想使用默认的`super()`方式调用父类构造器，还可以手动使用`super(参数)`调用父类有参数构造器。
- this和super访问构造方法，只能用到构造方法的第一句，否则会报错。

```java
访问本类成员：
	this.成员变量	//访问本类成员变量
	this.成员方法	//调用本类成员方法
	this()		   //调用本类空参数构造器
    this(参数)	  //调用本类有参数构造器
	
访问父类成员：
	super.成员变量	//访问父类成员变量
	super.成员方法	//调用父类成员方法
	super()		   //调用父类空参数构造器
    super(参数)	  //调用父类有参数构造器
```

## 多态

- 多态是在继承、实现情况下的一种现象，表现为：对象多态、行为多态。java中的属性（成员变量）不谈多态
- 在多态形式下，不能调用子类特有的方法，但是把父类变量转换为子类类型后是可以调用的。如果类型转换错了，就会出现类型转换异常ClassCastException，原本是什么类型，才能还原成什么类型

```java
//如果p接收的是子类对象
if(父类变量 instance 子类){
    //则可以将p转换为子类类型
    子类 变量名 = (子类)父类变量;
}
```

## final

-  final修饰类：该类称为最终类，特点是不能被继承

- final修饰方法：该方法称之为最终方法，特点是不能被重写。
- final修饰变量：该变量只能被赋值一次。修饰基本类型的变量，变量存储的数据不能被改变。修饰引用类型的变量，变量存储的地址不能被改变，但地址指向的对象的内容可以改变。

## 常量

- 被 static final 修饰的成员变量，称之为常量。为了方便在其他类中被访问所以一般还会加上public修饰符。建议都采用大写字母命名，多个单词之前有_隔开。由于常量是static的所以，在使用时直接用类名就可以调用
- 在程序编译后，常量会“宏替换”，出现常量的地方，全都会被替换为其记住的字面量。把代码反编译后，其实代码是下面的样子

## 抽象

- 被abstract修饰的类，就是抽象类，被abstract修饰的方法，就是抽象方法（不允许有方法体）
- 抽象类不能创建对象，但是它可以作为父类让子类继承。而且子类继承父类必须重写父类的所有抽象方法。
- 抽象类中可以不写抽象方法，但有抽象方法的类一定是抽象类
- 类有的成员（成员变量、方法、构造器）抽象类都具备
- 一个类继承抽象类，必须重写完抽象类的全部抽象方法，否则这个类也必须定义成抽象类
- 父类知道每个子类都要做某个行为，但每个子类要做的情况不一样，父类就定义成抽象方法，交给子类去重写实现

## 接口

- 接口中的成员变量默认为`public static final`修饰的常量，成员方法为`public abstract`修饰的抽象方法

- 接口是用来被类实现（implements）的，我们称之为实现类。

- 一个类是可以实现多个接口的（接口可以理解成干爹），类实现接口必须重写所有接口的全部抽象方法，否则这个类也必须是抽象类

- 接口弥补了单继承的不足，同时可以轻松实现在多种业务场景之间的切换。

  ```java
  //了解即可
  1.一个接口继承多个接口，如果多个接口中存在相同的方法声明，则此时不支持多继承
  2.一个类实现多个接口，如果多个接口中存在相同的方法声明，则此时不支持多实现
  3.一个类继承了父类，又同时实现了接口，父类中和接口中有同名的默认方法，实现类会有限使用父类的方法
  4.一个类实现类多个接口，多个接口中有同名的默认方法，则这个类必须重写该方法。
  ```

  

### 接口jdk8新特性

- 默认方法：必须使用default修饰，默认会被public修饰，可以包含方法体，属于实例方法，必须使用实现类的对象来访问。
- 私有方法(JDK 9开始才支持的)：必须使用private修饰，可以包含方法体，只有接口中的其他私有方法或者默认方法才能访问
- 静态方法：必须使用static修饰，默认会被public修饰，类方法，通过`接口.静态方法`调用

## 内部类

### 成员内部类

- 创建对象格式：`外部类.内部类 变量名 = new 外部类().new 内部类();`
- 既可以访问内部类成员、也可以访问外部类成员。如果内部类成员和外部类成员同名，访问外部类可以使用`类名.this.成员`

### 静态内部类

- 创建对象格式：`外部类.内部类 变量名 = new 外部类.内部类();`
- 静态内部类可以访问外部类的静态变量，不能访问外部类的实例成员

### 匿名内部类*

- 匿名内部类是一种特殊的局部内部类；所谓匿名，指的是程序员不需要为这个类声明名字。
- 创建对象格式：

```java
new 父类/接口(参数值){
    @Override
    重写父类/接口的方法;
}
```

- 匿名内部类本质上是一个没有名字的子类对象、或者接口的实现类对象。编译后系统会为自动为匿名内部类生产字节码，字节码的名称会以`外部类$1.class`的方法命名
- 只有在调用方法时，当方法的形参是一个接口或者抽象类，为了**简化代码**书写，而直接传递匿名内部类对象给方法。

```java
public class Test{
    public static void main(String[] args){
        //方式一
        Swimming s1 = new Swimming(){
            public void swim(){
                System.out.println("狗刨飞快");
            }
        };
        go(s1);
        //更简洁的方式二
        go(new Swimming(){
            public void swim(){
                System.out.println("猴子游泳也还行");
            }
        });
    }
    //形参是Swimming接口，实参可以接收任意Swimming接口的实现类对象
    public static void go(Swimming s){
        System.out.println("开始~~~~~~~~");
        s.swim();
        System.out.println("结束~~~~~~~~");
    }
    public interface Swimming{
    	public void swim();
	}
}
```

## 枚举

- 枚举是一种特殊的类，枚举项就表示枚举类的对象，只是这些对象在定义枚举类时就预先写好了，以后就只能用这几个固定的对象。它的格式是：


```java
public enum 枚举类名{
    枚举项1,枚举项2,枚举项3;
}
```

- 想要获取枚举类中的枚举项，只需要用类名调用 `类名 变量名 = 类名.枚举项`
- 枚举的第一行只能罗列一些名称，这些名称都是**常量**，并且每个常量记住的都是枚举类的一个对象
- 枚举都是最终类，不能被继承
- 枚举的应用场景：枚举一般表示几个固定的值，然后作为参数进行传输。

## 泛型

在编译阶段可以避免出现一些非法的数据，把具体的数据类型传递给类型变量。

### 自定义泛型类

在静态成员中不能使用泛型类定义的类型参数，但我们可以将静态成员方法定义为一个泛型方法。

```java
//这里的<T,W>其实指的就是类型变量，可以是一个，也可以是多个。
public class 类名<T,W>{
    
}
//也可以声明的类型变量继承于某个类
public class 类名<E extends ClassA>{
    
}
```

接下来，我们自己定义一个MyArrayList<E>泛型类，模拟一下自定义泛型类的使用。注意这里重点仅仅只是模拟泛型类的使用，所以方法中的一些逻辑是次要的，也不会写得太严谨。

```java
//定义一个泛型类，用来表示一个容器
//容器中存储的数据，它的类型用<E>先代替用着，等调用者来确认<E>的具体类型。
public class MyArrayList<E>{
    private Object[] array = new Object[10];
    //定一个索引，方便对数组进行操作
    private int index;
    
    //添加元素
    public void add(E e){
        array[index]=e;
        index++;
    }
    
    //获取元素
    public E get(int index){
        return (E)array[index];
    }
}
```

### 自定义泛型接口

```java
//这里的类型变量，一般是一个字母，比如<E>
public interface 接口名<类型变量>{
    
}
```

比如，我们现在要做一个系统要处理学生和老师的数据，需要提供2个功能，保存对象数据、根据名称查询数据，要求：这两个功能处理的数据既能是老师对象，也能是学生对象。

首先我们得有一个学生类和老师类

```java
public class Teacher{}
public class Student{}
```

我们定义一个`Data<T>`泛型接口，T表示接口中要处理数据的类型。

```java
public interface Data<T>{
    public void add(T t);
    
    public ArrayList<T> getByName(String name);
}
```

接下来，我们写一个处理Teacher对象的接口实现类

```java
//此时确定Data<E>中的E为Teacher类型，
//接口中add和getByName方法上的T也都会变成Teacher类型
public class TeacherData implements Data<Teacher>{
   	public void add(Teacher t){
        
    }
    
    public ArrayList<Teacher> getByName(String name){
        
    }
}
```

接下来，我们写一个处理Student对象的接口实现类

```java
//此时确定Data<E>中的E为Student类型，
//接口中add和getByName方法上的T也都会变成Student类型
public class StudentData implements Data<Student>{
   	public void add(Student t){
        
    }
    
    public ArrayList<Student> getByName(String name){
        
    }
}
```

### 泛型方法

- 在返回值类型和修饰符之间有<T>定义的是泛型方法

```java
public <泛型变量,泛型变量> 返回值类型 方法名(形参列表){
    
}
public static <T> void test(T t){
    return t;
}
public static <T extends classA> void test(T t){
    return t;
}
```

- 泛型通配符'?' ，可以在使用泛型的时候代表一切类型，ETKV是在定义泛型的时候使用
- 泛型上限 &lt;? extends Car &gt;  ?能接收的必须是Car或者其子类
- 泛型上限 &lt;? super Car &gt;  ?能接收的必须是Car或者其父类
- 泛型只能编译阶段有效，一旦编译成字节码，字节码中是不包含泛型的
- 泛型只支持引用数据类型，不支持基本数据类型。可使用对应的包装类代替

## Object类

### toString()

- 调用toString()方法可以返回对象的字符串表示形式。
      默认的格式是：“`包名.类名@哈希值16进制`”
- 直接输出对象默认调用toString方法
- Student重写toString方法，返回对象的属性值

```java
public class Student{
    private String name;
    private int age;
    
    public Student(String name, int age){
        this.name=name;
        this.age=age;
    }
    
    @Override
    public String toString(){
        return "Student{name=‘"+name+"’, age="+age+"}";
    }
}
```

### equals()

- equals本身也是比较对象的地址，和"=="没有区别

- Student重写toString方法，比较对象的属性值

```java
//重写equals方法，按照对象的属性值进行比较
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;

        Student student = (Student) o;

        if (age != student.age) return false;
        return name != null ? name.equals(student.name) : student.name == null;
    }
```

### clone()

- 克隆当前对象，返回一个新对象
- 想要调用clone()方法，必须让被克隆的类实现Cloneable接口

#### 浅克隆

拷贝出来的对象封装的数据与原对象封装的数据一模一样（引用类型拷贝的是地址值）

```java
@Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
}
```

#### 深克隆

- 对象中基本数据类型的数据直接拷贝
- 对象中字符串数据拷贝的还是地址（因为字符串存在堆中的字符串常量池中）
- 对象中还包含其他对象，不会拷贝地址，会创建新对象
- 数组的克隆是深克隆

## Objects工具类

### Objects.equals() 

s1是null的时候调用s1.equals(s2)会出NullPointerException异常，调用者不能为null，使用Objects.equals(s1,s2)不会有NullPointerException异常，底层会自动先判断空

### Objects.isNull()

判断对象是否为null，等价于==

### Objects.nonNull()

判断对象是否不为null，等价于!=

## 基本类型包装类

| 基本数据类型 | 对应的包装类（引用数据类型） |
| ------------ | ---------------------------- |
| byte         | Byte                         |
| short        | Short                        |
| **int**      | **Integer**                  |
| long         | Long                         |
| **char**     | **Character**                |
| float        | Float                        |
| double       | Double                       |
| boolean      | Boolean                      |

- 自动装箱：自动把基本类型的数据转换成对象

```java
//1.创建Integer对象，封装基本类型数据10
Integer a = new Integer(10);//已过时，不推荐使用

//2.使用Integer类的静态方法valueOf(数据)
Integer b = Integer.valueOf(10);//推举使用

//3.还有一种自动装箱的写法（意思就是自动将基本类型转换为引用类型）
Integer c = 10;
```

- 自动拆箱：自动把包装类型的对象转换成基本类型

```java
//4.有装箱肯定还有拆箱（意思就是自动将引用类型转换为基本类型）
int d = c;
```

- 包装类数据类型转换

```java
//1.把基本类型的数据转换为字符串
Integer a =23;
String r1 = Integer.toString(a);
String r2 = a.toString();
String r3 = a + "";
//2.把字符串转换为基本数据类型
String Str = "29";
int a = Integer.parseInt(str);//不推荐使用
int b = Integer.valueof(Str);
```

## StringBuilder

StringBuilder比String更合适做字符串的修改操作，效率更高，代码也更加简洁

![](JavaSEimages\Stringbuilder.PNG)

## StringBuffer

- StringBuffer的用法与StringBuilder是一模一样的
- 但是StringBuilder是线程不安全的，StringBuffer是线程安全的

## StringJoiner

StringJoiner号称是拼接神器，不仅效率高，而且代码简洁（jdk8以后才支持）

![](JavaSEimages\stringjoiner.PNG)

```java
//参数1：间隔符 参数2：开头 参数3：结尾
StringJoiner s1 = new StringJoiner(",",[","]");
```

## Math

```java
// 1、public static int abs(int a)：取绝对值（拿到的结果一定是正数）
// 2、public static double ceil(double a): 向上取整
// 3、public static double floor(double a): 向下取整
// 4、public static long round(double a)：四舍五入  
// 5、public static int max(int a, int b)：取较大值
//    public static int min(int a, int b)：取较小值
// 6、 public static double pow(double a, double b)：取次方
// 7、public static double random()： 取随机数 [0.0 , 1.0) (包前不包后)
```

## System

```java
// 1、public static void exit(int status):
//   终止当前运行的Java虚拟机。
//   该参数用作状态代码; 按照惯例，非零状态代码表示异常终止。
     System.exit(0); // 人为的终止虚拟机。(不要使用)

// 2、public static long currentTimeMillis():
//   获取当前系统的时间
//   返回的是long类型的时间毫秒值：指的是从1970-1-1 0:0:0开始走到此刻的总的毫秒值，1s = 1000ms

System.arraycopy(Object src, int srcPos, Object dest, int destPos, int length)//原数组和目标数组可以是一个
             //    原数组   原数组起始位置    目标数组   目标数组起始位置   拷贝长度

```

## Runtime

单例类，只能创建一个对象

```java
// 1、public static Runtime getRuntime() 返回与当前Java应用程序关联的运行时对象。
        Runtime r = Runtime.getRuntime();

// 2、public void exit(int status) 终止当前运行的虚拟机,该参数用作状态代码; 
        r.exit(0);

// 3、public int availableProcessors(): 获取虚拟机能够使用的处理器数。
        System.out.println(r.availableProcessors());

// 4、public long totalMemory() 返回Java虚拟机中的内存总量。
        System.out.println(r.totalMemory()/1024.0/1024.0 + "MB");

// 5、public long freeMemory() 返回Java虚拟机中的可用内存量
        System.out.println(r.freeMemory()/1024.0/1024.0 + "MB");

// 6、public Process exec(String command) 启动某个程序，并返回代表该程序的对象。
		r.exec("D:\\soft\\XMind\\XMind.exe");
```

## BigDecimal

为了解决计算精度损失的问题，Java给我们提供了BigDecimal类，它提供了一些方法可以对数据进行四则运算，而且不丢失精度，同时还可以保留指定的小数位。

```java
// 1、把浮点型数据封装成BigDecimal对象，再来参与运算。
// b、public BigDecimal(String val)  得到的BigDecimal对象是可以精确计算浮点型数据的。 可以使用。

// c、public static BigDecimal valueOf(double val): 通过这个静态方法得到的BigDecimal对象是可以精确运算的。是最好的方案。
	BigDecimal a1 = BigDecimal.valueOf(a);

// 2、public BigDecimal add(BigDecimal augend): 加法
// 3、public BigDecimal subtract(BigDecimal augend): 减法
// 4、public BigDecimal multiply(BigDecimal augend): 乘法

// 5、public BigDecimal divide(BigDecimal b): 除法
    BigDecimal c4 = a1.divide(b1);
// 6、public BigDecimal divide(另一个BigDecimal对象，精确几位，舍入模式) : 除法，可以设置精确几位。
    BigDecimal d3 = d1.divide(d2,  2, RoundingMode.HALF_UP); // 0.33
       
// 7、public double doubleValue() : 把BigDecimal对象又转换成double类型的数据。
    double db1 = d3.doubleValue();
```

## Date

![](C:\Users\Untaboo\Desktop\javasemd\JavaSEimages\1667399443159.png)

```java
// 1、创建一个Date的对象：代表系统当前时间信息的。
    Date d = new Date();

// 2、拿到时间毫秒值。
    long time = d.getTime();

// 3、把时间毫秒值转换成日期对象： 2s之后的时间是多少。
    time += 2 * 1000;
    Date d2 = new Date(time);

// 4、直接把日期对象的时间通过setTime方法进行修改
    Date d3 = new Date();
    d3.setTime(time);
```

## SimpleDateFormat

![](JavaSEimages\1667399804244.png)

创建SimpleDateFormat对象时，在构造方法的参数位置传递日期格式，而日期格式是由一些特定的字母拼接而来的。

| 字母 | 含义 |
| ---- | ---- |
| yyyy | 年   |
| MM   | 月   |
| dd   | 日   |
| HH   | 时   |
| mm   | 分   |
| ss   | 秒   |
| SSS  | 毫秒 |

"2022年12月12日" 的格式是 "yyyy年MM月dd日"
"2022-12-12 12:12:12" 的格式是 "yyyy-MM-dd HH:mm:ss"

```java
// 2、格式化日期对象，和时间 毫秒值。
SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss EEE a");
String rs = sdf.format(d);
String rs2 = sdf.format(time);

String dateStr = "2022-12-12 12:12:11";
// 1、创建简单日期格式化对象 , 指定的时间格式必须与被解析的时间格式一模一样，否则程序会出bug.
SimpleDateFormat sdf2 = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
Date d2 = sdf2.parse(dateStr);
```

## Calendar

![](JavaSEimages\1667400365583.png)

```java
// 1、得到系统此刻时间对应的日历对象。
Calendar now = Calendar.getInstance();

// 2、获取日历中的某个信息
int year = now.get(Calendar.YEAR);
int days = now.get(Calendar.DAY_OF_YEAR);
       
// 3、拿到日历中记录的日期对象。
Date d = now.getTime();
        
// 4、拿到时间毫秒值
long time = now.getTimeInMillis();
       
// 5、修改日历中的某个信息
now.set(Calendar.MONTH, 9); // 修改月份成为10月份。
now.set(Calendar.DAY_OF_YEAR, 125); // 修改成一年中的第125天。
now.set(2026, 11, 22);

// 6、为某个信息增加或者减少多少
now.add(Calendar.DAY_OF_YEAR, 100);
now.add(Calendar.DAY_OF_YEAR, -10);
now.add(Calendar.DAY_OF_MONTH, 6);
now.add(Calendar.HOUR, 12);
```

## DateTimeFormatter(JDK8)

```java
ofPattern(String pattern)：静态方法 ，返回一个指定字符串格式的DateTimeFormatter

format(TemporalAccessor t) ：格式化一个日期、时间，返回字符串
注：LocalDate、LocalTime、LocalDateTime继承自TemporalAccessor

parse(CharSequence text) ：将指定格式的字符序列解析为一个日期、时间
    
    
DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd hh:mm:ss");
//格式化：日期-->字符串
LocalDateTime now = LocalDateTime.now();
String str1 = formatter.format(now);
System.out.println(now);//2021-07-21T19:17:34.356
System.out.println(str1);//2021-07-21 07:17:34

//解析:字符串-->日期
TemporalAccessor parse = formatter.parse("2021-07-21 07:17:34");
```

## LocalDate (JDK8)

传统的时间类（Date、SimpleDateFormat、Calendar）

1. 设计不合理，使用不方便，很多都被淘汰了。
2. 都是可变对象，修改后会丢失最开始的时间信息。
3. 线程不安全。
4. 不能精确到纳秒，只能精确到毫秒。

```java
//转换为String
parse(CharSequence text)//将字符串转换为LocalDate,字符串的格式必须为yyyy-MM-dd 10位长度的日期格式，否则会报错,字符串必须是一个合法的日期，否则会报错
LocalDate l = LocalDate.parse("2021-01-29");
System.out.println(l); //2021-01-29
    
parse(CharSequence text, DateTimeFormatter formatter)//将text字符串转换为formatter格式，text的格式必须与formatter格式一致，如text为yyyyMMdd格式,则formatter也应该为yyyyMMdd格式,否则会报错
LocalDate l = LocalDate.parse("2021-11-29", DateTimeFormatter.ofPattern("yyyy-MM-dd"));
LocalDate localDate1 = LocalDate.parse("20211129", DateTimeFormatter.ofPattern("yyyyMMdd"));
System.out.println(l);
System.out.println(localDate1);
```



```java
// 0、获取本地日期对象(不可变对象)
LocalDate ld = LocalDate.now(); // 年 月 日
        
// 1、获取日期对象中的信息
int year = ld.getYear(); // 年
int month = ld.getMonthValue(); // 月(1-12)
int day = ld.getDayOfMonth(); // 日
int dayOfYear = ld.getDayOfYear();  // 一年中的第几天
int dayOfWeek = ld.getDayOfWeek().getValue(); // 星期几

// 2、直接修改某个信息: withYear、withMonth、withDayOfMonth、withDayOfYear
LocalDate ld2 = ld.withYear(2099);
LocalDate ld3 = ld.withMonth(12);

// 3、把某个信息加多少: plusYears、plusMonths、plusDays、plusWeeks
LocalDate ld4 = ld.plusYears(2);
LocalDate ld5 = ld.plusMonths(2);

// 4、把某个信息减多少：minusYears、minusMonths、minusDays、minusWeeks
LocalDate ld6 = ld.minusYears(2);
LocalDate ld7 = ld.minusMonths(2);

// 5、获取指定日期的LocalDate对象： public static LocalDate of(int year, int month, int dayOfMonth)
LocalDate ld8 = LocalDate.of(2099, 12, 12);
LocalDate ld9 = LocalDate.of(2099, 12, 12);

// 6、判断2个日期对象，是否相等，在前还是在后： equals isBefore isAfter
System.out.println(ld8.equals(ld9));// true
System.out.println(ld8.isAfter(ld)); // true
System.out.println(ld8.isBefore(ld)); // false
```

**LocalDate -> String**

LocalDate类有一个format()方法，可以将日期转成字符串。format()方法需要一个DateTimeFormatter对象作为参数。以下代码示例中，我们将日期对象转换为字符串。

```
String dateStr = LocalDate.now().format(DateTimeFormatter.ofPattern("yyyy-MM-dd"));
System.out.println("当前字符串日期：" + dateStr);
```

**String -> LocalDate**

我们可以使用parse()方法从字符串中解析日期对象

```
LocalDate date = LocalDate.parse(dateStr);System.out.println("日期对象：" + date);
```



## LocalTime(JDK8)

```java
// 0、获取本地时间对象
LocalTime lt = LocalTime.now(); // 时 分 秒 纳秒 不可变的

// 1、获取时间中的信息
int hour = lt.getHour(); //时
int minute = lt.getMinute(); //分
int second = lt.getSecond(); //秒
int nano = lt.getNano(); //纳秒

// 2、修改时间：withHour、withMinute、withSecond、withNano
LocalTime lt3 = lt.withHour(10);
LocalTime lt4 = lt.withMinute(10);
LocalTime lt5 = lt.withSecond(10);
LocalTime lt6 = lt.withNano(10);

// 3、加多少：plusHours、plusMinutes、plusSeconds、plusNanos
LocalTime lt7 = lt.plusHours(10);
LocalTime lt8 = lt.plusMinutes(10);
LocalTime lt9 = lt.plusSeconds(10);
LocalTime lt10 = lt.plusNanos(10);

// 4、减多少：minusHours、minusMinutes、minusSeconds、minusNanos
LocalTime lt11 = lt.minusHours(10);
LocalTime lt12 = lt.minusMinutes(10);
LocalTime lt13 = lt.minusSeconds(10);
LocalTime lt14 = lt.minusNanos(10);

// 5、获取指定时间的LocalTime对象：
// public static LocalTime of(int hour, int minute, int second)
LocalTime lt15 = LocalTime.of(12, 12, 12);
LocalTime lt16 = LocalTime.of(12, 12, 12);

// 6、判断2个时间对象，是否相等，在前还是在后： equals isBefore isAfter
System.out.println(lt15.equals(lt16)); // true
System.out.println(lt15.isAfter(lt)); // false
System.out.println(lt15.isBefore(lt)); // true
```

## LocalDateTime(JDK8)

```java
// 0、获取本地日期和时间对象。
LocalDateTime ldt = LocalDateTime.now(); // 年 月 日 时 分 秒 纳秒

// 1、可以获取日期和时间的全部信息
int year = ldt.getYear(); // 年
int month = ldt.getMonthValue(); // 月
int day = ldt.getDayOfMonth(); // 日
int dayOfYear = ldt.getDayOfYear();  // 一年中的第几天
int dayOfWeek = ldt.getDayOfWeek().getValue();  // 获取是周几
int hour = ldt.getHour(); //时
int minute = ldt.getMinute(); //分
int second = ldt.getSecond(); //秒
int nano = ldt.getNano(); //纳秒

// 2、修改时间信息：
// withYear withMonth withDayOfMonth withDayOfYear withHour
// withMinute withSecond withNano
LocalDateTime ldt2 = ldt.withYear(2029);
LocalDateTime ldt3 = ldt.withMinute(59);

// 3、加多少:
// plusYears  plusMonths plusDays plusWeeks 
// plusHours plusMinutes plusSeconds plusNanos
LocalDateTime ldt4 = ldt.plusYears(2);
LocalDateTime ldt5 = ldt.plusMinutes(3);

// 4、减多少：
// minusDays minusYears minusMonths minusWeeks 
// minusHours minusMinutes minusSeconds minusNanos
LocalDateTime ldt6 = ldt.minusYears(2);
LocalDateTime ldt7 = ldt.minusMinutes(3);


// 5、获取指定日期和时间的LocalDateTime对象：
// public static LocalDateTime of(int year, Month month, int dayOfMonth, int hour,
//                                  int minute, int second, int nanoOfSecond)
LocalDateTime ldt8 = LocalDateTime.of(2029, 12, 12, 12, 12, 12, 1222);
LocalDateTime ldt9 = LocalDateTime.of(2029, 12, 12, 12, 12, 12, 1222);

// 6、 判断2个日期、时间对象，是否相等，在前还是在后： equals、isBefore、isAfter
System.out.println(ldt9.equals(ldt8));
System.out.println(ldt9.isAfter(ldt));
System.out.println(ldt9.isBefore(ldt));

// 7、可以把LocalDateTime转换成LocalDate和LocalTime
// public LocalDate toLocalDate()
// public LocalTime toLocalTime()
// public static LocalDateTime of(LocalDate date, LocalTime time)
LocalDate ld = ldt.toLocalDate();
LocalTime lt = ldt.toLocalTime();
LocalDateTime ldt10 = LocalDateTime.of(ld, lt);
```

## ZonedDateTime(JDK8)

```java
// 1、ZoneId的常见方法：
// public static ZoneId systemDefault(): 获取系统默认的时区
ZoneId zoneId = ZoneId.systemDefault();
System.out.println(zoneId.getId());
System.out.println(zoneId);

// public static Set<String> getAvailableZoneIds(): 获取Java支持的全部时区Id
System.out.println(ZoneId.getAvailableZoneIds());

// public static ZoneId of(String zoneId) : 把某个时区id封装成ZoneId对象。
ZoneId zoneId1 = ZoneId.of("America/New_York");

// 2、ZonedDateTime：带时区的时间。
// public static ZonedDateTime now(ZoneId zone): 获取某个时区的ZonedDateTime对象。
ZonedDateTime now = ZonedDateTime.now(zoneId1);
System.out.println(now);

// 世界标准时间了
ZonedDateTime now1 = ZonedDateTime.now(Clock.systemUTC());
System.out.println(now1);

// public static ZonedDateTime now()：获取系统默认时区的ZonedDateTime对象
ZonedDateTime now2 = ZonedDateTime.now();
System.out.println(now2);

// Calendar instance = Calendar.getInstance(TimeZone.getTimeZone(zoneId1));
```

## Instant(JDK8)

通过获取Instant的对象可以拿到此刻的时间，该时间由两部分组成：从1970-01-01 00:00:00 开始走到此刻的总秒数+不够1秒的纳秒数。（时间线上的某个时刻、时间戳）

```java
// 1、创建Instant的对象，获取此刻时间信息（标准时间）
Instant now = Instant.now(); // 不可变对象

// 2、获取总秒数（获取1970-01-01 00:00:00开始记录的秒数）
long second = now.getEpochSecond();

// 3、不够1秒的纳秒数
int nano = now.getNano();

// 4、增加时间系列的方法plusSeconds plusMillis
Instant instant = now.plusNanos(111);

// Instant对象的作用：做代码的性能分析，或者记录用户的操作时间点
Instant now1 = Instant.now();
// 代码执行。。。。
Instant now2 = Instant.now();
```

## Period(JDK8)

用来计算两个日期之间相隔的年、相隔的月、相隔的日。**只能两个计算LocalDate对象之间的间隔**

```java
LocalDate start = LocalDate.of(2029, 8, 10);
LocalDate end = LocalDate.of(2029, 12, 15);

// 1、创建Period对象，封装两个日期对象。
Period period = Period.between(start, end);

// 2、通过period对象获取两个日期对象相差的信息。
System.out.println(period.getYears());
System.out.println(period.getMonths());
System.out.println(period.getDays());
```

## Duration(JDK8)

用于计算两个时间对象相差的天数、小时数、分数、秒数、纳秒数；**支持LocalTime、LocalDateTime、Instant等时间**

```java
LocalDateTime start = LocalDateTime.of(2025, 11, 11, 11, 10, 10);
LocalDateTime end = LocalDateTime.of(2025, 11, 11, 11, 11, 11);
// 1、得到Duration对象
Duration duration = Duration.between(start, end);

// 2、获取两个时间对象间隔的信息
System.out.println(duration.toDays());// 间隔多少天
System.out.println(duration.toHours());// 间隔多少小时
System.out.println(duration.toMinutes());// 间隔多少分
System.out.println(duration.toSeconds());// 间隔多少秒
System.out.println(duration.toMillis());// 间隔多少毫秒
System.out.println(duration.toNanos());// 间隔多少纳秒
```

## Arrays工具类

### Arrays.toString()

**public static String toString(类型[] arr): 返回数组的内容**
int[] arr = {10, 20, 30, 40, 50, 60};
System.out.println(Arrays.toString(arr));

### Arrays.copyOfRange()

**public static 类型[] copyOfRange(类型[] arr, 起始索引, 结束索引)**
**// 拷贝数组（指定范围，左闭右开）**
int[] arr2 = Arrays.copyOfRange(arr, 1, 4);
System.out.println(Arrays.toString(arr2));

### Arrays.copyOf()

**public static copyOf(类型[] arr, int newLength)**
**// 拷贝数组，可以指定新数组的长度。****
int[] arr3 = Arrays.copyOf(arr, 10);
System.out.println(Arrays.toString(arr3));

### Arrays.setAll()

**public static setAll(double[] array, IntToDoubleFunction generator)**
**// 把数组中的原数据改为新数据又存进去。**

```javadouble[] prices = {99.8, 128, 100};
// 把所有的价格都打八折，然后又存进去。
Arrays.setAll(prices, new IntToDoubleFunction() {
    @Override
    public double applyAsDouble(int value) {
        // value = 0  1  2
        return prices[value] * 0.8;
    }
});
System.out.println(Arrays.toString(prices));
```

### Arrays.fill()

**Arrays.fill(Type[] array, Type value)**

**//填充到所有位置**

**Arrays.fill(Type[] array, int startIndex, int endIndex, Type value)**

**//填充到指定范围位置，startIndex和endIndex左闭右开**

### Arrays.asList()

- 该方法适用于对象型数据的数组（String、Integer…）
- 方法不建议使用于基本数据类型的数组（byte,short,int,long,float,double,boolean）

- 该方法将数组与List列表链接起来：当更新其一个时，另一个自动更新

- 不支持add()、remove()、clear()等方法

- 此方法得到的List长度是不可变的

- asList返回的是java.util.Arrays.ArrayList，而不是java.util

### Arrays.sort()

**public static void sort(类型[] arr)**

**//对数组进行排序(默认是升序排序)**
Arrays.sort(prices);

### Arrays.sort()自定义排序

自定义排序规则：认为左边对象大于右边对象，请返回正整数。认为左边对象小于右边对象，请返回负整数。认为左边对象等于右边对象，请一定返回0。<a id = "arrays.sort">Arrays.sort() 自定义排序锚点</a>

- 排序方式1：让Student类实现Comparable接口，同时重写compareTo方法。Arrays的sort方法底层会根据compareTo方法的返回值是正数、负数、还是0来确定谁大、谁小、谁相等。代码如下：

```java
public class Student implements Comparable<Student>{
    private String name;
    private double height;
    private int age;
    
    //...get、set、空参数构造方法、有参数构造方法...自己补全

    // 指定比较规则
    // this  o
    @Override
    public int compareTo(Student o) {
        // 约定1：认为左边对象 大于 右边对象 请您返回正整数
        // 约定2：认为左边对象 小于 右边对象 请您返回负整数
        // 约定3：认为左边对象 等于 右边对象 请您一定返回0
		/* if(this.age > o.age){
            return 1;
        }else if(this.age < o.age){
            return -1;
        }
        return 0;*/

        //上面的if语句，也可以简化为下面的一行代码
        return this.age - o.age; // 按照年龄升序排列
        // return o.age - this.age; // 按照年龄降序排列
    }
    
    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", height=" + height +
                ", age=" + age +
                '}';
    }
}
```

- **排序方式2**：在调用`Arrays.sort(数组,Comparator比较器);`时，除了传递数组之外，传递一个Comparator比较器对象。Arrays的sort方法底层会根据Comparator比较器对象的compare方法方法的返回值是正数、负数、还是0来确定谁大、谁小、谁相等。代码如下

```java
public class ArraysTest2 {
    public static void main(String[] args) {
        // 目标：掌握如何对数组中的对象进行排序。
        Student[] students = new Student[4];
        students[0] = new Student("蜘蛛精", 169.5, 23);
        students[1] = new Student("紫霞", 163.8, 26);
        students[2] = new Student("紫霞", 163.8, 26);
        students[3] = new Student("至尊宝", 167.5, 24);

		// 2、public static <T> void sort(T[] arr, Comparator<? super T> c)
        // 参数一：需要排序的数组
        // 参数二：Comparator比较器对象（用来制定对象的比较规则）
        Arrays.sort(students, new Comparator<Student>() {
            @Override
            public int compare(Student o1, Student o2) {
                // 制定比较规则了：左边对象 o1   右边对象 o2
                // 约定1：认为左边对象 大于 右边对象 请您返回正整数
                // 约定2：认为左边对象 小于 右边对象 请您返回负整数
                // 约定3：认为左边对象 等于 右边对象 请您一定返回0
//                if(o1.getHeight() > o2.getHeight()){
//                    return 1;
//                }else if(o1.getHeight() < o2.getHeight()){
//                    return -1;
//                }
//                return 0; // 升序
                 return Double.compare(o1.getHeight(), o2.getHeight()); // 升序
                // return Double.compare(o2.getHeight(), o1.getHeight()); // 降序
            }
        });
        System.out.println(Arrays.toString(students));
    }
}
```

## Lambda表达式*

在使用Lambda表达式之前，必须先有一个接口，而且接口中只能有一个抽象方法。**（注意：不能是抽象类，只能是接口）**像这样的接口，我们称之为函数式接口，只有基于函数式接口的匿名内部类才能被Lambda表达式简化。

```java
(被重写方法的形参列表) -> {
    被重写方法的方法体代码;
}
```

Lamdba表达式的几种简化写法。具体的简化规则如下

```java
1.Lambda的标准格式
	(参数类型1 参数名1, 参数类型2 参数名2)->{
		...方法体的代码...
		return 返回值;
	}

2.在标准格式的基础上()中的参数类型可以直接省略
	(参数名1, 参数名2)->{
		...方法体的代码...
		return 返回值;
	}
	
3.如果{}总的语句只有一条语句，则{}可以省略、return关键字、以及最后的“;”都可以省略
	(参数名1, 参数名2)-> 结果
	
4.如果()里面只有一个参数，则()可以省略
	(参数名)->结果
```

## 方法引用(JDK8)

### 静态方法引用

- 如果某个Lambda表达式里只是调用一个静态方法，并且前后参数的形式一致，就可以使用静态方法引用

- `类名::静态方法`

```java
// Lambda表达式
Arrays.sort(students, (o1, o2) -> CompareByData.compareByAge(o1, o2));

//静态方法引用：类名::静态方法名
Arrays.sort(students, CompareByData::compareByAge);
```

### 实例方法引用

- 如果某个Lambda表达式里只是调用一个实例方法，并且前后参数的形式一致，就可以使用实例方法引用
- `对象名::实例方法`

```java
// Lambda表达式
CompareByData compare = new CompareByData();
Arrays.sort(students, (o1, o2) -> compare.compareByAgeDesc(o1, o2)); // 降序
//实例方法引用：对象名::实例方法名
CompareByData compare = new CompareByData();
Arrays.sort(students, compare::compareByAgeDesc); // 降序
```

### 特定类型的方法引用

- 如果某个Lambda表达式里只是调用一个实例方法，并且前面参数列表中的第一个参数作为方法的主调，后面的所有参数都是作为该实例方法的入参时，则就可以使用特定类型的方法引用。
- `类型::方法名`

```java
//lambda表达式写法
Arrays.sort(names, ( o1,  o2) -> o1.compareToIgnoreCase(o2) );
//特定类型的方法引用：类型::方法名
Arrays.sort(names, String::compareToIgnoreCase);
```

### 构造器引用

- 如果某个Lambda表达式里只是在创建对象，并且前后参数情况一致，就可以使用构造器引用
- `类名::new`

```java
//lambda表达式写法
CreateCar cc2 = (name,  price) -> new Car(name, price);

//使用方法引用改进：构造器引用
CreateCar cc3 = Car::new;
```

## 正则表达式

### 正则表达式的书写规则

![](JavaSEimages\1667469259345.png)

```java
// 1、字符类(只能匹配单个字符)
System.out.println("a".matches("[abc]"));  // [abc]只能匹配a、b、c
System.out.println("e".matches("[abcd]")); // false

System.out.println("d".matches("[^abc]"));   // [^abc] 不能是abc
System.out.println("a".matches("[^abc]"));  // false

System.out.println("b".matches("[a-zA-Z]")); // [a-zA-Z] 只能是a-z A-Z的字符
System.out.println("2".matches("[a-zA-Z]")); // false

System.out.println("k".matches("[a-z&&[^bc]]")); // ： a到z，除了b和c
System.out.println("b".matches("[a-z&&[^bc]]")); // false

System.out.println("ab".matches("[a-zA-Z0-9]")); // false 注意：以上带 [内容] 的规则都只能用于匹配单个字符

// 2、预定义字符(只能匹配单个字符)  .  \d  \D   \s  \S  \w  \W
System.out.println("徐".matches(".")); // .可以匹配任意字符
System.out.println("徐徐".matches(".")); // false

// \转义
System.out.println("\"");
// \n \t
System.out.println("3".matches("\\d"));  // \d: 0-9
System.out.println("a".matches("\\d"));  //false

System.out.println(" ".matches("\\s"));   // \s: 代表一个空白字符
System.out.println("a".matches("\s")); // false

System.out.println("a".matches("\\S"));  // \S: 代表一个非空白字符
System.out.println(" ".matches("\\S")); // false

System.out.println("a".matches("\\w"));  // \w: [a-zA-Z_0-9]
System.out.println("_".matches("\\w")); // true
System.out.println("徐".matches("\\w")); // false

System.out.println("徐".matches("\\W"));  // [^\w]不能是a-zA-Z_0-9
System.out.println("a".matches("\\W"));  // false

System.out.println("23232".matches("\\d")); // false 注意：以上预定义字符都只能匹配单个字符。

// 3、数量词： ?   *   +   {n}   {n, }  {n, m}
System.out.println("a".matches("\\w?"));   // ? 代表0次或1次
System.out.println("".matches("\\w?"));    // true
System.out.println("abc".matches("\\w?")); // false

System.out.println("abc12".matches("\\w*"));   // * 代表0次或多次
System.out.println("".matches("\\w*"));        // true
System.out.println("abc12张".matches("\\w*")); // false

System.out.println("abc12".matches("\\w+"));   // + 代表1次或多次
System.out.println("".matches("\\w+"));       // false
System.out.println("abc12张".matches("\\w+")); // false

System.out.println("a3c".matches("\\w{3}"));   // {3} 代表要正好是n次
System.out.println("abcd".matches("\\w{3}"));  // false
System.out.println("abcd".matches("\\w{3,}"));     // {3,} 代表是>=3次
System.out.println("ab".matches("\\w{3,}"));     // false
System.out.println("abcde徐".matches("\\w{3,}"));     // false
System.out.println("abc232d".matches("\\w{3,9}"));     // {3, 9} 代表是  大于等于3次，小于等于9次

// 4、其他几个常用的符号：(?i)忽略大小写 、 或：| 、  分组：()
System.out.println("abc".matches("(?i)abc")); // true
System.out.println("ABC".matches("(?i)abc")); // true
System.out.println("aBc".matches("a((?i)b)c")); // true
System.out.println("ABc".matches("a((?i)b)c")); // false

// 需求1：要求要么是3个小写字母，要么是3个数字。
System.out.println("abc".matches("[a-z]{3}|\\d{3}")); // true
System.out.println("ABC".matches("[a-z]{3}|\\d{3}")); // false
System.out.println("123".matches("[a-z]{3}|\\d{3}")); // true
System.out.println("A12".matches("[a-z]{3}|\\d{3}")); // false

// 需求2：必须是”我爱“开头，中间可以是至少一个”编程“，最后至少是1个”666“
System.out.println("我爱编程编程666666".matches("我爱(编程)+(666)+"));
System.out.println("我爱编程编程66666".matches("我爱(编程)+(666)+"));
```

### 正则表达式搜索、替换

```java
public String replaceAll(String regex, String newStr) //按照正则表达式的内容进行替换

public String[] split(String regex)  //按照正则表达式的内容进行分割字符串，返回一个字符串数组
 
```

```java 
// 1、public String replaceAll(String regex , String newStr)：按照正则表达式匹配的内容进行替换
// 需求1：请把下面字符串中的不是汉字的部分替换为 “-”
String s1 = "古力娜扎ai8888迪丽热巴999aa5566马尔扎哈fbbfsfs42425卡尔扎巴";
System.out.println(s1.replaceAll("\\w+", "-"));

// 2、public String[] split(String regex)：按照正则表达式匹配的内容进行分割字符串，反回一个字符串数组。
// 需求1：请把下面字符串中的人名取出来，使用切割来做
String s3 = "古力娜扎ai8888迪丽热巴999aa5566马尔扎哈fbbfsfs42425卡尔扎巴";
String[] names = s3.split("\\w+");
System.out.println(Arrays.toString(names));
```

**去除字符串中重复的字符**

某语音系统，收到一个口吃的人说的“我我我喜欢编编编编编编编编编编编编程程程！”，需要优化成“我喜欢编程！”。
String s2 = "我我我喜欢编编编编编编编编编编编编程程程";
`System.out.println(s2.replaceAll("(.)\\1+", "$1"));`

## 异常

![](JavaSEimages\1667313423356.png)

### 运行时异常

### 编译时异常

我们在调用SimpleDateFormat对象的parse方法时，要求传递的参数必须和指定的日期格式一致，否则就会出现异常。 Java比较贴心，它为了更加强烈的提醒方法的调用者，设计了编译时异常，它把异常的提醒提前了，你调用方法是否真的有问题，只要可能有问题就给你报出异常提示（红色波浪线）。

 **编译时异常的目的：意思就是告诉你，这里小心点容易出错，仔细检查一下，解决方案如下**

- 第一种：使用throws在方法上声明，意思就是告诉下一个调用者，这里面可能有异常啊，你调用时注意一下。

```java
public class ExceptionTest1 {
    public static void main(String[] args) throws ParseException{
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        Date d = sdf.parse("2028-11-11 10:24");
        System.out.println(d);
    }
}
```

- 第二种：使用try...catch语句块异常进行处理。

```java
public class ExceptionTest1 {
    public static void main(String[] args) throws ParseException{
        try {
            SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
            Date d = sdf.parse("2028-11-11 10:24");
            System.out.println(d);
        } catch (ParseException e) {
            e.printStackTrace();
        }
    }
}
```

### 自定义异常

```java
1.如果自定义异常类继承Excpetion，则是编译时异常。
	特点：方法中抛出的是编译时异常，必须在方法上使用throws声明，强制调用者处理。
	
2.如果自定义异常类继承RuntimeException，则运行时异常。
	特点：方法中抛出的是运行时异常，不需要在方法上用throws声明。
```

### 异常处理

1.将异常捕获，将比较友好的信息显示给用户看；2.尝试重新执行，看是是否能修复这个问题。

- 第一种处理方式是，在main方法中对异常进行try...catch捕获处理了，给出友好提示。

```java
public class ExceptionTest3 {
    public static void main(String[] args)  {
        try {
            test1();
        } catch (FileNotFoundException e) {
            System.out.println("您要找的文件不存在！！");
            e.printStackTrace(); // 打印出这个异常对象的信息。记录下来。
        } catch (ParseException e) {
            System.out.println("您要解析的时间有问题了！");
            e.printStackTrace(); // 打印出这个异常对象的信息。记录下来。
        }
    }

    public static void test1() throws FileNotFoundException, ParseException {
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        Date d = sdf.parse("2028-11-11 10:24:11");
        System.out.println(d);
        test2();
    }

    public static void test2() throws FileNotFoundException {
        // 读取文件的。
        InputStream is = new FileInputStream("D:/meinv.png");
    }
}
```

- 第二种处理方式是：在main方法中对异常进行捕获，并尝试修复

```java
public class ExceptionTest4 {
    public static void main(String[] args) {
        // 需求：调用一个方法，让用户输入一个合适的价格返回为止。
        // 尝试修复
        while (true) {
            try {
                System.out.println(getMoney());
                break;
            } catch (Exception e) {
                System.out.println("请您输入合法的数字！！");
            }
        }
    }

    public static double getMoney(){
        Scanner sc = new Scanner(System.in);
        while (true) {
            System.out.println("请您输入合适的价格：");
            double money = sc.nextDouble();
            if(money >= 0){
                return money;
            }else {
                System.out.println("您输入的价格是不合适的！");
            }
        }
    }
}
```

## Collection

Collection是单列集合的根接口，Collection接口下面又有两个子接口List接口、Set接口，List和Set下面分别有不同的实现类

![](JavaSEimages\1666155169359.png)

上图中各种集合的特点如下图所示：

![](JavaSEimages\1666155218956.png)

Collection集合的常用方法，ArrayList、LinkedList、HashSet、LinkedHashSet、TreeSet集合都可以调用下面的方法。

```java
//1.public boolean add(E e): 添加元素到集合

//2.public int size(): 获取集合的大小

//3.public boolean contains(Object obj): 判断集合中是否包含某个元素

//4.pubilc boolean remove(E e): 删除某个元素，如果有多个重复元素只能删除第一个

//5.public void clear(): 清空集合的元素

//6.public boolean isEmpty(): 判断集合是否为空 是空返回true 反之返回false

//7.public Object[] toArray(): 把集合转换为数组

//8.如果想把集合转换为指定类型的数组，可以使用下面的代码
String[] array1 = c.toArray(new String[c.size()]);
System.out.println(Arrays.toString(array1)); //[java1,java2, java2, java3]

//9.还可以把一个集合中的元素，添加到另一个集合中
Collection<String> c1 = new ArrayList<>();
c1.add("java1");
c1.add("java2");
Collection<String> c2 = new ArrayList<>();
c2.add("java3");
c2.add("java4");
c1.addAll(c2); //把c2集合中的全部元素，添加到c1集合中去
```

### Collection遍历

#### 迭代器遍历



![](JavaSEimages\1666162899638.png)

迭代器代码的原理如下：

- 当调用iterator()方法获取迭代器时，当前指向第一个元素
- hasNext()方法则判断这个位置是否有元素，如果有则返回true，进入循环
- 调用next()方法获取元素，并将当月元素指向下一个位置，
- 等下次循环时，则获取下一个元素，依此类推

```java
Collection<String> c = new ArrayList<>();
c.add("赵敏");
c.add("小昭");
c.add("素素");
c.add("灭绝");
System.out.println(c); //[赵敏, 小昭, 素素, 灭绝]

//第一步：先获取迭代器对象
//解释：Iterator就是迭代器对象，用于遍历集合的工具)
Iterator<String> it = c.iterator();

//第二步：用于判断当前位置是否有元素可以获取
//解释：hasNext()方法返回true，说明有元素可以获取；反之没有
while(it.hasNext()){
    //第三步：获取当前位置的元素，然后自动指向下一个元素.
    String e = it.next();
    System.out.println(s);
}
```

#### 增强for遍历

增强for不光可以遍历集合，还可以遍历数组。

```java
//增强for的格式
for(元素的数据类型 变量名: 数组或者集合){
    
}

Collection<String> c = new ArrayList<>();
c.add("赵敏");
c.add("小昭");
c.add("素素");
c.add("灭绝");

//1.使用增强for遍历集合
for(String s: c){
    System.out.println(s); 
}

//2.再尝试使用增强for遍历数组
String[] arr = {"迪丽热巴", "古力娜扎", "稀奇哈哈"};
for(String name: arr){
    System.out.println(name);
}
```

增强for的内存原理如下图所示：**当往集合中存对象时，实际上存储的是对象的地址值**

![1666165033103](JavaSEimages\1666165033103.png)

#### forEach遍历(JDK8)

在JDK8版本以后还提供了一个forEach方法也可以遍历集合

```java
default void forEach(Consumer<? super T> action)  //结合lambda遍历集合
```

forEach方法的参数是一个Consumer接口，而Consumer是一个函数式接口，所以可以传递Lambda表达式

```java
Collection<String> c = new ArrayList<>();
c.add("赵敏");
c.add("小昭");
c.add("素素");
c.add("灭绝");

//调用forEach方法
//由于参数是一个Consumer接口，所以可以传递匿名内部类
c.forEach(new Consumer<String>{
    @Override
    public void accept(String s){
        System.out.println(s);
    }
});


//也可以使用lambda表达式对匿名内部类进行简化
c.forEach(s->System.out.println(s)); //[赵敏, 小昭, 素素, 灭绝]
```

### Collection的并发修改异常

为什么会出现这个异常呢？那是因为迭代器遍历机制，规定迭代器遍历集合的同时，不允许集合自己去增删元素，否则就会出现这个异常。<a id ='bingfayichang'>Collection的并发修改异常锚点</a>

- 使用迭代器遍历集合时，又同时删除集合中的数据，程序就会出现并发修改异常的错误。
- 由于增强for循环遍历集合就是迭代器遍历集合的简化写法，因此，使用增强for循环遍历集合，又在同时删除集合中的数据时，程序也会出现并发修改异常的错误

解决方案

- 使用迭代器遍历集合，**但用迭代器自己的删除方法删除数据即可 `it.remove();`**
- 如果能用for循环遍历时，**可以倒着遍历并删除，或者从前往后遍历，但删除元素后做i--操作**

### Deque接口（双向队列）

双端队列（Deque），是Quene是一个子接口，双向队列是指该队列两端的元素既能入队（offer）也能出队(poll)，如果将Deque限制为只能从一端入队(push)和出队(pop)，则可限制栈的数据结构。对于栈而言，有入栈，遵循先进后出原则。

它既可以当作栈使用，也可以当作队列使用

```
*                    第一个元素（头部）               最后一个元素（尾部）
* 操作         引发异常          返回特殊值         引发异常       特殊价值
* 插入        addFirst(e)      offerFirst(e)    addLast(e)     offerLast(e)
* 获取并删除   removeFirst()    pollFirst()      removeLast()   pollLast()
* 获取        getFirst()        peekFirst()      getLast()      peekLast()
```

3、此接口继承了Queue接口。当双端用作 FIFO（先进先出）行为队列时 Deque的部分方法与 Queue的等价：

```
* Queue 方法    等效 Deque 的方法
* add(e)         addLast(e)
* offer(e)       offerLast(e)
* remove()       removeFirst()
* poll()         pollFirst()
* element()      getFirst()
* peek()         peekFirst()
```

4、Deque 也可以用作LIFO（后进先出）堆栈。应优先使用此接口而不是旧 Stack 类。当双端面用作堆栈时，元素将从双端的开头推送和弹出。堆栈方法与下表所示的方法完全相同 Deque：

```
* 堆栈方法   等效 Deque 方法
* push(e)    addFirst(e)
* pop()      removeFirst()
* peek()     peekFirst()
```

### List

**有序（添加数据的顺序和取出数据的顺序一致），可重复，有索引**

```java
//public void add(int index, E element): 在某个索引位置插入元素

//public E remove(int index): 根据索引删除元素, 返回被删除的元素

//public E get(int index): 返回集合中指定位置的元素

//public E set(int index, E e): 修改索引位置处的元素，修改后，会返回原数据
```

List集合相比于前面的Collection多了一种可以通过普通for循环遍历的方式（只因为List有索引）

**List集合删除元素(使用for循环)**[Collection的并发修改异常](#bingfayichang)

- 每次删除完元素后，让控制循环的变量`i--`
- 倒着遍历集合，在遍历过程中删除元素

#### toArray()

list.toArray()方法不接收参数时, 返回一个Object数组

**toArray(T[] a)方法接收T类型的数组, 返回一个T类型的数组 (常用)**

```java
//数组的长度指定为0或者指定为list.size()都可以，String[0]就是起一个模板的作用，
//即使传入的数组长度不够也没关系, 会创建新数组 
String[] strs = list.toArray(new String[0]);
//int[][]的长度指定为0或者指定为list.size()都可以
int[][]arr = list.toArray(new int[0][]);
```

- 该方法用了泛型,并且是用在方法的创建中( 相当于定义泛型，T[]是在使用泛型T)

- 泛型是Java SE 1.5的新特性，泛型的本质是参数化类型，也就是说所操作的数据类型被指定为一个参数。这种参数类型可以用在类、接口和方法的创建中，分别称为泛型类、泛型接口、泛型方法

- 该方法返回集合中所有元素的数组；返回数组的运行时类型与指定数组的运行时类型相同

- 与toArray()相比，toArray是返回一个Object[] 然后进行copy

- toArray(new String[0])则是根据参数数组的类型，构造了一个对应类型的，长度跟ArrayList的size一致的空数组
  

#### ArrayList

**有序（添加数据的顺序和取出数据的顺序一致），可重复，有索引**

ArrayList集合底层是基于数组结构实现的，也就是说当你往集合容器中存储元素时，底层本质上是往数组中存储元素。

![](JavaSEimages\1666166151267.png)

数组扩容，并不是在原数组上扩容（原数组是不可以扩容的），底层是创建一个新数组，然后把原数组中的元素全部复制到新数组中去。

![](JavaSEimages\1666166661149.png)



![](JavaSEimages\1662620389155.png)

```java
//1.创建一个ArrayList集合对象（有序、有索引、可以重复）
List<String> list = new ArrayList<>();//只能调用ArrayList中实现List中的方法
list.add("蜘蛛精");
list.add("至尊宝");
list.add("至尊宝");
list.add("牛夫人"); 
System.out.println(list); //[蜘蛛精, 至尊宝, 至尊宝, 牛夫人]

//2.public void add(int index, E element): 在某个索引位置插入元素
list.add(2, "紫霞仙子");
System.out.println(list); //[蜘蛛精, 至尊宝, 紫霞仙子, 至尊宝, 牛夫人]

//3.public E remove(int index): 根据索引删除元素, 返回被删除的元素
System.out.println(list.remove(2)); //紫霞仙子
System.out.println(list);//[蜘蛛精, 至尊宝, 至尊宝, 牛夫人]

//4.public E get(int index): 返回集合中指定位置的元素
System.out.println(list.get(3));

//5.public E set(int index, E e): 修改索引位置处的元素，修改后，会返回原数据
System.out.println(list.set(3,"牛魔王")); //牛夫人
System.out.println(list); //[蜘蛛精, 至尊宝, 至尊宝, 牛魔王] 
```



- 如果有相同的数据`remove`默认删除的是第一次出现的数据
- 通过`print`打印出来的不是`ArrayList`的地址，而是数据（因为`ArrayList`的父类`AbstractList`的父类`AbstractCollection`重写了`object`的`toString`方法）
- 一般定义集合都要采用泛型，如果想存各种类型的数据，则`ArrayList<Object> list = new Arraylist<>()`
- 集合容器中存储的是每个对象在**堆内存**中的地址

#### LinkedList

**有序（添加数据的顺序和取出数据的顺序一致），可重复，有索引**

LinkedList底层是**双向链表**结构，链表结构是由一个一个的节点组成，一个节点由数据值、下一个元素的地址组成。

![](JavaSEimages\1666167523139.png)

LinkedList集合是基于双向链表实现了，所以相对于ArrayList新增了一些可以针对头尾进行操作的方法，如下图示所示：

![](JavaSEimages\1666167572387.png)

##### queue

入队列可以调用LinkedList集合的addLast方法，出队列可以调用removeFirst()方法

```java
//1.创建一个队列：先进先出、后进后出
LinkedList<String> queue = new LinkedList<>();

//入对列
queue.addLast("第1号人");
queue.addLast("第2号人");
queue.addLast("第3号人");
queue.addLast("第4号人");
System.out.println(queue);

//出队列
System.out.println(queue.removeFirst());	//第4号人
System.out.println(queue.removeFirst());	//第3号人
System.out.println(queue.removeFirst());	//第2号人
System.out.println(queue.removeFirst());	//第1号人
```

**Queue类**

- add(E e) & offer(E e)：都是向优先队列中插入元素，只是Queue接口规定二者对插入失败时的处理不同，前者在插入失败时抛出异常，后则则会返回false。
- peek()：都是**获取但不删除队首元素**，二者唯一的区别是当方法失败时前者抛出异常，后者返回null。
- remove() & poll()：都是**获取并删除队首元素**，区别是当方法失败时前者抛出异常，后者返回null。

##### stack

接着，我们就用LinkedList来模拟下栈结构，代码如下：

```java
//1.创建一个栈对象
LinkedList<String> stack = new ArrayList<>();
//压栈(push) 等价于 addFirst()
stack.push("第1颗子弹");
stack.push("第2颗子弹");
stack.push("第3颗子弹");
stack.push("第4颗子弹");
System.out.println(stack); //[第4颗子弹, 第3颗子弹, 第2颗子弹,第1颗子弹]

//弹栈(pop) 等价于 removeFirst()
System.out.println(statck.pop()); //第4颗子弹
System.out.println(statck.pop()); //第3颗子弹
System.out.println(statck.pop()); //第2颗子弹
System.out.println(statck.pop()); //第1颗子弹

//弹栈完了，集合中就没有元素了
System.out.println(list); //[]
```

**Stack类**

**Stack< String >stack = new Stack<>();**

- push() 进栈
- pop() 出栈
- peek() 返回栈顶元素的值
- size() 返回栈的长度
- empty() 判断栈是否为空，返回布尔值









### Set

**无序（添加数据的顺序和取出数据的顺序不一致），不重复，无索引**

#### HashSet

**无序（添加数据的顺序和取出数据的顺序不一致），不重复，无索引**

**HashSet底层就是HashMap**，我们可以看源码验证这一点，如下图所示，我们可以看到，创建HashSet集合时，底层帮你创建了HashMap集合；往HashSet集合中添加添加元素时，底层却是调用了Map集合的put方法把元素作为了键来存储。所以实际上根本没有什么HashSet集合，把HashMap的集合的值忽略不看就是HashSet集合。

![](JavaSEimages\1667641783744.png)

------

**HashMap底层实现**

故HashSet集合底层就是基于哈希表实现的

- JDK8以前：哈希表 = 数组+链表
- JDK8以后：哈希表 = 数组+链表+红黑树

![](JavaSEimages\1666170451762-1667311904484.png)

![](JavaSEimages\1666171011761-1667311900100.png)

```java
if（链表长度 > 8 && 数组长度 > 64）
	将链表转换为红黑树
```

如果HashMap的容量较小，例如小于64，链表的长度即使超过了阈值，转换成红黑树可能并不会带来明显的性能提升。因为哈希表的容量小，碰撞（哈希冲突）的概率相对较低，链表的长度也不会太长，查找操作的时间复杂度仍然可以保持在较低水平。



------

**HashSet相同元素判断**

往HashSet集合中存储元素时，底层调用了元素的两个方法：一个是hashCode方法获取元素的hashCode值（哈希值）；另一个是调用了元素的equals方法，用来比较新添加的元素和集合中已有的元素是否相同。 

- 只有新添加元素的hashCode值和集合中以后元素的hashCode值相同、新添加的元素调用equals方法和集合中已有元素比较结果为true, 才认为元素重复。
- 如果hashCode值相同，equals比较不同，则以链表的形式连接在数组的同一个索引为位置（如上图所示）

要想保证在HashSet集合中没有重复元素，我们需要**重写元素类的hashCode和equals方法**。比如以下面的Student类为例，假设把Student类的对象作为HashSet集合的元素，想要让学生的姓名和年龄相同，就认为元素重复。

```java
//按快捷键生成hashCode和equals方法
//alt+insert 选择 hashCode and equals
@Override
public boolean equals(Object o) {
    if (this == o) return true;
    if (o == null || getClass() != o.getClass()) return false;

    Student student = (Student) o;

    if (age != student.age) return false;
    if (Double.compare(student.height, height) != 0) return false;
    return name != null ? name.equals(student.name) : student.name == null;
}

@Override
public int hashCode() {
    int result;
    long temp;
    result = name != null ? name.hashCode() : 0;
    result = 31 * result + age;
    temp = Double.doubleToLongBits(height);
    result = 31 * result + (int) (temp ^ (temp >>> 32));
    return result;
}

```

#### LinkedHashSet

**有序（添加数据的顺序和取出数据的顺序一致），不重复，无索引**

**LinkedHashSet它底层就是LinkedHashMap**，采用的是也是哈希表结构，只不过额外新增了一个双向链表来维护元素的存取顺序。每次添加元素，就和上一个元素用双向链表连接一下。第一个添加的元素是双向链表的头节点，最后一个添加的元素是双向链表的尾节点。

![](JavaSEimages\1666171776819-1667311894748.png)

#### TreeSet

**排序（按照数据的大小默认升序排序），不重复，无索引**

**TreeSet底层就是TreeMap**，TreeSet集合的特点是可以对元素进行排序，但是必须指定元素的排序规则。如果往集合中存储String类型的元素，或者Integer类型的元素，它们本身就具备排序规则，所以直接就可以排序。如果往TreeSet集合中存储自定义类型的元素，比如说Student类型，则需要我们自己指定排序规则，否则会出现异常。



**TreeSet ceiling()**

java.util.TreeSet 类的ceiling()方法用于返回大于或等于给定元素的该集合中的最小元素；如果没有此类元素，则返回null。

```
public E ceiling(E e)
```

**参数：**该方法将值e作为要匹配的参数。

**返回值：**此方法返回大于或等于e的最小元素；如果没有这样的元素，则返回null。



让TreeSet集合按照指定的规则排序，有两种办法：(类似[Arrays.sort()中的自定义排序规则](#arrays.sort))

- 第一种方法：让元素的类实现Comparable接口，重写compareTo方法
- 第二种方法：在创建TreeSet集合时，通过构造方法传递Compartor比较器对象

注意：如果类本身有实现Comparable接口（第一种方法），TreeSet集合也同时自带比较器（第二种方法），**默认使用集合自带的比较器排序（第二种方法）**。

```java
//1.重写compareTo方法
//按照年龄进行比较，只需要在方法中让this.age和o.age相减就可以。
//原理：在往TreeSet集合中添加元素时，add方法底层会调用compareTo方法，
//根据该方法的结果是正数、负数、还是零，决定元素放在后面、前面还是不存。

@Override
public int compareTo(Student o) {
    //this：表示将要添加进去的Student对象
    //o: 表示集合中已有的Student对象
    return this.age-o.age;
}

//2.创建TreeSet集合时，传递比较器对象排序
//原理：当调用add方法时，底层会先用比较器，
//根据Comparator的compare方是正数、负数、还是零，决定谁在后，谁在前，谁不存。

//下面代码中是按照学生的年龄升序排序
Set<Student> students = new TreeSet<>(new Comparator<Student>{
    @Override
    public int compare(Student o1, Student o2){
        //需求：按照学生的身高排序
        return Double.compare(o1,o2); 
    }
});
//Lambda表达式
Set<Student> students = new TreeSet<>((o1,o2) -> Double.compare(o1.getAge(),o2.getAge()));
```

------

##  Collections工具类

![](JavaSEimages\1667195108724.png)

```java
//1.public static <T> boolean addAll(Collection<? super T> c, T...e)
List<String> names = new ArrayList<>();
Collections.addAll(names, "张三","王五","李四", "张麻子");
System.out.println(names);

//2.public static void shuffle(List<?> list)：对集合打乱顺序
Collections.shuffle(names);
System.out.println(names);

//3.public static <T> void sort(List<T list): 对List集合排序
List<Integer> list = new ArrayList<>();
list.add(3);
list.add(5);
list.add(2);
Collections.sort(list);
System.out.println(list);
```

`Collections.sort()自定义排序` 如果我们往List集合中存储自定义对象，这个时候想要对List集合进行排序自定义比较规则的。指定排序规则有两种方式 (类似[Arrays.sort()中的自定义排序规则](#arrays.sort))

------

## Map

**键不能重复，值可以重复，每一个键只能找到自己对应的值。Map系列集合都是由键决定的，值只是一个附属品，值是不做要求的**

Map集合中的每一个元素是以`key=value`的形式存在的，一个`key=value`就称之为一个键值对，而且在Java中有一个类叫Entry类，Entry的对象用来表示键值对对象。



![](JavaSEimages\1667308854001.png)

```java
public V replace(K key, V value) //替换指定键的值
//该key是需要更改其关联值的指定键，value是要放入的新值，replace方法返回旧值，如果没有与指定键关联的值，则返回null。
      
default boolean replace(K key, V oldValue, V newValue) //只有当指定的旧值与指定的键相关联的值相匹配时，才会用指定的键的新值替换该值。
//key是需要更改其关联值的指定键，oldValue是与指定键关联的旧值，newValue是要放置的新值，无论值的替换是否成功，replace方法都返回true/false，如果指定的键没有关联的值，则不会进行替换，因此将返回false。如果指定键的现有旧值与指定的旧值不匹配，则不进行替换，因此返回false

public V getOrDefault(Object key, V defaultValue) 
//当Map集合中有这个key时，就返回这个key对应的value值，如果没有这个key就返回默认值defaultValue
//map中若没有key则把key的value设置为0, 若有key则把key的value设置为原有值上+1
map.put(key, window.getOrDefault(key, 0) + 1);
```



```java
 // 1.添加元素: 无序，不重复，无索引。
Map<String, Integer> map = new HashMap<>();
map.put("手表", 100);
map.put("手表", 220);
map.put("手机", 2);
map.put("Java", 2);
map.put(null, null);
System.out.println(map);
// map = {null=null, 手表=220, Java=2, 手机=2}

// 2.public int size():获取集合的大小
System.out.println(map.size());

// 3、public void clear():清空集合
//map.clear();
//System.out.println(map);

// 4.public boolean isEmpty(): 判断集合是否为空，为空返回true ,反之！
System.out.println(map.isEmpty());

// 5.public V get(Object key)：根据键获取对应值
int v1 = map.get("手表");
System.out.println(v1);
System.out.println(map.get("手机")); // 2
System.out.println(map.get("张三")); // null

// 6.public V remove(Object key)：根据键删除整个元素(删除键会返回键的值)
System.out.println(map.remove("手表"));
System.out.println(map);

// 7.public  boolean containsKey(Object key): 判断是否包含某个键 ，包含返回true ,反之
System.out.println(map.containsKey("手表")); // false
System.out.println(map.containsKey("手机")); // true
System.out.println(map.containsKey("java")); // false
System.out.println(map.containsKey("Java")); // true

// 8.public boolean containsValue(Object value): 判断是否包含某个值。
System.out.println(map.containsValue(2)); // true
System.out.println(map.containsValue("2")); // false

// 9.public Set<K> keySet(): 获取Map集合的全部键。
Set<String> keys = map.keySet();
System.out.println(keys);

// 10.public Collection<V> values(); 获取Map集合的全部值。
Collection<Integer> values = map.values();
System.out.println(values);

// 11.把其他Map集合的数据倒入到自己集合中来。(拓展)
Map<String, Integer> map1 = new HashMap<>();
map1.put("java1",  10);
map1.put("java2",  20);
Map<String, Integer> map2 = new HashMap<>();
map2.put("java3",  10);
map2.put("java2",  222);
map1.putAll(map2); // putAll：把map2集合中的元素全部倒入一份到map1集合中去。
System.out.println(map1);
System.out.println(map2);
```

### Map遍历

#### 键找值遍历

先获取Map集合全部的键，再通过遍历键来找值

```java
public class MapTest1 {
    public static void main(String[] args) {
        // 准备一个Map集合。
        Map<String, Double> map = new HashMap<>();
        map.put("蜘蛛精", 162.5);
        map.put("蜘蛛精", 169.8);
        map.put("紫霞", 165.8);
        map.put("至尊宝", 169.5);
        map.put("牛魔王", 183.6);
        System.out.println(map);
        // map = {蜘蛛精=169.8, 牛魔王=183.6, 至尊宝=169.5, 紫霞=165.8}

        // 1、获取Map集合的全部键
        Set<String> keys = map.keySet();
        // System.out.println(keys);
        // [蜘蛛精, 牛魔王, 至尊宝, 紫霞]
        //         key
        // 2、遍历全部的键，根据键获取其对应的值
        for (String key : keys) {
            // 根据键获取对应的值
            double value = map.get(key);
            System.out.println(key + "=====>" + value);
        }
    }
}
```

#### 使用Entry对象遍历

直接获取每一个Entry对象，把Entry存储到Set集合中去，再通过Entry对象获取键和值。

![](JavaSEimages\1667309587178.png)

```java
public class MapTest2 {
    public static void main(String[] args) {
        Map<String, Double> map = new HashMap<>();
        map.put("蜘蛛精", 169.8);
        map.put("紫霞", 165.8);
        map.put("至尊宝", 169.5);
        map.put("牛魔王", 183.6);
        System.out.println(map);
        // map = {蜘蛛精=169.8, 牛魔王=183.6, 至尊宝=169.5, 紫霞=165.8}
        // entries = [(蜘蛛精=169.8), (牛魔王=183.6), (至尊宝=169.5), (紫霞=165.8)]
        // entry = (蜘蛛精=169.8)
        // entry = (牛魔王=183.6)
        // ...
		
        // 1、调用Map集合提供entrySet方法，把Map集合转换成键值对类型的Set集合
        Set<Map.Entry<String, Double>> entries = map.entrySet();
        //entrySet实现了Set接口，里面存放的是键值对。一个K对应一个V。
        for (Map.Entry<String, Double> entry : entries) {
            String key = entry.getKey();
            double value = entry.getValue();
            System.out.println(key + "---->" + value);
        }
    }
}
```

#### Lambda表达式遍历(JDK8)

Map集合的第三种遍历方式，需要用到下面的一个方法forEach，而这个方法是JDK8版本以后才有的。调用起来非常简单，最好是结合的lambda表达式一起使用。

```java
public class MapTest3 {
    public static void main(String[] args) {
        Map<String, Double> map = new HashMap<>();
        map.put("蜘蛛精", 169.8);
        map.put("紫霞", 165.8);
        map.put("至尊宝", 169.5);
        map.put("牛魔王", 183.6);
        System.out.println(map);
        // map = {蜘蛛精=169.8, 牛魔王=183.6, 至尊宝=169.5, 紫霞=165.8}

		//遍历map集合，传递匿名内部类
        map.forEach(new BiConsumer<String, Double>() {
            @Override
            public void accept(String k, Double v) {
                System.out.println(k + "---->" + v);
            }
        });
		//遍历map集合，传递Lambda表达式
        map.forEach(( k,  v) -> {
            System.out.println(k + "---->" + v);
        });
    }
}
```

### HashMap

**键无序（添加数据的顺序和取出数据的顺序不一致），不重复，无索引**

------

**HashMap底层实现**

HashMap集合底层是基于哈希表实现的，哈希表是一种增删改查数据，性能相对都较好的数据结构

- JDK8以前：哈希表 = 数组+链表，链表采用头插法（新元素往链表的头部添加）
- JDK8以后：哈希表 = 数组+链表+红黑树，链表采用尾插法（新元素我那个链表的尾部添加）
- 底层数组默认长度为16，如果数组中有超过12个位置已经存储了元素，则会对数组进行扩容2倍，数组扩容的加载因子是0.75，意思是：16*0.75=12

![](JavaSEimages\1666170451762-1667311904484.png)

![](JavaSEimages\1666171011761-1667311900100.png)

------

**Hash表中链表和红黑树的转换条件**

在JDK8开始后，为了提高性能（链表的），当**链表超过8且数组长度(数据总量)大于等于64**才会转为红黑树。将链表转换成红黑树前会判断，**如果当前数组的长度小于64，则不会转换为红黑树，而是进行数组扩容**。

在**红黑树结点数量为6**的时候转换回链表，不是8的原因是**避免在阈值附近增加删除元素而引起频繁的链表和红黑树的转换。**

链表查找的时间复杂度是O(n)，红黑树查找的时间复杂度O(logn)

------

**往HashMap中添加键值对的过程**

- 第1步：当你第一次往HashMap集合中存储键值对时，底层会创建一个长度为16的数组
- 第2步：把键然后将键和值封装成一个对象，叫做Entry对象
- 第3步：再根据Entry对象的键计算hashCode值（和值无关）
- 第4步：利用hashCode值和数组的长度做一个类似求余数的算法，会得到一个索引位置
- 第5步：判断这个索引的位置是否为null，如果为null,就直接将这个Entry对象存储到这个索引位置，如果不为null，则还需要进行第6步的判断
- 第6步：继续调用equals方法判断两个对象键是否相同，如果equals返回false，则以链表的形式往下挂，如果equals方法true,则认为键重复，此时新的键值对会替换就的键值对。

------

**HashMap相同键值对判断**

往HashMap集合中存储键值对时，底层调用了键的两个方法：一个是hashCode方法获取键的hashCode值（哈希值）；另一个是调用了键的equals方法，用来比较新添加的键和集合中已有的键是否相同。 

- 只有新添加键的hashCode值和集合中以后键的hashCode值相同、新添加的键调用equals方法和集合中已有键比较结果为true, 才认为元素重复。
- 如果hashCode值相同，equals比较不同，则以链表的形式连接在数组的同一个索引为位置（如上图所示）

往Map集合中存储自定义对象作为键，为了保证键的唯一性，我们需要**重写元素类的hashCode和equals方法**。比如以下面的Student类为例，假设把Student类的对象作为HashMap集合的键，想要让学生的姓名和年龄相同，就认为键重复。

```java
//按快捷键生成hashCode和equals方法
//alt+insert 选择 hashCode and equals
@Override
public boolean equals(Object o) {
    if (this == o) return true;
    if (o == null || getClass() != o.getClass()) return false;
    Student student = (Student) o;
    return age == student.age && Double.compare(student.height, height) == 0 && Objects.equals(name, student.name);
}

@Override
public int hashCode() {
    return Objects.hash(name, age, height);
}
```

------

### LinkedHashMap

**键有序（添加数据的顺序和取出数据的顺序一致），不重复，无索引**

LinkedHashMap它底层采用的是也是哈希表结构，只不过额外新增了一个双向链表来维护元素的存取顺序。每次添加元素，就和上一个元素用双向链表连接一下。第一个添加的元素是双向链表的头节点，最后一个添加的元素是双向链表的尾节点。

![](JavaSEimages\1666171776819-1667311894748.png)

------

### TreeMap

**排序（按照键的大小默认升序排序），不重复，无索引**

TreeMap集合的底层原理和TreeSet也是一样的，底层都是红黑树实现的。所以可以对键进行排序。让TreeMap集合按照指定的规则排序，有两种办法：(类似[Arrays.sort()中的自定义排序规则](#arrays.sort))

------

## Stream流(JDK8)

**Stream流大量结合了Lambda的语法风格来编程**，提供了一种更强大，更简单的方式操作集合或者数组中的数据，代码更简洁，可读性更好。

![](JavaSEimages\767846784678.PNG)

------

### Stream流的创建

![直接上代码演示](JavaSEimages\8678686786.PNG)

```java

// 1、如何获取List集合的Stream流？
List<String> names = new ArrayList<>();
Collections.addAll(names, "张三丰","张无忌","周芷若","赵敏","张强");
Stream<String> stream = names.stream();

// 2、如何获取Set集合的Stream流？
Set<String> set = new HashSet<>();
Collections.addAll(set, "刘德华","张曼玉","蜘蛛精","马德","德玛西亚");
Stream<String> stream1 = set.stream();
stream1.filter(s -> s.contains("德")).forEach(s -> System.out.println(s));

// 3、如何获取Map集合的Stream流？
Map<String, Double> map = new HashMap<>();
map.put("古力娜扎", 172.3);
map.put("迪丽热巴", 168.3);
map.put("马尔扎哈", 166.3);
map.put("卡尔扎巴", 168.3);

Set<String> keys = map.keySet();
Stream<String> ks = keys.stream();

Collection<Double> values = map.values();
Stream<Double> vs = values.stream();

Set<Map.Entry<String, Double>> entries = map.entrySet();
Stream<Map.Entry<String, Double>> kvs = entries.stream();
kvs.filter(e -> e.getKey().contains("巴"))
    .forEach(e -> System.out.println(e.getKey()+ "-->" + e.getValue()));

// 4、如何获取数组的Stream流？
String[] names2 = {"张翠山", "东方不败", "唐大山", "独孤求败"};
Stream<String> s1 = Arrays.stream(names2);
Stream<String> s2 = Stream.of(names2);
```

------

### Stream流中间方法

中间方法指的是：**调用完方法之后其结果是一个新的Stream流，于是可以继续调用方法**，这样一来就可以支持链式编程（或者叫流式编程）。

![](JavaSEimages\1667649509262.png)



```java
List<Double> scores = new ArrayList<>();
Collections.addAll(scores, 88.5, 100.0, 60.0, 99.0, 9.5, 99.6, 25.0);
// 需求1：找出成绩大于等于60分的数据，并升序后，再输出。
scores.stream().filter(s -> s >= 60).sorted().forEach(s -> System.out.println(s));

List<Student> students = new ArrayList<>();
Student s1 = new Student("蜘蛛精", 26, 172.5);
Student s2 = new Student("蜘蛛精", 26, 172.5);
Student s3 = new Student("紫霞", 23, 167.6);
Student s4 = new Student("白晶晶", 25, 169.0);
Student s5 = new Student("牛魔王", 35, 183.3);
Student s6 = new Student("牛夫人", 34, 168.5);
Collections.addAll(students, s1, s2, s3, s4, s5, s6);
// 需求2：找出年龄大于等于23,且年龄小于等于30岁的学生，并按照年龄降序输出.
students.stream().filter(s -> s.getAge() >= 23 && s.getAge() <= 30)
    .sorted((o1, o2) -> o2.getAge() - o1.getAge())
    .forEach(s -> System.out.println(s));

// 需求3：取出身高最高的前3名学生，并输出。
students.stream().sorted((o1, o2) -> Double.compare(o2.getHeight(), o1.getHeight()))
    .limit(3).forEach(System.out::println);
System.out.println("-----------------------------------------------");

// 需求4：取出身高倒数的2名学生，并输出。   s1 s2 s3 s4 s5 s6
students.stream().sorted((o1, o2) -> Double.compare(o2.getHeight(), o1.getHeight()))
    .skip(students.size() - 2).forEach(System.out::println);

// 需求5：找出身高超过168的学生叫什么名字，要求去除重复的名字，再输出。
students.stream().filter(s -> s.getHeight() > 168).map(Student::getName)
    .distinct().forEach(System.out::println);

// distinct去重复，自定义类型的对象（希望内容一样就认为重复，重写hashCode,equals）
students.stream().filter(s -> s.getHeight() > 168)
    .distinct().forEach(System.out::println);

Stream<String> st1 = Stream.of("张三", "李四");
Stream<String> st2 = Stream.of("张三2", "李四2", "王五");
Stream<String> allSt = Stream.concat(st1, st2);
allSt.forEach(System.out::println);
```

------

### Stream流终结方法

调用完Stream流的终结方法之后，其结果就不再是Stream流了，所以不支持链式编程。

![](JavaSEimages\1667649867150.png)

```java
 List<Student> students = new ArrayList<>();
Student s1 = new Student("蜘蛛精", 26, 172.5);
Student s2 = new Student("蜘蛛精", 26, 172.5);
Student s3 = new Student("紫霞", 23, 167.6);
Student s4 = new Student("白晶晶", 25, 169.0);
Student s5 = new Student("牛魔王", 35, 183.3);
Student s6 = new Student("牛夫人", 34, 168.5);
Collections.addAll(students, s1, s2, s3, s4, s5, s6);
// 需求1：请计算出身高超过168的学生有几人。
long size = students.stream().filter(s -> s.getHeight() > 168).count();
System.out.println(size);

// 需求2：请找出身高最高的学生对象，并输出。
Student s = students.stream().max((o1, o2) -> Double.compare(o1.getHeight(), o2.getHeight())).get();
System.out.println(s);

// 需求3：请找出身高最矮的学生对象，并输出。
Student ss = students.stream().min((o1, o2) -> Double.compare(o1.getHeight(), o2.getHeight())).get();
System.out.println(ss);

// 需求4：请找出身高超过170的学生对象，并放到一个新集合中去返回。
// 流只能收集一次。
List<Student> students1 = students.stream().filter(a -> a.getHeight() > 170).collect(Collectors.toList());
System.out.println(students1);

Set<Student> students2 = students.stream().filter(a -> a.getHeight() > 170).collect(Collectors.toSet());
System.out.println(students2);

// 需求5：请找出身高超过170的学生对象，并把学生对象的名字和身高，存入到一个Map集合返回。
Map<String, Double> map =
    students.stream().filter(a -> a.getHeight() > 170)
    .distinct().collect(Collectors.toMap(a -> a.getName(), a -> a.getHeight()));
System.out.println(map);

// Object[] arr = students.stream().filter(a -> a.getHeight() > 170).toArray();
Student[] arr = students.stream().filter(a -> a.getHeight() > 170).toArray(len -> new Student[len]);
System.out.println(Arrays.toString(arr));
```

## File

**File对象的创建**

![](JavaSEimages\1667651303731.png)

```java
public static void main(String[] args) {
    // 1、创建一个File对象，指代某个具体的文件。
    // 路径分隔符
    // File f1 = new File("D:/resource/ab.txt");
    // File f1 = new File("D:\\resource\\ab.txt");
    File f1 = new File("D:" + File.separator +"resource" + File.separator + "ab.txt");
    System.out.println(f1.length()); // 文件大小

    File f2 = new File("D:/resource");
    System.out.println(f2.length());

    // 注意：File对象可以指代一个不存在的文件路径
    File f3 = new File("D:/resource/aaaa.txt");
    System.out.println(f3.length());
    System.out.println(f3.exists()); // false

    // 我现在要定位的文件是在模块中，应该怎么定位呢？
    // 绝对路径：带盘符的
    // File f4 = new File("D:\\code\\javasepromax\\file-io-app\\src\\itheima.txt");
    // 相对路径（重点）：不带盘符，默认是直接去工程下寻找文件的。
    File f4 = new File("file-io-app\\src\\itheima.txt");
    System.out.println(f4.length());
}
```

------

**File判断和获取方法**

![](JavaSEimages\1667659321570.png)

------

**创建和删除方法**

```java
Notice:
1.mkdir(): 只能创建单级文件夹
2.mkdirs(): 才能创建多级文件夹
3.delete(): 文件可以直接删除，但是文件夹只能删除空的文件夹，文件夹有内容删除不了

public static void main(String[] args) throws Exception {
    // 1、public boolean createNewFile()：创建一个新文件（文件内容为空），创建成功返回true,反之。
    File f1 = new File("D:/resource/itheima2.txt");
    System.out.println(f1.createNewFile());

    // 2、public boolean mkdir()：用于创建文件夹，注意：只能创建一级文件夹
    File f2 = new File("D:/resource/aaa");
    System.out.println(f2.mkdir());

    // 3、public boolean mkdirs()：用于创建文件夹，注意：可以创建多级文件夹
    File f3 = new File("D:/resource/bbb/ccc/ddd/eee/fff/ggg");
    System.out.println(f3.mkdirs());

    // 3、public boolean delete()：删除文件，或者空文件，注意：不能删除非空文件夹。
    System.out.println(f1.delete());
    System.out.println(f2.delete());
    File f4 = new File("D:/resource");
    System.out.println(f4.delete());
}
```

------

**遍历文件夹方法**

![](JavaSEimages\1667659732559.png)

```java
NOTICE:
1.当主调是文件时，或者路径不存在时，返回null
2.当主调是空文件夹时，返回一个长度为0的数组
3.当主调是一个有内容的文件夹时，将里面所有一级文件和文件夹路径放在File数组中，并把数组返回
4.当主调是一个文件夹，且里面有隐藏文件时，将里面所有文件和文件夹的路径放在FIle数组中，包含隐藏文件
5.当主调是一个文件夹，但是没有权限访问时，返回null
```

```java
 public static void main(String[] args) {
     // 1、public String[] list()：获取当前目录下所有的"一级文件名称"到一个字符串数组中去返回。
     File f1 = new File("D:\\course\\待研发内容");
     String[] names = f1.list();
     for (String name : names) {
         System.out.println(name);
     }
     
     // 2、public File[] listFiles():（重点）
     // 获取当前目录下所有的"一级文件对象"到一个文件对象数组中去返回（重点）
     File[] files = f1.listFiles();
     for (File file : files) {
         System.out.println(file.getAbsolutePath());
     }

     File f = new File("D:/resource/aaa");
     File[] files1 = f.listFiles();
     System.out.println(Arrays.toString(files1));
 }
```

**递归文件搜索**

案例需求：在`D:\\`判断下搜索QQ.exe这个文件，然后直接输出。

```java
1.先调用文件夹的listFiles方法，获取文件夹的一级内容，得到一个数组
2.然后再遍历数组，获取数组中的File对象
3.因为File对象可能是文件也可能是文件夹，所以接下来就需要判断
	判断File对象如果是文件，就获取文件名，如果文件名是`QQ.exe`则打印，否则不打印
	判断File对象如果是文件夹，就递归执行1,2,3步骤
所以：把1，2,3步骤写成方法，递归调用即可。
```

代码如下：

```java
/**
 * 目标：掌握文件搜索的实现。
 */
public class RecursionTest3 {
    public static void main(String[] args) throws Exception {
          searchFile(new File("D:/") , "QQ.exe");
    }

    /**
     * 去目录下搜索某个文件
     * @param dir  目录
     * @param fileName 要搜索的文件名称
     */
    public static void searchFile(File dir, String fileName) throws Exception {
        // 1、把非法的情况都拦截住
        if(dir == null || !dir.exists() || dir.isFile()){
            return; // 代表无法搜索
        }

        // 2、dir不是null,存在，一定是目录对象。
        // 获取当前目录下的全部一级文件对象。
        File[] files = dir.listFiles();

        // 3、判断当前目录下是否存在一级文件对象，以及是否可以拿到一级文件对象。
        if(files != null && files.length > 0){
            // 4、遍历全部一级文件对象。
            for (File f : files) {
                // 5、判断文件是否是文件,还是文件夹
                if(f.isFile()){
                    // 是文件，判断这个文件名是否是我们要找的
                    if(f.getName().contains(fileName)){
                        System.out.println("找到了：" + f.getAbsolutePath());
                        Runtime runtime = Runtime.getRuntime();
                        runtime.exec(f.getAbsolutePath());
                    }
                }else {
                    // 是文件夹，继续重复这个过程（递归）
                    searchFile(f, fileName);
                }
            }
        }
    }
}
```

## 字符集

**GBK**

- **GBK中一个汉字采用两个字节来存储**，为了能够显示英文字母，GBK字符集也兼容了ASCII字符集，**在GBK字符集中一个字母还是采用一个字节来存储**。
- 如果是**存储字母，采用1个字节来存储，一共8位，其中第1位是0**。如果是**存储汉字，采用2个字节来存储，一共16位，其中第1位是1**。当读取文件中的字符时，通过识别读取到的第1位是0还是1来判断是字母还是汉字，如果读取到第1位是0，就认为是一个字母，此时往后读1个字节。如果读取到第1位是1，就认为是一个汉字，此时往后读2个字节。

**UTF-8**

- UTF-8是一种可变长的编码方案，工分为4个长度区
- 英文字母、数字占1个字节兼容(ASCII编码)
- **汉字字符占3个字节**
- 4极少数字符占4个字节

**编码和解码**

```java
// 1、编码
String data = "a我b";
byte[] bytes = data.getBytes(); // 默认是按照平台字符集（UTF-8）进行编码的。
System.out.println(Arrays.toString(bytes));

// 按照指定字符集进行编码。
byte[] bytes1 = data.getBytes("GBK");
System.out.println(Arrays.toString(bytes1));

// 2、解码
String s1 = new String(bytes); // 按照平台默认编码（UTF-8）解码
System.out.println(s1);

String s2 = new String(bytes1, "GBK");
System.out.println(s2);
```

## IO流

IO流分为两大派系：

1. ​	字节流：字节流又分为字节输入流、字节输出流
2. ​	字符流：字符流由分为字符输入流、字符输出流

![1667924794181](JavaSEimages\1667924794181.png)

### IO流资源释放(JDK7)

Java在JDK7版本为我们提供了一种简化的是否资源的操作，它会自动是否资源。代码写起来也想当简单。

格式如下：

```java
try(资源对象1; 资源对象2;
   //注意：这里只能放置资源对象（流对象）
    //资源都是会实现AutoCloseable接口，资源都会有一个close方法，
    //并且资源放到这里后，用完之后会自动调用其close方法完成资源的释放操作  
   ){
    使用资源的代码
}catch(异常类 e){
    处理异常的代码
}

//注意：注意到没有，这里没有释放资源的代码。它会自动是否资源
```

代码如下：

```java
public static void main(String[] args)  {
    try (
        // 1、创建一个字节输入流管道与源文件接通
        InputStream is = new FileInputStream("D:/resource/meinv.png");
        // 2、创建一个字节输出流管道与目标文件接通。
        OutputStream os = new FileOutputStream("C:/data/meinv.png");
    ){
        // 3、创建一个字节数组，负责转移字节数据。
        byte[] buffer = new byte[1024]; // 1KB.
        // 4、从字节输入流中读取字节数据，写出去到字节输出流中。读多少写出去多少。
        int len; // 记住每次读取了多少个字节。
        while ((len = is.read(buffer)) != -1){
            os.write(buffer, 0, len);
        }
        System.out.println(conn);
        System.out.println("复制完成！！");

    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

在JDK7版本以前，我们可以使用try...catch...finally语句来处理。格式如下

```java
try{
    //有可能产生异常的代码
}catch(异常类 e){
    //处理异常的代码
}finally{
    //释放资源的代码
    //finally里面的代码有一个特点，不管异常是否发生，finally里面的代码都会执行。
}
```

------

### 字节流

![](JavaSEimages\1667823417184.png)

------

**FileInputStream读取一个字节**

使用FileInputStream读取文件中的字节数据，步骤如下

```java
第一步：创建FileInputStream文件字节输入流管道，与源文件接通。
第二步：调用read()方法开始读取文件的字节数据。
第三步：调用close()方法释放资源
```

代码如下：

```java
// 1、创建文件字节输入流管道，与源文件接通。
InputStream is = new FileInputStream(("file-io-app\\src\\itheima01.txt"));

// 2、开始读取文件的字节数据。
// public int read():每次读取一个字节返回，如果没有数据了，返回-1.
int b; // 用于记住读取的字节。
while ((b = is.read()) != -1){
    System.out.print((char) b);
}

//3、流使用完毕之后，必须关闭！释放系统资源！
is.close();
```

这里需要注意一个问题：由于一个中文在UTF-8编码方案中是占3个字节，采用一次读取一个字节的方式，读一个字节就相当于读了1/3个汉字，此时将这个字节转换为字符，是会有乱码的。

------

**FileInputStream读取多个字节**

使用FileInputStream一次读取多个字节的步骤如下

```java
第一步：创建FileInputStream文件字节输入流管道，与源文件接通。
第二步：调用read(byte[] bytes)方法开始读取文件的字节数据。
第三步：调用close()方法释放资源
```

代码如下：

```java
// 1、创建一个字节输入流对象代表字节输入流管道与源文件接通。
InputStream is = new FileInputStream("file-io-app\\src\\itheima02.txt");

// 2、开始读取文件中的字节数据：每次读取多个字节。
//  public int read(byte b[]) throws IOException
//  每次读取多个字节到字节数组中去，返回读取的字节数量，读取完毕会返回-1.

// 3、使用循环改造。
byte[] buffer = new byte[3];
int len; // 记住每次读取了多少个字节。  abc 66
while ((len = is.read(buffer)) != -1){
    // 注意：读取多少，倒出多少。
    String rs = new String(buffer, 0 , len);
    System.out.print(rs);
}
// 性能得到了明显的提升！！
// 这种方案也不能避免读取汉字输出乱码的问题！！

is.close(); // 关闭流
```

- 需要我们注意的是：**`read(byte[] bytes)`它的返回值，表示当前这一次读取的字节个数。**

假设有一个a.txt文件如下：

```java
abcde
```

每次读取过程如下

```java
也就是说，并不是每次读取的时候都把数组装满，比如数组是 byte[] bytes = new byte[3];
第一次调用read(bytes)读取了3个字节(分别是97,98,99)，并且往数组中存，此时返回值就是3
第二次调用read(bytes)读取了2个字节(分别是99,100),并且往数组中存，此时返回值是2
第三次调用read(bytes)文件中后面已经没有数据了，此时返回值为-1
```

- 还需要注意一个问题：采用一次读取多个字节的方式，也是可能有乱码的。因为也有可能读取到半个汉字的情况。

------

**FileInputStream读取全部字节**

![](JavaSEimages\1667830119965.png)

```java
// 1、一次性读取完文件的全部字节到一个字节数组中去。
// 创建一个字节输入流管道与源文件接通
InputStream is = new FileInputStream("file-io-app\\src\\itheima03.txt");

// 2、准备一个字节数组，大小与文件的大小正好一样大。
File f = new File("file-io-app\\src\\itheima03.txt");
long size = f.length();
byte[] buffer = new byte[(int) size];

int len = is.read(buffer);
System.out.println(new String(buffer));

//3、关闭流
is.close(); 
```

![](JavaSEimages\1667830186936.png)

```java
// 1、一次性读取完文件的全部字节到一个字节数组中去。
// 创建一个字节输入流管道与源文件接通
InputStream is = new FileInputStream("file-io-app\\src\\itheima03.txt");

//2、调用方法读取所有字节，返回一个存储所有字节的字节数组。
byte[] buffer = is.readAllBytes();
System.out.println(new String(buffer));

//3、关闭流
is.close(); 
```

最后，还是要注意一个问题：**一次读取所有字节虽然可以解决乱码问题，但是文件不能过大，如果文件过大，可能导致内存溢出。**

------

**FileOutputStream写字节**

使用FileOutputStream往文件中写数据的步骤如下：

```java
第一步：创建FileOutputStream文件字节输出流管道，与目标文件接通。
第二步：调用wirte()方法往文件中写数据
第三步：调用close()方法释放资源
```

代码如下：

```java
// 1、创建一个字节输出流管道与目标文件接通。
// 覆盖管道：覆盖之前的数据
// OutputStream os =
//       new FileOutputStream("file-io-app/src/itheima04out.txt");

// 追加数据的管道
OutputStream os =
    new FileOutputStream("file-io-app/src/itheima04out.txt", true);

// 2、开始写字节数据出去了
os.write(97); // 97就是一个字节，代表a
os.write('b'); // 'b'也是一个字节
// os.write('磊'); // [ooo] 默认只能写出去一个字节

byte[] bytes = "我爱你中国abc".getBytes();
os.write(bytes);

os.write(bytes, 0, 15);

// 换行符
os.write("\r\n".getBytes());

os.close(); // 关闭流
```

------

**字节流复制文件**

复制文件的思路如下所示：

```java
1.需要创建一个FileInputStream流与源文件接通，创建FileOutputStream与目标文件接通
2.然后创建一个数组，使用FileInputStream每次读取一个字节数组的数据，存如数组中
3.然后再使用FileOutputStream把字节数组中的有效元素，写入到目标文件中
```

```java
// 需求：复制照片。
// 1、创建一个字节输入流管道与源文件接通
InputStream is = new FileInputStream("D:/resource/meinv.png");
// 2、创建一个字节输出流管道与目标文件接通。
OutputStream os = new FileOutputStream("C:/data/meinv.png");

// 3、创建一个字节数组，负责转移字节数据。
byte[] buffer = new byte[1024]; // 1KB.
// 4、从字节输入流中读取字节数据，写出去到字节输出流中。读多少写出去多少。
int len; // 记住每次读取了多少个字节。
while ((len = is.read(buffer)) != -1){
    os.write(buffer, 0, len);
}

os.close();
is.close();
System.out.println("复制完成！！");
```

------

### 字符流

**FileReader**

![1667915012716](JavaSEimages\1667915012716.png)

**注意：两个read方法的返回值，含义不一样**

FileReader读取文件的步骤如下：

```java
第一步：创建FileReader对象与要读取的源文件接通
第二步：调用read()方法读取文件中的字符
第三步：调用close()方法关闭流
```

```java
public static void main(String[] args)  {
    try (
        // 1、创建一个文件字符输入流管道与源文件接通
        Reader fr = new FileReader("io-app2\\src\\itheima01.txt");
    ){
        // 2、一个字符一个字符的读（性能较差）
        //            int c; // 记住每次读取的字符编号。
        //            while ((c = fr.read()) != -1){
        //                System.out.print((char) c);
        //            }
        // 每次读取一个字符的形式，性能肯定是比较差的。

        // 3、每次读取多个字符。（性能是比较不错的！）
        char[] buffer = new char[3];
        int len; // 记住每次读取了多少个字符。
        while ((len = fr.read(buffer)) != -1){
            // 读取多少倒出多少
            System.out.print(new String(buffer, 0, len));
        }
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

------

**FileWriter**

![1667915265102](JavaSEimages\1667915265102.png)

FileWriter往文件中写字符数据的步骤如下：

```java
//第一步：创建FileWirter对象与要读取的目标文件接通
//第二步：调用write(字符数据/字符数组/字符串)方法读取文件中的字符
//第三步：调用close()方法关闭流
    
 public static void main(String[] args) {
     try (
         // 0、创建一个文件字符输出流管道与目标文件接通。
         // 覆盖管道
         // Writer fw = new FileWriter("io-app2/src/itheima02out.txt");
         // 追加数据的管道
         Writer fw = new FileWriter("io-app2/src/itheima02out.txt", true);
     ){
         // 1、public void write(int c):写一个字符出去
         fw.write('a');
         fw.write(97);
         //fw.write('磊'); // 写一个字符出去
         fw.write("\r\n"); // 换行

         // 2、public void write(String c)写一个字符串出去
         fw.write("我爱你中国abc");
         fw.write("\r\n");

         // 3、public void write(String c ,int pos ,int len):写字符串的一部分出去
         fw.write("我爱你中国abc", 0, 5);
         fw.write("\r\n");

         // 4、public void write(char[] buffer):写一个字符数组出去
         char[] buffer = {'黑', '马', 'a', 'b', 'c'};
         fw.write(buffer);
         fw.write("\r\n");

         // 5、public void write(char[] buffer ,int pos ,int len):写字符数组的一部分出去
         fw.write(buffer, 0, 2);
         fw.write("\r\n");
     } catch (Exception e) {
         e.printStackTrace();
     }
 }
```

**FileWriter写完数据之后，必须刷新或者关闭，写出去的数据才能生效。**加上了`fw.flush()`方法之后，数据就会立即到目标文件中去。调用了`fw.close()`方法，数据也会立即到文件中去。因为`close()`方法在关闭流之前，会将内存中缓存的数据先刷新到文件，再关流。但是需要注意的是，关闭流之后，就不能在对流进行操作了。否则会出异常

------

### 缓冲流

原始流进行包装，提高原始流读写数据的性能。

![1667915902693](JavaSEimages\1667915902693.png)

------

**字节缓冲流**

缓冲流的底层自己封装了一个长度为**8KB（8129byte）**的字节数组，但是缓冲流不能单独使用，它需要依赖于原始流。在创建缓冲字节流对象时，需要封装一个原始流对象进来。构造方法如下

![1667916924862](JavaSEimages\1667916924862.png)

- **读数据时：**它先用原始字节输入流一次性读取8KB的数据存入缓冲流内部的数组中（ps: 先一次多囤点货），再从8KB的字节数组中读取一个字节或者多个字节（把消耗屯的货）。

- **写数据时：** 它是先把数据写到缓冲流内部的8BK的数组中（ps: 先攒一车货），等数组存满了，再通过原始的字节输出流，一次性写到目标文件中去（把囤好的货，一次性运走）。

如果我们用缓冲流复制文件，代码写法如下:

```java
public static void main(String[] args) {
    try (
        InputStream is = new FileInputStream("io-app2/src/itheima01.txt");
        // 1、定义一个字节缓冲输入流包装原始的字节输入流
        InputStream bis = new BufferedInputStream(is);

        OutputStream os = new FileOutputStream("io-app2/src/itheima01_bak.txt");
        // 2、定义一个字节缓冲输出流包装原始的字节输出流
        OutputStream bos = new BufferedOutputStream(os);
    ){

        byte[] buffer = new byte[1024];
        int len;
        while ((len = bis.read(buffer)) != -1){
            bos.write(buffer, 0, len);
        }
        System.out.println("复制完成！！");

    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

------

**字符缓冲流**

它的原理和字节缓冲流是类似的，它底层也会有一个8KB的数组，但是这里是字符数组。字符缓冲流也不能单独使用，它需要依赖于原始字符流一起使用。

**BufferedReader读数据**

它先原始字符输入流一次性读取8KB的数据存入缓冲流内部的数组中（ps: 先一次多囤点货），再从8KB的字符数组中读取一个字符或者多个字符（把消耗屯的货）。

创建BufferedReader对象需要用到BufferedReader的构造方法，内部需要封装一个原始的字符输入流，我们可以传入FileReader.

![1667919020690](JavaSEimages\1667919020690.png)

而且BufferedReader还要特有的方法，一次可以读取文本文件中的一行

![1667919061356](JavaSEimages\1667919061356.png)

------

**BufferedWriter写数据**

它是先把数据写到字符缓冲流内部的8BK的数组中（ps: 先攒一车货），等数组存满了，再通过原始的字符输出流，一次性写到目标文件中去（把囤好的货，一次性运走）。

创建BufferedWriter对象时需要用到BufferedWriter的构造方法，而且内部需要封装一个原始的字符输出流，我们这里可以传递FileWriter。

![1667919195054](D:\BaiduSyncdisk\javasemd\JavaSEimages\1667919195054.png)

而且BufferedWriter新增了一个功能，可以用来写一个换行符

![1667919243053](D:\BaiduSyncdisk\javasemd\JavaSEimages\1667919243053.png)

------

**缓冲流性能分析**

- 数组越大性能越高，低级流和缓冲流性能相当。相差的那几秒可以忽略不计。

- 缓冲流的性能不一定比低级流高，其实低级流自己加一个数组，性能其实是不差。只不过缓冲流帮你加了一个相对而言大小比较合理的数组 。

- 当数组大到一定程度，性能已经提高了多少了，甚至缓冲流的性能还没有低级流高。


### 转换流

**InputStreamReader & OutputStreamWriter** 

InputStreamReader和OutputStreamWriter也是不能单独使用的，它内部需要封装一个InputStream和OutputStream的子类对象，再指定一个编码表，如果不指定编码表，默认会按照UTF-8形式进行转换。

```java
// 1、得到文件的原始字节流（GBK的字节流形式）
InputStream is = new FileInputStream("io-app2/src/itheima06.txt");
// 2、把原始的字节输入流按照指定的字符集编码转换成字符输入流
Reader isr = new InputStreamReader(is, "GBK");
// 3、把字符输入流包装成缓冲字符输入流
BufferedReader br = new BufferedReader(isr);

// 1、创建一个文件字节输出流
OutputStream os = new FileOutputStream("io-app2/src/itheima07out.txt");
// 2、把原始的字节输出流，按照指定的字符集编码转换成字符输出转换流。
Writer osw = new OutputStreamWriter(os, "GBK");
// 3、把字符输出流包装成缓冲字符输出流
BufferedWriter bw = new BufferedWriter(osw);
```

### 打印流

打印流，这里所说的打印其实就是写数据的意思，它和普通的write方法写数据还不太一样，一般会使用打印流特有的方法叫`print(数据)`或者`println(数据)`，它打印啥就输出啥。打印流有两个，一个是字节打印流PrintStream，一个是字符打印流PrintWriter，PrintStream和PrintWriter的用法是一样的。

**PrintStream和PrintWriter的用法是一样的，所以这里就一块演示了。**

```java
public static void main(String[] args) {
    try (
        // 1、创建一个打印流管道
        // PrintStream ps =
        // new PrintStream("io-app2/src/itheima08.txt", Charset.forName("GBK"));
        // PrintStream ps =
        // new PrintStream("io-app2/src/itheima08.txt");
        PrintWriter ps =
        new PrintWriter(new FileOutputStream("io-app2/src/itheima08.txt", true));
    ){
        ps.print(97);	//文件中显示的就是:97
        ps.print('a'); //文件中显示的就是:a
        ps.println("我爱你中国abc");	//文件中显示的就是:我爱你中国abc
        ps.println(true);//文件中显示的就是:true
        ps.println(99.5);//文件中显示的就是99.5

        ps.write(97); //文件中显示a，发现和前面println方法的区别了吗？

    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

------

**重定向输出语句**

System里面有一个静态变量叫out，out的数据类型就是PrintStream，它就是一个打印流，而且这个打印流的默认输出目的地是控制台，所以我们调用`System.out.pirnln()`就可以往控制台打印输出任意类型的数据。System还提供了一个方法`setOut()`，可以修改底层的打印流，这样我们就可以重定向打印语句的输出目的地了。

```java
public static void main(String[] args) {
    System.out.println("老骥伏枥");
    System.out.println("志在千里");

    try ( PrintStream ps = new PrintStream("io-app2/src/itheima09.txt"); ){
        // 把系统默认的打印流对象改成自己设置的打印流
        System.setOut(ps);

        System.out.println("烈士暮年");	
        System.out.println("壮心不已");
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

此时打印语句，将往文件中打印数据，而不在控制台。

### 数据流

**DataOutputStream**

创建DataOutputStream对象时，底层需要依赖于一个原始的OutputStream流对象。然后调用它的wirteXxx方法，写的是特定类型的数据。

![1667924147403](JavaSEimages\1667924147403.png)

```java
public static void main(String[] args) {
    try (
        // 1、创建一个数据输出流包装低级的字节输出流
        DataOutputStream dos =
        new DataOutputStream(new FileOutputStream("io-app2/src/itheima10out.txt"));
    ){
        dos.writeInt(97);
        dos.writeDouble(99.5);
        dos.writeBoolean(true);
        dos.writeUTF("黑马程序员666！");

    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

------

**DataInputStream**

创建DataInputStream对象时，底层需要依赖于一个原始的InputStream流对象。然后调用它的readXxx()方法就可以读取特定类型的数据。

![1667924375953](JavaSEimages\1667924375953.png)

代码如下：读取文件中特定类型的数据（整数、小数、字符串等）

```java
public static void main(String[] args) {
    try (
        DataInputStream dis =
        new DataInputStream(new FileInputStream("io-app2/src/itheima10out.txt"));
    ){
        int i = dis.readInt();
        System.out.println(i);

        double d = dis.readDouble();
        System.out.println(d);

        boolean b = dis.readBoolean();
        System.out.println(b);

        String rs = dis.readUTF();
        System.out.println(rs);
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

### 序列化流

- 序列化：意思就是把对象写到文件或者网络中去。（简单记：写对象）
- 反序列化：意思就是把对象从文件或者网络中读取出来。（简单记：读对象）

**ObjectOutputStraem**

ObjectOutputStream流也是一个包装流，不能单独使用，需要结合原始的字节输出流使用。

代码如下：将一个User对象写到文件中去

第一步：先准备一个User类，**必须让其实现Serializable接口。**

```java
// 注意：对象如果需要序列化，必须实现序列化接口。
public class User implements Serializable {
    private String loginName;
    private String userName;
    private int age;
    // transient 这个成员变量将不参与序列化。
    private transient String passWord;

    public User() {
    }

    public User(String loginName, String userName, int age, String passWord) {
        this.loginName = loginName;
        this.userName = userName;
        this.age = age;
        this.passWord = passWord;
    }

    @Override
    public String toString() {
        return "User{" +
                "loginName='" + loginName + '\'' +
                ", userName='" + userName + '\'' +
                ", age=" + age +
                ", passWord='" + passWord + '\'' +
                '}';
    }
}
```

第二步：再创建ObjectOutputStream流对象，调用writeObject方法对象到文件。

```java
public class Test1ObjectOutputStream {
    public static void main(String[] args) {
        try (
                // 2、创建一个对象字节输出流包装原始的字节 输出流。
                ObjectOutputStream oos =
                        new ObjectOutputStream(new FileOutputStream("io-app2/src/itheima11out.txt"));
                ){
            // 1、创建一个Java对象。
            User u = new User("admin", "张三", 32, "666888xyz");

            // 3、序列化对象到文件中去
            oos.writeObject(u);
            System.out.println("序列化对象成功！！");

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

注意：写到文件中的对象，是不能用记事本打开看的。因为对象本身就不是文本数据，打开是乱码，怎样才能读懂文件中的对象是什么呢？这里必须用反序列化，自己写代码读。

------

**ObjectInputStream**

ObjectInputStream流也是一个包装流，不能单独使用，需要结合原始的字节输入流使用。接着前面的案例，文件中已经有一个Student对象，现在要使用ObjectInputStream读取出来。称之为反序列化。

```java
public class Test2ObjectInputStream {
    public static void main(String[] args) {
        try (
            // 1、创建一个对象字节输入流管道，包装 低级的字节输入流与源文件接通
            ObjectInputStream ois = new ObjectInputStream(new FileInputStream("io-app2/src/itheima11out.txt"));
        ){
            User u = (User) ois.readObject();
            System.out.println(u);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### IO框架*

apache开源基金组织提供了一组有关IO流小框架，可以提高IO流的开发效率。这个框架的名字叫commons-io：其本质是别人写好的一些字节码文件（class文件），打包成了一个jar包。我们只需要把jar包引入到我们的项目中，就可以直接用了。 

**FileUtils**

![1667925627850](JavaSEimages\1667925627850.png)

在写代码之前，先需要**引入jar包**，具体步骤如下

1. 在模块的目录下，新建一个lib文件夹
2. 把jar包复制粘贴到lib文件夹下
3. 选择lib下的jar包，右键点击Add As Library，然后就可以用了。

```java
public static void main(String[] args) throws Exception {
    //1.复制文件
    FileUtils.copyFile(new File("io-app2\\src\\itheima01.txt"), new File("io-app2/src/a.txt"));

    //2.复制文件夹
    FileUtils.copyDirectory(new File("D:\\resource\\私人珍藏"), new File("D:\\resource\\私人珍藏3"));

    //3.删除文件夹
    FileUtils.deleteDirectory(new File("D:\\resource\\私人珍藏3"));

    // Java提供的原生的一行代码搞定很多事情
    Files.copy(Path.of("io-app2\\src\\itheima01.txt"), Path.of("io-app2\\src\\b.txt"));
    System.out.println(Files.readString(Path.of("io-app2\\src\\itheima01.txt")));
}

```

