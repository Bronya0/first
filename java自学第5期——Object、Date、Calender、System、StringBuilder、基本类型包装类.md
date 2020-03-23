# 一、Object类
***
* 作用：对象操作
* 位置：java.lang.Object
* 方法：
 public String ==toString()== ：返回对象的字符串表示形式。  
public boolean ==equals(Object obj)==： 指示是否有其他对象“等于”这一个。  
* 注意：Object类里的toString方法
 Object类是所有类的终极父类。
person类默认继承了Object类，所以可以使用Objcct类里的方法，返回该对象的字符串表示。通常，toString 方法会返回一个“以文本方式表示”此对象的字符串。
结果应是一个简明但易于读懂的信息表达式。建议所有子类都重写此方法。
Object 类的 toString 方法返回一个字符串，
该字符串由类名（对象是该类的一个实例）、at 标记符“@”和此对象哈希码的无符号十六进制表示组成。
换句话说，该方法返回一个字符串，它的值等于：
*getClass().getName() + '@' + Integer.toHexString(hashCode())*
打印地址值没有意义，所以要重写toString方法，打印对象属性
* equals方法源码：
```
public boolean equals(Object obj){
    return(this == obj);
}
//this指调用equals方法的那个对象

```
* ==  : 对于基本数据类型，比较的是数值；对于引用数据类型，比较的是地址值。
比较地址值无意义，所以要重写equals方法，比较两个对象的属性。
* 代码示范：

```
public class Demo02Equals {
    private String name;
    private int age;

    public static void main(String[] args) {
        Demo03Class obj1 = new Demo03Class();
        Demo03Class obj2 = new Demo03Class();
        //打印obj1地址值
        System.out.println(obj1);
        //打印obj2地址值
        System.out.println(obj2);
        //打印二者地址值对比结果，false
        System.out.println(obj1.equals(obj2));
        //将obj2地址值赋给obj1
        obj1 = obj2;
        //打印true
        System.out.println(obj1.equals(obj2));
    }

//    @Override
//    public boolean equals(Object obj) {
//        //将equals源码中Object类型的obj向下转型为子类Demo03Class
//        //目的是为了调用子类Demo03Class里的方法。
//        Demo03Class p = (Demo03Class) obj;
//        //比较两个对象的属性，this(Demo03Class-p)和obj(Demo03Class-)
//        boolean a = this.name.equals(p.name) && this.age == p.age;
//        return a;
//    }
}
```
# 二、Date类
***
* 作用：表示时间和日期的类
* 位置：java.util.Date
* 注意：
类Date表示特定的瞬间，精确到毫秒（千分之一秒）。
毫秒值的作用：可以对时间和日期进行计算:
1、 获取当前计算机系统时间到英国格林威治时间【1970年1月1日00:00:00】经历了多少毫秒——System.currentTimeMillis方法
2、一天 24h =  86400*1000 ms
3、将毫秒的差值转换为日期

**注意：中国为东八区，时间加8个小时，中国此时为1月1日08:00:00.**
* 方法：
public ==Date()==: 空参构造方法，获得当前系统时间
public ==Date(long date)==：实参构造方法，传递毫秒值，把毫秒值转换为日期
public long ==getTime()==：成员方法，返回原点时间到目前系统时间的毫秒差值，相当于之前的System.currentTimeMillis()方法。

```
public static void main(String[] args) {
        //获取当前计算机系统时间到【1970年1月1日00:00:00】经历了多少毫秒
        System.out.println(System.currentTimeMillis());
    }
```

```
public static void main(String[] args) {
        method1();
    }

        private static void method1() {
            Date date1 = new Date();//导包后创建Date类的对象（Date()空参构造方法）
            System.out.println(date1);//获得当前系统时间Sat Aug 24 17:43:27 CST 2019
            //（CST指中国的标准时间）

            Date date2 = new Date(0L);
            System.out.println(date2);//Thu Jan 01 08:00:00 CST 1970 原点时间
            Date date3 = new Date(1566041013344L);
            System.out.println(date3);//原点时间加毫秒差值 得出现在时间

            Date date4 = new Date();
            long time = date4.getTime();//使用成员方法
            System.out.println(time);//返回原点时间到目前系统时间的毫秒差值

        }
```
* DateFormat是一个[抽象类]的子类格式的日期/时间格式化和解析日期或独立于语言的方式时间。
日期时间格式的子类，如 SimpleDateFormat，允许的格式（例如，日期→文本），分析（文本→日期），和标准化。
日期表示为 Date对象或自1970年1月1日00:00:00 GMT毫秒，
DateFormat提供的分类方法很多，用于获取默认日期/时间基于默认或指定的场所和一系列的格式样式格式化。
抽象类无法直接创建对象，我们使用它的子类*SimpleDateFormat*.
* 构造方法：
==SimpleDateFormat(String pattern)==:用给定的模式和默认语言环境的日期格式符号构造:
参数String pattern:  传递指定的格式，详细见api文档
区分大小写：
        ==y:年 M:月 d:日 H:时 m:分 s:秒==
