# 一、Scanner类
***
## 1、api简介：
应用程序编程接口
## 2、Scanner类：
 * 作用：获取键盘输入的数据
 * 位置： java.util.Scanner.
* 使用：使用成员方法==nextInt()== 和 ==next()== 分别接收整型和字符串类型数据
```
		//将Scanner类实例化，并用System.in表示键盘输入
        Scanner scanner = new Scanner(System.in);
        //使用成员方法nextInt() 和next() 分别接收整型和字符串类型数据
		int num = scanner.nextInt();
		String str = scanner.next()
		
```
* //next()和nextLine()的区别详解：
next()方法在读取内容时，会过滤掉有效字符前面的无效字符，对输入有效字符之前遇到的空格键、Tab键或Enter键等结束符，next()方法会自动将其过滤掉；只有在读取到有效字符之后，next()方法才将其后的空格键、Tab键或Enter键等视为结束符；所以next()方法不能得到带空格的字符串。
* nextLine()方法字面上有扫描一整行的意思，它的结束符只能是Enter键，即nextLine()方法返回的是Enter键之前没有被读取的所有字符，它是可以得到带空格的字符串的。
 
# 二、匿名对象
*** 
* 格式： new 类名称();
* 注意：new一次使用一次，彼此互不关联
* 使用：匿名对象作方法的返回值：
```
	public static void main(String[] args){
		method(new Scanner(System.in))
	}
	//匿名对象作方法的返回值
	public static void method(Scanner sc){
		int num = sc.nextInt();
		String str = sc.next();
	}
```
# 三、Random类
***
* 作用：获取随机数
* 位置：java.util.Random；
* 方法1：==nextInt()== : 返回一个任意范围随机数。 
* 方法2：==nextInt(int bound)==: 返回一个0~(bound-1)范围内的随机数。
```
	Random random = new Random();
	int num1 = random.nextInt();
	int num2 = random.nextInt(11);//0~10
	int num3 = sandom.nextInt(10)+1;//1~10
```
# 四、ArrayList集合类
***
* 作用：长度可以改变的集合
* 位置：java.util.ArrayList;
* 注意：ArrayList的泛型只能是引用类型，不能是基本数据类型；可通过基本数据类型的包装类解决
* 使用：
public boolean ==add(E e)==： 添加元素，新增元素需要与泛型一致
   public E ==get(int index)== ：从集合中获取元素，参数是索引编号，返回值就是对应位置的元素
   public E ==remove(int index)== ：从集合中删除元素，参数是索引编号，返回值是被删除掉的元素
   public int ==size()==：获取集合的尺寸长度，返回值是集合中包含的元素个数
```
//普通数组
	int[] array1 = new int[长度];
//对象数组,可存放该类型的对象
	对象类型[] 数组名 = new 对象类型[长度];
//数组长度不可变，因此可用长度可变的集合代替数组
	 //ArrayList<E>的尖括号中的E代表泛型，指装在集合中的所有元素都是统一的类型
     //泛型只能是引用类型，不能是基本类型
     //注意创建语句的写法，右边ArrayList<>()
        ArrayList<String> array = new ArrayList<>();
     //ArrayList直接打印出来的是内容，不是地址值，内容为空时打印中括号
        System.out.println(array);
```
```
public static void main(String[] args) {
        //创建
        ArrayList<String> list = new ArrayList<>();
        System.out.println(list);//[]
        //添加add
        list.add("原子裂变");
        list.add("核子聚变");
        list.add("高维打击");
        //[原子裂变, 核子聚变, 高维打击]
        System.out.println(list);
        //获取get
        String string = list.get(1);//将返回值存入string中
        System.out.println(string);
        //删除remove
        list.remove(2);
        System.out.println(list);
        //获取集合中的元素个数size
        int num = list.size();
        System.out.println(num);
    }
  ```
  ```
  //ArrayList<int> array = new ArrayList<>();
        // 错误语句，ArrayList的泛型只能是引用类型，不能是基本数据类型；
        // 集合里保存的都是地址值，但基本数据类型没有地址值
        // 基本类型的使用需要对应的“包装类”
/*
    基本类型      包装类
       byte          Byte
       short         Short
       int           Integer  *
       long          Long
       float         Float
       double        Double
       char          Character *
       boolean       Boolean
 */
        ArrayList<Integer> array = new ArrayList<>();
        array.add(12345);
        array.add(13265);
        array.add(15433);
        System.out.println(array);

        int a = array.get(2);
        int b = array.size();
        System.out.println(a);
        System.out.println(b);
  ```
  
