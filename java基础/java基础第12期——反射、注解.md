# 一. 反射

反射: 将类的各个组成部分封装为其他对象.

### 1.1 获取class对象的方式

``` 
Class.forName("全类名"):
将字节码文件加载进内存,返回class对象
多用于配置文件,将类名定义在配置文件中,读取文件,加载类

类名.class: 
通过类名的属性class获取
多用于参数的传递

对象.getClass():
多用于对象的获取字节码的方式
```

注意:
以上三种方法获得的字节码文件地址相同
同一个字节码文件在一次程序运行中只会加载一次.

#### 1.1.1 代码演示

示例person:

```
package reflecting;

public class demo00_person {

    public String name;
    protected String name1;
    String name2;
    private String name3;

    //成员方法
    public static void method1(){
        System.out.println("我是空参方法");
    }
    public static void method11(String s){
        System.out.println("我是有参方法"+s);
    }


    public demo00_person() {
    }

    public demo00_person(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "demo00_person{" +
                "name='" + name + '\'' +
                '}';
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

使用:

```
public static void main(String[] args) throws ClassNotFoundException {//异常
        Class cl1 = Class.forName("reflecting.demo00_person");//从包名开始
        Class cl2 = demo00_person.class;
        Class cl3 = demo00_person.class;

        System.out.println(cl1 ==cl2);//true
        System.out.println(cl1==cl3);//true
   }
```

### 1.2 class对象的功能

#### 1.2.1 获取

```
1.成员变量：
Filed[] getFields():  获取public修饰的成员变量
Filed getFiled(String name):  获取public修饰的指定名称的成员变量
Filed[] getDeclaredFields()
Filed getDeclaredField(String name)

2.构造方法：
Constructor<?>[] getConstructors()
Constructor<T> getConstructor(类<?>... parameterTypes)
Constructor<?>[] getDeclaredConstructors()
Constructor<T> getDeclaredConstructor(类<?>... parameterTypes)

3.成员方法:
Method getMethod(String name, 类<?>... parameterTypes),方法名和按声明顺序标识该方法形参类型
Method[] getMethods()
Method getDeclaredMethod(String name, 类<?>... parameterTypes)
method[] getDeclaredMethods()

4.类名
String getName()
```

#### 1.2.2 使用

```
一.成员变量：Field对象操作:
1.设置值:
void set(Object obj, Object value)
2.获取值:
Object get(Object obj)
3.忽略访问权限修饰符的安全检查:
SetAccessible(true): 暴力反射

二. 构造方法Constructor对象
1.创建对象：
T newInstance(Object... initargs)
使用此Constructor对象表示的构造函数，使用指定的初始化参数来创建和初始化构造函数的声明类的新实例。
2.如果创建空参数构造器,可以使用:
class.getDeclaredConstructor().newInstance()
getDeclaredConstructor()根据他的参数对该类的构造函数进行搜索并返回对应的构造函数，
没有参数就返回该类的无参构造函数，然后再通过newInstance进行实例化。
同样有SetAccessible(true)暴力反射.

三. 成员方法Method对象
1. 获取成员方法后,执行方法:
Object invoke(Object obj, Object... args), 传入调用该方法的类的对象和实参
同样有SetAccessible(true).
2. 获取方法名
String getName()
```

#### 1.2.3 代码演示

```
public static void main(String[] args) throws Exception {
        //获取person的Class对象
        Class<demo00_person> p1 = demo00_person.class;
        //获取成员变量
        Field[] fields = p1.getFields();
        for (Field field : fields) {
            System.out.println(field);//person写了四个权限,只获取到public修饰的
        }
        //获取Field对象:名为name的成员变量
        Field name = p1.getField("name");
        //获取值,传入person对象
        demo00_person person = new demo00_person();
        Object value = name.get(person);//获得值,字符串默认值null

        //设置值
        name.set(person,"bronya");
        System.out.println(person);//打印结果name="bronya"

        System.out.println("---------");

        //忽略访问权限的安全检查
        Field dc = p1.getDeclaredField("name3");
        dc.setAccessible(true);//忽略权限
        Object o = dc.get(person);
        System.out.println(o);//null

        System.out.println("---获取构造方法---");
        Constructor<demo00_person> constructor = p1.getConstructor(String.class);
        System.out.println("构造器："+constructor);
        //T newInstance(Object... initargs)方法创建对象
        demo00_person person1 = constructor.newInstance("板鸭");
        System.out.println("创建的对象:"+person);
        //创建空参数构造器
        demo00_person person2 = p1.getDeclaredConstructor().newInstance();
        System.out.println("空参数:"+person2);

        //获取成员方法,传入指定方法名,和形参类型
        Method method1 = p1.getMethod("method1");//空参的
        Method method11 = p1.getMethod("method11", String.class);//有参写法
        //执行方法,传入类对象和实参
        method1.invoke(p1);
        method11.invoke(p1,"666");
        //获取所有public修饰的成员方法
        Method[] methods = p1.getMethods();
        for (Method method : methods) {
            System.out.println(method);//还包括Object类的方法
            System.out.println(
                method.getName()
            );
        }
    }
