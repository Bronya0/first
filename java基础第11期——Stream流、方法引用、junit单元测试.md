# 1.Stream流
Stream流与io流是不同的东西，用于解决集合类库已有的弊端，
## 1.1 获取Stream流：

```java
Collection集合的Stream方法，注意Map集合要经过转化
default Stream<E> stream()
返回以此集合作为源的顺序 Stream 。

Stream<T> filter(Predicate<? super T> predicate)
返回由过滤条件过滤后的流。

void forEach(Consumer<? super T> action)
对此流的每个元素执行操作。即逐一处理。

long count()
返回此流中的元素数。

Stream<T> skip(long n)
在丢弃流的第一个n元素后，返回由该流的n元素组成的流。

static <T> Stream<T> of(T... values)
返回其元素是指定值的顺序排序流。

```

代码演示：

```
public class demo01 {
    public static void main(String[] args) {
        ArrayList<String> array = new ArrayList<>();
        array.add("人类");
        array.add("天地鬼神");
        array.add("天堂");
        array.stream()
                .filter(name->name.startsWith("天"))//首字符
                .filter(name->name.length() == 4)//长度

                .forEach(name-> System.out.println(name));//对每个元素进行输出操作

        //获取流
        //List、Set、Map集合转换Stream流
        List<String> list = new ArrayList<>();
        Stream<String> stream = list.stream();

        Set<String> set = new HashSet<>();
        Stream<String> stream1 = set.stream();

        Map<String,String> map = new HashMap<>();
        Set<String> set1 = map.keySet();//获取键存储到Set集合中
        Stream<String> stream2 = set1.stream();
        Collection<String> values = map.values();//获取值存储到Set集合中
        Stream<String> stream3 = values.stream();
        //获取键值对，（键与值的映射关系entrySet）
        Set<Map.Entry<String, String>> entries = map.entrySet();

        //把数组转换为Stream流
        Stream<Integer> integerStream = Stream.of(1, 2, 3, 4, 5);
        //参数是可变参数可以传递数组
        Integer[] arr  = {1,2,3,4,5};
        Stream<Integer> arr1 = Stream.of(arr);
        String[] arr2 = {"a","bb","ccc"};
        Stream<String> arr21 = Stream.of(arr2);
    }
}
```

## 1.2 处理流的方法:

```
<R> Stream<R> map(Function<? super T, ? extends R> mapper);
将一个流中的元素映射到另一个流中

long count();
统计流中元素的个数

Stream<T> limit(long maxSize)
截取流中前maxSize个元素，参数是一个long型，如果集合当前长度大于参数则进行截取；否则不进行操作。;

Stream<T> skip(long n)：
skip方法获取一个抛弃前n个元素的新流

static <T> Stream<T> concat(Stream<? extends T> a, Stream<? extends T> b)
将两个流组合形成一个新流
```

代码演示：

```
public static void main(String[] args) {
        Stream<String> stringStream = Stream.of("1", "2", "3");
        //map方法的参数通过方法引用，将字符串类型转换成为了int类型，并自动装箱为Integer类对象
        Stream<Integer> integerStream = stringStream.map((String str) -> Integer.parseInt(str));
        System.out.println(integerStream.count());

        //综合练习
        ArrayList<String> arr = new ArrayList();
        arr.add("kiana");
        arr.add("key");
        arr.add("may");
        arr.add("mace");
        arr.add("bronya");
        Stream<String> k = arr.stream().filter(s -> s.startsWith("k")).limit(1);
        k.forEach(System.out::println);
  }
```

# 2. 方法引用

```
方法引用,如果lambda方法体的内容已经存在于某个方法的实现中,
那么我们就不需要再将具体的内容重复写入lambda中,而是直接通过方法引用的写法.自动推导参数和重载
函数式接口的抽象方法的执行体就是lambda的代码块部分,抽象方法参数传给引用的方法
使用方法引用符::
```

定义一个接口：

```
@FunctionalInterface
interface Printable{
    void print(String str);
}
```

一个类：

```
class test{

    void printUpperCase(String str){
        System.out.println(str.toUpperCase());
    }
}
```

引用：

```
public static void main(String[] args) {
        //通过对象引用已经存在的方法,不必再将方法重复写入lambda中
        test test1 = new test();
        printString(test1::printUpperCase);
}
```

## 2.1 通过类名引用静态方法

```
@FunctionalInterface
interface math{
    int calc(int num);
}
```

```
public class demo04 {
    public static void main(String[] args) {
        //
        method(-2,Math::abs);
//        method(-2,(a)->Math.abs(a));
    }
    private static void method(int num,math obj){
        System.out.println(obj.calc(num));
    }
}
```

## 2.2 子类通过super引用父类成员方法:

```
@FunctionalInterface
interface itf{
    void method();
}
```

```
class father{
    void mt1(){
        System.out.println("hello");
    }
}
```

```
public class demo05 extends father {
    @Override
    void mt1() {
        System.out.println("hi");
    }
    private static void mt2(itf obj){
        obj.method();
    }
    public void mt3() {
        mt2(super::mt1);
    }
    public static void main(String[] args) {
        new demo05().mt3();
    }
}
```

## 2.3 使用this关键字引用本类中的方法,被引用方法不可以是静态方法

```
@FunctionalInterface
interface test4{
    void want();
}
```

```
public class demo06 {
    private void mt1(){
        System.out.println("个子不算矮");
    }

    public static void mt2(test4 obj1){
        obj1.want();
    }

    public void mt3() {
        mt2(this::mt1);
    }
    //主方法
    public static void main(String[] args) {
        new demo06().mt3();
    }
}
```

## 2.4 类的构造器引用

* 由于构造器的名称与类名相同,可能修改,所以用 类名::new 的格式表示.

```
interface if4{
    person mt2(String name);
}
```

```
class person{
    private String name;
    public person(String name){
        this.name = name; }
    public String getName(){
        return name; }
    public void setName(String name){
        this.name = name; }
}
```

```
public class demo07 {
    private static void mt3(String name,if4 obj){
        System.out.println(obj.mt2(name).getName());
    }
    public static void main(String[] args) {

        mt3("love",person::new);
       
    }
}
```

# 3. junit单元测试

* 测试分类

```
1.黑盒:不写代码,检测程序输出的是否是期望值
2.白盒:写代码,关注程序运行具体过程
注：junit:白盒测试
```

## 3.1 使用流程

```
1.定义一个测试类:***Test
2.定义测试方法,
方法名:Test***
返回值:void
参数:空
3.测试方法添加@Test注解
4.导入junit依赖环境
```

* 结果判断: 断言

```
Assert.assertEquals(期望值,实际值)
运行时红色表示错误,绿色表示通过
```

* 两个注解

```
1.@Before:
用于资源申请,被注解的方法自动执行于所有测试方法之前
2.@After
用于资源释放,被注解的方法自动执行于所有测试方法之后
```

## 3.2 演示

样本：

```
public class demo08Test {
    public int add(int a, int b){
        return a+b;
    }
}
```

测试类：

```
import org.junit.After;
import org.junit.Assert;
import org.junit.Before;
import org.junit.Test;

//注意不是public会报错
public class demo09 {
    @Before
    public void before(){
        System.out.println("before");
    }
    @Test
    public void TestCalculate(){
        System.out.println("test");
        demo08Test calculate = new demo08Test();
        int result = calculate.add(1, 3);
        //期待2,对比结果
        Assert.assertEquals(2,result);
    }
    @After
    public void after(){
        System.out.println("after");
    }
}
```

