# 1、入门
使用场景：如果创建**函数式接口**（该接口的抽象方法只能有一个）的实例时，使用Lambda表达式更加简洁方便。

# 2、格式：
(形参列表) -> { 代码块 }

# 3、简化
只有一个参数时，可以省略圆括号();
代码块只有一条语句，可以省略花括号{}；
代码块里只有一个语句时，即是需要return，也可以省略return关键字（仅仅是关键字），
需要返回值时自动返回这条省略了return的语句的值。

# 4、限制：
1、Lambda表达式的类型必须是函数式接口（不是的话可以强制类型转换，但与目标类型的唯一抽象方法要有相同的形参列表）；

2、Lambda表达式只能为函数式接口创建对象；

函数式接口典型例子：
XxxFunction\XxxConsumer\Predicate\XxxSupplier

3、java8为函数式接口提供了@FunctionalInterface注解，放在接口定义前面，告诉编译器对该接口进行检查是否为函数式接口。

```java
public class Demo01 {
    //创建Runnable对象,Runnable是一个函数式接口
    public static void main(String[] args) {
        Runnable r = ()-> System.out.println("Runnable");
        Runnable r1 = () -> {
            for (int i = 0; i < 2; i++) {
                System.out.println(i);
            }
		};
```
# 5、补充
**方法引用和构造器引用简化写法**
条件：如果代码块只有一条语句时，还可以进一步简化lambda写法。

1.引用类方法
格式： 类名::方法
说明： 接口中被实现方法的全部参数 传给该类方法作参数。

2.引用特定对象的实例方法
格式： 特定对象::实例方法
说明： 接口中被实现方法的全部参数 传给该类方法作参数。

3.引用某类对象的实例方法
格式： 类名::实例方法
说明： 接口中被实现方法的第一个参数作为调用者，剩余参数作为该方法的参数

4.引用构造器
格式： 类名::new
说明： 接口中被实现方法的全部参数 传给该构造器作参数。

```java
//定义一个函数式接口
@FunctionalInterface
interface Demo02 {
    //只有一个抽象方法convert()
    Integer convert(String from);
}
@FunctionalInterface
interface Demo021{
    String test(String a,int b,int c);
}
@FunctionalInterface
interface  Demo022{
    JFrame win(String title);
}
```

```java
class Demo03{
    public static void main(String[] args) {
        //调用Integer类的valueOf方法
        Demo02 obj1 = Integer::valueOf;
        //调用"99"对象的indexOf()实例方法实现
        Demo02 obj2 = "99"::indexOf;
        //被实现方法的第一个参数作为调用者
        //后面全部参数传给该方法作参数
        Demo021 obj3 = String::substring;
        //构造器引用
        Demo022 obj4 = JFrame::new;
    }
}
```
# 6、区别
lambda表达式与匿名内部类的区别：

lambda表达式是匿名内部类的一种简化，

* 相同点：
1、都可以直接访问effectively final局部变量以及外部类的成员变量（包括实例变量和类变量）；
2、二者创建的对象都可以直接调用从接口中继承的默认方法。

* 不同点：
1、匿名内部类可为任意接口创建实例，无论有多少抽象方法，lambda只能为函数式接口创建实例。
2、匿名内部类可以为抽象类或普通类创建实例，后者只能为函数式接口创建实例。
3、匿名内部类实现的抽象方法的方法体允许调用接口定义的默认方法，lambda表达式的代码块不允许调用接口定义的默认方法。
***
下期介绍File类和IO流。