例如："yyyy-MM-dd HH:mm:ss"
注意模式中的字母不可改变，但是连接符号可以更改。例如：
"yyyy年MM月dd日 HH时mm分ss秒"
* 成员方法：
 String ==format(Date date)==:	按照指定的格式，将Data日期格式化为符合模式的字符串。
Date ==parse(String source)==:	把符合模式的字符串解析为Date日期

* parse方法声明了一个异常ParseException,当要转换的字符串格式与构造方法规定的不同时便会报此异常
两种解决方法：throws继续抛出这个异常:鼠标移动至异常处，按Alt+Enter声明该异常，交给虚拟机处理。
或者try catch自己处理

```
public static void main(String[] args) throws ParseException {
        method2();
    }

    /*
    创建SimpleDateFormat对象，构造方法中传递指定的模式。
    调用SimpleDataFormat对象中的方法format(),把Date日期格式化为符合模式的字符串（文本）。
     */
    private static void method2() throws ParseException {
        //创建SimpleDateFormat类的对象
        SimpleDateFormat simpleDateFormat1 = new SimpleDateFormat();

        Date date = new Date();//创建date
        //date作为参数传给SimpleDataFormat对象的format方法
        String format = simpleDateFormat1.format(date);
        System.out.println(date);//输出Sat Aug 17 21:11:50 CST 2019
        System.out.println(format);//输出2019/8/17 下午9:11

        SimpleDateFormat simpleDateFormat2 = new SimpleDateFormat();
        Date parse = simpleDateFormat2.parse("2019/8/17 下午9:11");
//        Date parse = simpleDateFormat2.parse("2019/8/17 下午9:"); 删掉秒，发现解析异常ParseException
        System.out.println(parse);//输出 2019/8/17 下午9:11
    }
```
# 三、Calender类
***
* 作用：calendar是一个*抽象类*，里面提供了很多提供操作日历字段的方法
* 位置：java.lang.Object 
* 注意：
**西方月份是0-11月**
日历类calendar
calendar是一个抽象类，calendar无法直接创建对象，
主要使用【getInstance】静态方法，静态方法直接调用，不用创建对象
该方法返回了calendar类的子类对象。
* 静态方法：
==getInstance==：静态方法直接调用，不用创建对象
该方法返回了calendar类的子类对象。
public static final int ==MONTH==;返回月份
public static final int ==YEAR==
public static final int ==MINUTE==
public static final int ==SECONDE==

* Calendar类常用的成员方法：
public int ==get(int field)==:返回给定日历字段的值。
public void ==set(int field,int value)==:将给定的日历字段设定为给定值。
public abstract void ==add(int field,int amount)==:根据日历规则，为给定的日历字段添加或者减去指定的时间量。
public Date ==getTime()==:返回一个表示此Calendar时间值（从历元到现在的毫秒偏量）的Date对象。
*参数：*
int field :日历类的字符段，可以使用calendar类的静态成员变量获取。
int value: 要设置的数值
int value: 正数增加，负数减少

```
private static void method4(){
        //使用getInstance直接返回日历对象
        Calendar calendar1 = Calendar.getInstance();
        //调用对象的get方法，参数为Calendar类的静态方法YEAR，得到年）
        int i = calendar1.get(Calendar.YEAR);
        System.out.println(i);//输出2019
        int momth = calendar1.get(Calendar.MONTH);
        System.out.println(momth);//打印7，在中国即为8月。
        int day = calendar1.get(Calendar.DAY_OF_MONTH);
        System.out.println(day);//输出17
    }

    private static void method5(){
        Calendar calendar = Calendar.getInstance();
        calendar.set(Calendar.YEAR,9999);//设置年份
        calendar.set(Calendar.MONTH,11);//设置月份

        int year = calendar.get(Calendar.YEAR);
        System.out.println(year);//输出年份9999

        int month = calendar.get(Calendar.MONTH);
        System.out.println(month);//输出月份11

        // 使用另一个不同参数列表的set方法，同时设置年月日。
        calendar.set(2019,1,3);

        Calendar calendar3 = Calendar.getInstance();
        calendar3.add(Calendar.YEAR,1);//增加1年

        //由2019年变成了2020年。
        System.out.println(calendar3.get(Calendar.YEAR));

        calendar3.add(Calendar.YEAR,-2);//减少2年
        //2018年
        System.out.println(calendar3.get(Calendar.YEAR));

        Calendar calendar5 = Calendar.getInstance();
        //把日历变成日期
        Date time = calendar5.getTime();
        //输出Sat Aug 17 22:36:45 CST 2019
        System.out.println(time);
        
    }
```
# 四、System类
***
* 作用：由System类提供的设施包括标准输入、标准输出和错误输出流；访问外部定义的属性和环境变量；加载文件和库的方法；和一种快速复制数组的一部分的实用方法。
* 位置：java.lang.System 