# 五、标准的类
***
```
//一个标准的类，应该包含以下四部分

public class demo07test2 {
    //private 成员变量
    private String name;
    private int age;
    //无参构造
    public demo07test2(){
    
    }
    //全参构造
    public demo07test2(String name,int age){
        this.name = name;
        this.age = age;
    }
    //get set方法
    public String getName(){
         return name;
    }
    public void setName(String name){
        this.name = name;
    }
    public int getAge(){
        return age;
    }
    public void setAge(int age){
        this.age = age;
    }
}
```
# 六、String类
***
* 作用：字符串数据处理
* 位置： java.lang.String；
* 注意：
 1.String类是不可变类，字符串是常量，String对象创建后不可被修改直至被销毁
2.字符串可以共享使用，避免重复创建，节省内存
3.字符串效果上是char[]数组，但java9后是byte[]再加一个encoding-flag来存储字段，一个字符串的每个字符只占用1个字
* 创建字符串的4种方式：
三种构造方法：
 public String():创建一个空白字符串，不含有任何内容；
 public String(char[] array):根据字符数组的内容创建相应的字符串；
 public String(byte[] array):根据字节数组的内容创建对应的字符串；
一种直接创建方法：
 String str = "hello world":双引号直接创建了字符串对象（JVM自动帮你new了对象）
 ```
 	   String str1 = new String();
        System.out.println(str1);//第一种方法

        char[] array1 = {'n','e','w'};
        String str2 = new String(array1);
        System.out.println(str2);//第二种方法

        byte[] array2 = {'b','e'};
        String str3 = new String(array2);
        System.out.println(str3);//第三种方法

        String str4 = "newbe";
        System.out.println(str4);//直接创建字符串对象
   ```
   * 字符串比较方法：
   public boolean ==equals(object obj)==：参数可以是任意对象，只有参数是一个字符串且内容相同时才给true，否则false。任何对象都能用object进行接收。==equalsIgnoreCase==方法可以忽略大小写比较
   * 字符串获取相关
   public int ==length()==: 获取字符串的字符个数；
public String ==concat(String str)==：将当前字符串和参数字符串拼接成为返回值新的字符串；
public char ==charAt(int index)==: 获取指定索引位置的单个字符。索引从0开始；
public int ==indexOf(String str)==: 查找参数字符串在在本字符串当中首次出现的索引位置，如果没有则返回-1；
* 字符串的截取方法：
public String ==substring(int index)==:截取从参数位置一直到字符串末尾，返回新的字符串；
public String ==substring(int begin, int end)==:截取从begin开始，一直到end结束中间的字符串；左闭右开
* 字符串转换相关方法
public char[] ==toCharArray()==:  将当前字符串拆分成字符数组作为返回值；
public byte[] ==getBytes()==: 获得当前字符串底层的字符数组；
public String ==replace(CharSequence oldString,charSequence newString)==: 将所有出现的老字符串替换成新的字符串；
* 字符串分割相关
 public String[] ==split(String regex)==:按参数的规则将字符串切割成若干部分；
若regex为".",则要写成“\\.” (正则表达式)

**比较**：
```
	String str1 = "newbe";
	String str2 = "newbe";
	System.out.println(str1.equals(str2));//true
	System.out.println("newbe".equalsIgnoreCase("NEWBE"));//true
```
**获取**：
```
 public static void main(String[] args) {
 		//获取字符串个数，length方法()
        int num = "newswonderxswlawsl".length();
        System.out.println(num);

        String str1 = "awsl";
        String str2 = "xswl";
        String str3 = "2333";
        
        //拼接str3.str2,contact方法
        String str4 = str3.concat(str2);
        String str5 = str4.concat(str1);
        System.out.println(str5);

		//查找str1中第三个字符，charAt(int index)方法
        char char1 = str1.charAt(2);
        System.out.println(char1);
		
		//查找str1中s的索引值2，indexOf(String str)方法
        int num2 = str1.indexOf('s');
        int num3 = str1.indexOf('c');//没有则返回-1
        System.out.println(num2);
        System.out.println(num3);
    }
```
**截取**：
```
public static void main(String[] args) {
        String str1 = "2333xswl";
        String str2 = str1.substring(4);//xswl
        String str3 = str1.substring(4,7);//xsw
        System.out.println(str2);
        System.out.println(str3);
    }
```
**转换**