```

#### 1.2.4 框架应用

```
写个框架,创建任意类的对象,执行其中任意方法.
1.将要创建对象的全类名和需要执行的方法定义在配置文件中
2.在程序中加载配置文件
3.用反射技术加载类文件进内存
4.创建对象, 执行方法
```

src目录下新建一个properties配置文件:

```
className = reflecting.demo00_person
methodName = method11
```

使用:

```
public class demo03_Test {
    public static void main(String[] args) throws Exception {
        //加载配置文件,先创建Properties对象,用load方法加载
        Properties properties = new Properties();
        //获取该字节码文件对应的类加载器,
        ClassLoader classLoader = demo03_Test.class.getClassLoader();
        //调用getResourceAsStream()传入配置文件名称,获取字节输入流
        InputStream ras = classLoader.getResourceAsStream("pro.properties");
        //字节输入流给属性集合
        properties.load(ras);
        //获取配置文件中定义的数据
        String classname = properties.getProperty("className");
        String method = properties.getProperty("methodName");
        //加载该类进内存
        Class<?> aClass = Class.forName(classname);
        //创建类对象
        Object obj = aClass.getDeclaredConstructor().newInstance();
        //获取方法对象,传入方法名和参数
        Method method1 = aClass.getMethod(method,String.class);
        //执行方法,传入类对象和实参
        method1.invoke(obj,"111");
    }
}
```

# 二. 注解

### 2.1 定义

```
注解（Annotation），也叫元数据。一种代码级别的说明。
它是JDK1.5及以后版本引入的一个特性，与类、接口、枚举是在同一个层次。
它可以声明在包、类、字段、方法、局部变量、方法参数等的前面，用来对这些元素进行说明，注释。
```

```
作用分类：
①编写文档：通过代码里标识的元数据生成文档【生成文档doc文档】java doc命令
②代码分析：通过代码里标识的元数据对代码进行分析【使用反射】
③编译检查：通过代码里标识的元数据让编译器能够实现基本的编译检查【Override】
```

### 2.2 jdk内置的注解

```
@Override: 检查该方法是否是继承自父类的
@Deprecated: 该注解标注的内容已经过时(但仍能使用)
@SuppressWarnings: 告诉编译器忽略指定的警告，不用在编译完成后出现警告信息。参数传入all表示全部
```

### 2.3 自定义注解

#### 2.3.1 格式

```
元注解
public @interface 名称{}
```

注解本质是接口,继承java.lang.annotation.Annotation接口.

#### 2.3.2 元注解

元注解是用于描述注解的注解

```
@Target: 表示注解能够作用的位置
        参数ElementType.
                TYPE: 表示注解可以作用到类上
                METHOD: 表示可以作用到方法
                FIELD: 可以作用于成员变量上
```

```
@Retention: 描述注解被保留的阶段
        参数RetentionPolicy.
                SOURCE: 不会保留到Class字节码文件中
                CLASS: 会保留到Class字节码文件中
          (常用) RUNTIME: 当前被描述的注解会保留到Class字节码文件中并被JVM读取到
```

```
@Documented: 描述注解是否被抽取到api文档中
```

```
@Inherited: 描述注解是否被子类继承
```

#### 2.3.3 注解属性

注解里的抽象方法称作注解属性, 

注解属性的要求:

```
1.返回值类型必须为之一: 基本数据类型\String\枚举\注解\以上类型的数组
2.使用时需要赋值, 或者定义属性时用default初始化默认值
```

#### 2.3.4 代码演示

定义一个枚举类:

```
public enum meiju {
    p1,p2,p3;
}
```

定义一个注解类:

```
public @interface an0 {
}
```

自定义注解, 比如用到了枚举类和注解类:

```
@Target({ElementType.TYPE,ElementType.METHOD,ElementType.FIELD})
@Retention(RetentionPolicy.SOURCE)
@Documented
@Inherited
public @interface an1 {
    //定义一个注解里的抽象方法,称注解的属性
    String mt3() default "111";
    int mt4();//基本数据类型
    String mt5();//String类型
    meiju mt6();//枚举类型
    an0 mt7();//注解类型
    String[] mt8();//数组类型
}
```

给注解赋值:

```
//压制该类全部警告
@SuppressWarnings("all")
public class demo00 {
	//表示已经过时
    @Deprecated
    public void mt1(){
        System.out.println("1");
    }
    //注意如何赋值:基本数据类型\String\枚举\注解\以上类型的数组
    @an1(mt4 = 1,mt5 = "1",mt6 = meiju.p1,mt7 = @an0,mt8 = {"1","2","a"})
    public void mt2(){
        mt1();
    }
}
```

### 2.4 解析注解

在程序中使用(解析)注解:  用来替换配置文件

```
步骤:
1. 获取注解定义的位置的对象
2. 获取指定的注解: getAnnotation(class) 方法
3. 调用注解的抽象方法获取配置的属性值
```

首先自定义一个注解:

```
//该注解用来描述需要执行的类名和方法名
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
public @interface demo01 {
    String className();
    String methodName();
}
```

我们要使用的类Annotation.demo00, 假如我们要使用其中的mt1()

```
//压制该类全部警告
@SuppressWarnings("all")
public class demo00 {
	//表示已经过时
    @Deprecated
    public void mt1(){
        System.out.println("1");
    }
    //注意如何赋值:基本数据类型\String\枚举\注解\以上类型的数组
    @an1(mt4 = 1,mt5 = "1",mt6 = meiju.p1,mt7 = @an0,mt8 = {"1","2","a"})
    public void mt2(){
        mt1();
    }
}
```

最后的具体操作:

```
@demo01(className = "Annotation.demo00",methodName = "mt1")
public class demo011 {
    public static void main(String[] args) throws Exception {
        //解析注解
        //获取该类字节码文件对象
        Class<demo011> cls = demo011.class;
        //获取注释对象
        demo01 an = cls.getAnnotation(demo01.class);//在内存中生成了该注解接口的子类实现对象
        //调用注解对象中定义的抽象方法获取返回值
        String className = an.className();
        String methodName = an.methodName();

        //加载该类进内存
        Class<?> aClass = Class.forName(className);
        //创建类对象
        Object obj = aClass.getDeclaredConstructor().newInstance();
        //获取方法对象,传入方法名和参数
        Method method1 = aClass.getMethod(methodName);
        //执行方法,传入类对象和实参
        method1.invoke(obj);
    }
}
```