* 方法：
public static long ==currentTimeMillis()==
返回当前时间以毫秒为单位。请注意，返回值的时间单位为毫秒，则值的粒度取决于底层操作系统，并且可能更大。例如，许多操作系统测量单位为几十毫秒的时间。
public static void ==arraycopy(Object src, int srcPos, Object dest,int destPos,int length)==
从指定的源数组中复制一个数组，开始在指定的位置，到目标数组的指定位置。
src:	源数组
srcPos：源数组中的起始位置
dest：目标数组
destPos：目标数组中的起始位置
length：要复制的数组元素的数量

```
private static void method1(){
        long time1 = System.currentTimeMillis();
        //得到原点1970.1.1到现在所差的毫秒值1566371047409
        System.out.println(time1);
    }
```

```
public static void main(String[] args) {
        int src[] = {1,2,3,4,5};
        int desk[] = {6,7,8,9,10};
        System.arraycopy(src,0,desk,0,3);
        //输出[1,2,3,9,10]
        System.out.println(Arrays.toString(desk));

    }
```
# 五、StringBuilder类
***
* 作用：StringBuilder类，字符串缓冲区，可以提高字符串的操作效率。
可以看作一个长度可以变化的字符串。
* 位置：java.lang.StringBuilder 
* 注意：
**字符串是常量，它们的值不能被创建后改变**。支持可变字符串字符串缓冲区。
字符串的底层是被final修饰的数组private final byte[] value;
因为字符串对象是不可改变的，所以它们可以被共享。
进行字符串的相加，内存中是多个字符串相加，占用内存多，效率低下。
StringBuilder类，字符串缓冲区，可以提高字符串的操作效率。
可以看作一个长度可以变化的字符串。
该类底层也是一个数组，但是无final修饰，因此可以改变长度。
byte[] value = new byte[16];
初始长度16。
字符串会依次填入StringBuider数组中，容量不够时会自动扩容，因此始终占据一个数组
内存占用小，效率高。
* 构造方法：
1. public ==StringBuilder()==
构造一个没有字符的字符串生成器，并构造了16个字符的初始容量。
2. public ==StringBuilder(String str)==
构造一个初始化为指定字符串的内容的字符串生成器。该字符串生成器的初始容量是 16加字符串的长度。
参数 str ：缓冲区的初始内容。
* 成员方法：
1.  public StringBuilder ==append(...)== 添加任意类型的字符串，并返回当前对象自身。
由于返回的是对象本身，因此无需接受返回值
*链式编程：*
方法返回值是一个对象，因此可以继续调用方法：
a.append(1).append("e")...

2.  public String ==toString()==
返回表示该序列中的数据的字符串。一个新的 String对象被初始化为包含目前代表这个对象的字符序列。
这 String然后返回。这个序列的后续变化不影响内容的 String。

* String 和 StringBuilder**相互转换**：
String 到 StringBuilder:	使用StringBuilder的构造方法 StringBuilder(String str)
StringBuilder 到 String ：使用StringBuider中的toString方法.

```
public static void main(String[] args) {
        //空构造方法
        StringBuilder stringBuilder1 = new StringBuilder();
        System.out.println(stringBuilder1);//空的
        //有参构造方法
        StringBuilder stringBuilder2 = new StringBuilder("我是参数");
        System.out.println(stringBuilder2);

    }
```