```
public static void main(String[] args ){
        String str1 = "青山不改，绿水长流";
        System.out.println(str1);

        char[] char1 = str1.toCharArray();
        System.out.println(char1);//打印 青山不改，绿水长流

        byte[] byte2 = str1.getBytes();
        System.out.println(byte2);//打印[B@10f87f48
        System.out.println(byte2[2]);//打印-110

        String sre2 = str1.replace("不","也");
        System.out.println(sre2);//打印青山也改，绿水长流
    }
```
**分割**：
```
public static void main(String[] args) {

        String str1 = "aaa,bbb,ccc";
        String[] str2 = str1.split(",");
        for (int i = 0; i < str2.length; i++) {
            System.out.println(str2[i]);	//aaa bbb ccc
        }

        String str3 = "aaa bbb ccc";
        String[] str4 = str3.split(" ");
        for (int i = 0; i < str4.length; i++) {
            System.out.println(str4[i]); //aaa bbb ccc
        }

        String str5 = "aaa.bbb.ccc";
        String[] str6 = str5.split(".");
        for (int i = 0; i < str6.length; i++) {
            System.out.println(str6[i]);	//无
        }

        String[] str8 = str5.split("\\.");
        for (int i = 0; i < str8.length; i++) {
            System.out.println(str8[i]);	//aaa bbb ccc
        }
    }
```
**如何统计字符串中各种字符出现的次数？**
```
public class demo08statistics {

    public static void main(String[] args) {
        String str1 = "abbCCC";
        int a=0,b=0,C=0;
        char[] chararray1 = str1.toCharArray();
        for (int i = 0; i < chararray1.length; i++) {
            if (chararray1[i] == 'a') a++;
            if (chararray1[i] == 'b') b++;
            if (chararray1[i] == 'C') C++;
        }
        System.out.println("a有"+ a + "个");
        System.out.println("b有" + b +"个");
        System.out.println("C有" + C + "个");
    }
}
```
# 七、static静态
***
* 注意：
用了static关键字的内容属于类，所有对象共享同一份；
static修饰成员变量时，该变量不再属于对象自己，而是属于所在的类；
执行时静态内容总是在前，非静态在后
静态方法中不能使用this关键字，this代表当前对象， 静态方法不涉及对象；
* 方法调用相关
没有static的成员方法需要一个对象才能使用它；
成员方法通过*创建对象再调用*，静态方法还可以且推荐*直接用类名称调用*（方便区分成员方法），
当使用对象名调用静态方法时实质上也是转换成通过类名称调用；
对于*本类中的静态方法，可以省略类名称*直接调用；
*成员方法可以直接访问成员变量和静态变量，但静态方法只能直接访问静态变量*，
原因：内存中现有的静态内容，后有的动态内容，先来的没见过后来的，但后来的听过先来者；



```
 public static void main(String[] args) {
        demo01basic one = new demo01basic();//创建对象，引用类型
        one.method1();//通过对象调用成员方法
        demo01basic.method2();//通过类调用静态方法
        method3();//对于本类中的静态方法，可以省略类名称直接调用；
    }

    public static void method3(){
        System.out.println("xiuer");
    }

    int num1;//成员变量
    static int num2;//静态变量

    public void method4(){
        System.out.println(num1);
        System.out.println(num2);//成员方法可以访问成员变量和静态变量
    }

    public static void method5(){
//        System.out.println(num1); 报错，静态方法无法访问成员变量
        System.out.println(num2);//静态方法可以访问静态变量
    }

```
* ==静态代码块==：
 ```
    public class ***{
        static{
            静态代码块的内容；
        }
    }
    当第一次用到本类时，静态代码块执行唯一的一次;第二次执行时不会运行静态代码块
  ```
  # 八、Arrays工具类
  * 作用：实现数组的常见操作
  * 位置：java.util.arrays；
  * 使用：
  java.util.arrays是一个与数组相关的工具类，里面提供了大量的静态方法，用来实现数组的常见操作；
public static String ==toString(数组)==: 将参数数组变成字符串，默认格式[元素1，元素2，元素3...]
public static void ==sort(数组)==：按照默认升序（从小到大）对数组元素进行排序；如果是自定义的类型，需要有Comparable或者Comparactor接口的支持；
```
    public static void main(String[] args) {

        char[] array1 = {'2','2','3','3','强','者'};	//定义一个数组
        System.out.println(array1);		//2233强者
        System.out.println(Arrays.toString(array1));	//[2, 2, 3, 3, 强, 者]


        int[] array2 = {2,4,1,3,6,5};
        Arrays.sort(array2);
        System.out.println(Arrays.toString(array2));	//; 注意语法 [1, 2, 3, 4, 5, 6] 

        String[] array3 = {"aaa","ccc","bbb"};
        Arrays.sort(array3);
        System.out.println(Arrays.toString(array3));	//字符串默认按照字母升序排列 [aaa, bbb, ccc]
    }
```
# 八、Math类
***
* 作用：这类 Math包含用于执行基本的数字运算等基本指数、对数、平方根法、三角函数。 
* 位置：java.lang.Object；
* 方法：
public static double ==abs(double num)==:获取绝对值
public static double ==ceil(double num)==: 向上取整
public static double ==floor(double num)==: 向下取整
public static long ==round(double num)==: 四舍五入
Math.PI：获取PI的值
```
public static void main(String[] args) {

        System.out.println(Math.abs(-3.14));//3.14
        System.out.println(Math.ceil(3.14));//4.0
        System.out.println(Math.floor(3.14));//3.0
        System.out.println(Math.round(3.14));//3
        System.out.println(Math.PI);//3.141592653589793

    }
```

* 关注我，一起学习、一起进步，无限期更新，期待下一期哦！
* Bye!