```
 public static void main(String[] args) {
        StringBuilder stringBuilder1 = new StringBuilder("kiana");
//      该写法不对，无需接受返回值
//      StringBuilder stringBuilder2 = stringBuilder1.append(" love 芽衣");
        stringBuilder1.append(" 喜欢 芽衣");
        //输出： kiana love 芽衣
        System.out.println(stringBuilder1);

        //链式编程
        stringBuilder1.append(", 而我").append("喜欢").append("所有人");
        //输出： kiana 喜欢 芽衣, 而我喜欢所有人
        System.out.println(stringBuilder1);

        //String 转换 StringBuilder
        String str = "love";
        StringBuilder stringBuilder3 = new StringBuilder(str);
        //输出love
        System.out.println(stringBuilder3);

        //StringBuilder转为String
        StringBuilder stringBuilder6 = new StringBuilder("女武神Valkykie");
        stringBuilder6.toString();
        //输出女武神Valkykie
        System.out.println(stringBuilder6);

    }
```
# 六、基本类型包装类
***
* 作用：基本类型的数据效率高，但功能少，为了将其当作对象使用，可以使用基本数据类型对应的包装类。
除了int 为 Integer,char 为 Character外，其他只是首字母大写即可。
* **装箱**：把基本数据类型的数据包装到包装类中
* Integer的构造方法：
1. ==Integer(int value)==
构建了一个新分配的 Integer表示指定的 int价值。
2. ==Integer(String s)==
构建了一个新分配的 Integer表示 int值表示的 String参数。

*注意，2中的参数字符串必须是基本数据类型的字符串，不可是其他类型，否则抛出异常
例如："123"对 ， "一二三"错误*

* 两个静态方法：
1.public static Integer ==valueOf(int i)==
返回一个 Integer实例表示指定的 int价值。
如果一个新的 Integer实例是不需要的，这种方法一般应优先使用构造函数 Integer(int)，
这种方法可能会产生显着更好的空间和时间，通过缓存经常请求的价值表现。
此方法将总是在范围内的缓存值- 128至127，包含，并可能缓存在此范围以外的其他值。
*参数*
		i - int价值。
结果
一个 Integer实例代表 i。

2. public static Integer ==valueOf(String s)==
                       throws NumberFormatException
返回一个 Integer对象持有指定的 String值。参数的解释是代表一个十进制整数，
就像争论给 parseInt(java.lang.String)方法。其结果是一个 Integer表示的字符串指定的整数值。
换句话说，这个方法返回一个Integer对象相等的价值：
new Integer(Integer.parseInt(s))
*参数*
s ：被解析的字符串。
结果
一个 Integer对象持有价值为代表的字符串参数。
异常
NumberFormatException如果不能将字符串解析为一个整数。
***

* **拆箱**：在包装类中取出基本数据类型。
* 拆箱成员方法：
public int intValue()
作为一个 int返回该 Integer价值。
Specified by:
intValue 方法重写，继承类  Number
结果
这代表的对象转换为数值型 int后。
* 自动装箱与自动拆箱：基本数据类型和包装类可以自已自动转换。自jdk1.5开始。
自动装箱写法：Integer value = 1;
自动拆箱： 进行value + 1运算时自动将包装类的value转换为int类型。
***
* 基本数据类型和字符串类型的相互转换。
**基本转字符串**：
1.基本类型 + 空字符串“”
2.包装类的静态方法toString(参数)，注意不是Object类的toString().
static String toString(int i),返回一个表示指定整数的String对象。
3.String类的静态方法valueOf(int i)返回int参数的字符串表示形式。
static String valueOf(int i)
**字符串转基本**：
1.使用包装类的静态方法parse***("数值类型的字符串")
Integer类： static int parseInt(String s)
Double类: static double parseInt(String s)
```
public static void main(String[] args) {
        //构造方法示例：
        Integer integer1 = new Integer("123");
        Integer integer2 = new Integer(123);
        //123,重写了toString方法
        System.out.println(integer1);
        //123,重写了toString方法
        System.out.println(integer2);

        //静态方法
        Integer integer3 = Integer.valueOf(123);
        Integer integer4 = Integer.valueOf("123");
        //123,都是将其变成了包装类
        System.out.println(integer3);
        System.out.println(integer4);

        //拆箱：包装类变成基本数据类型
        int int1 = integer3.intValue();
        int int2 = integer2.intValue();
        //123
        System.out.println(int1);
        System.out.println(int2);

    }
```

```
public static void main(String[] args){
        //基本转字符串3个方法
        int int1 = 1;
        //方法1
        String string1 = int1+"";
        //方法2
        String string2 = Integer.toString(int1);
        //方法3
        String string3 = String.valueOf(int1);
        //1
        System.out.println(string1);

        //字符串转基本,字符串必须是数值类型
        int int2 = Integer.parseInt(string1);
        System.out.println(int2);//1

    }
```

  *  关注我，一起学习、一起进步，无限期更新，期待下一期哦！
 *   Bye!

