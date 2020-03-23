# 1、IO简介
IO（输入输出）通过java.io包下的类和接口来支持，包下包括输入、输出两种IO流，每种输入输出流又可分为字符流和字节流两大类。

# 2、File类
File类是io包下与平台无关的文件和目录，File能新建、删除、重命名文件和目录，不能访问文件本身，后者需要使用输入输入流。

##  2.1 构造方法
File类的构造方法：

File(File parent, String child)     参数：父路径，子路径
          根据 parent 抽象路径名和 child 路径名字符串创建一个新 File 实例。
File(String pathname)
          通过将给定路径名字符串转换为抽象路径名来创建一个新 File 实例。
File(String parent, String child)
          根据 parent 路径名字符串和 child 路径名字符串创建一个新 File 实例。
File(URI uri)
          通过将给定的 file: URI 转换为一个抽象路径名来创建一个新的 File 实例。 
 
 ##  2.2 静态方法         
File类静态方法：

static String ==pathSeparator==()
          与系统有关的路径分隔符，为了方便，它被表示为一个字符串。
static char ==pathSeparatorChar==()
          与系统有关的路径分隔符。
static String ==separator==()
          与系统有关的默认名称分隔符，为了方便，它被表示为一个字符串。
static char ==separatorChar==()
          与系统有关的默认名称分隔符。
          
## 2.3 常用方法：

### 2.3.1 获取相关
public String ==getAbsolutePath==()
        返回此File的绝对路径名字符串。
public String ==getPath==()
        将此File转换为路径名字符串。
public String ==getName==()
        返回由此File表示的文件或目录的名称。
public long ==length==()
        返回由此File表示的文件的长度。不能获取文件夹大小，返回0。如果路径不存在则返回0。
```java
//静态方法直接通过类名调用
    public static void main(String[] args) {
        System.out.println(File.pathSeparator);//打印 ;
        System.out.println(File.pathSeparatorChar);//打印 ;

        System.out.println(File.separator);//打印 \
        System.out.println(File.separatorChar);//打印 \

        File obj1 = new File("D:\\test.txt");//以文件结尾的路径
        System.out.println("文件绝对路径：" + obj1.getAbsolutePath());
        System.out.println("文件构造路径：" + obj1.getPath());
        System.out.println("文件名：" + obj1.getName());
        System.out.println("文件长度：" + obj1.length());

        File obj2 = new File("D\\TEST");//以文件夹结尾的路径
        System.out.println("目录绝对路径：" + obj2.getAbsolutePath());
        System.out.println("目录构造路径：" + obj2.getPath());
        System.out.println("目录名：" + obj1.getName());
        System.out.println("目录长度：" + obj2.length());
        /*
        文件绝对路径：D:\test.txt
        文件构造路径：D:\test.txt
        文件名：test.txt
        文件长度：3字节

        目录绝对路径：D:\Coding\demo01\D\TEST
        目录构造路径：D\TEST
        目录名：test.txt
        目录长度：0
         */
```



### 2.3.2 判断相关
boolean ==exists==()
          测试此抽象路径名表示的文件或目录是否存在。
boolean ==isFile==()
          测试此抽象路径名表示的是否是文件，而不是目录。
boolean ==isDirectory==()
          测试此抽象路径名表示的文件是否是一个目录。
boolean ==isAbsolute==()
          测试此抽象路径名是否为绝对路径名。
注意：路径必须存在，否则isFile()和isDirectory()都返回false。

### 2.3.3 操作相关
boolean ==createNewFile==()
         当次File对象对应的文件不存在时，新建一个该File对象所指定的新文件，创建成功返回true，反之false。
boolean ==delete==()
         删除File对象对应的文件或路径。
boolean ==mkdir==()
         创建此抽象路径名指定的目录。
boolean ==mkdirs==()
         创建此抽象路径名指定的目录，包括所有必需但不存在的父目录。
     


         
### 2.3.4 遍历目录：
String[] ==list==()
          返回一个字符串数组，这些字符串指定此抽象路径名表示的目录中的文件和目录。
String[] list(FilenameFilter filter)
          返回一个字符串数组，这些字符串指定此抽象路径名表示的目录中满足指定过滤器的文件和目录。
File[] listFiles()
          返回一个抽象路径名（File）数组，这些路径名表示此抽象路径名表示的目录中的文件。
File[] listFiles(FileFilter filter)
          返回抽象路径名（File）数组，这些路径名表示此抽象路径名表示的目录中满足指定过滤器的文件和目录。
File[] ==listFiles==(FilenameFilter filter)
          返回抽象路径名（File）数组，这些路径名表示此抽象路径名表示的目录中满足指定过滤器的文件和目录。

注意：如果list()和listFiles()遍历的目录不存在，或者构造方法给的路径不是目录，
都会抛出空指针异常NullPointerEcxeption;该遍历非能获取到隐藏的文件或目录

路径分隔符：Windows下为分号; Linux下为冒号:
默认名称分隔符：Windows下为反斜杠\，Linux下为正斜杠/
```java
public static void main(String[] args) {
        File f1 = new File("D:\\TEST");
        System.out.println(f1.exists());
        System.out.println(f1.isFile());
        System.out.println(f1.isDirectory());
        System.out.println(f1.isAbsolute());

        File f2 = new File("E:\\FireFox");
        String[] str1 = f2.list();//list()返回字符串数组
        //增强for遍历字符串数组
        for (String str:str1) {
            System.out.println(str);
        }
        //返回的是File类型的数组
        File[] file = f2.listFiles();
        //遍历目录
        for (File f : file){
            System.out.println(f);
        }
    }
```


### 2.3.5 绝对路径和相对路径：
绝对路径：从盘符开始的路径
相对路径：相对于当前项目的根目录的路径
注意：路径不区分大小写；


## 2.4 递归
递归：
在当前方法调用自己

分类：
直接递归，间接递归（a调用b，b调用a）

要求：
1、必须要有条件限定结束方法，不认栈内存溢出
2、递归次数不能太多，不然也会内存溢出
3、构造方法不允许递归，创建的对象太多

使用前提：
调用方法时，方法的主体不变，但每次调用的参数在不断改变
如果仅仅是求1到n的和，建议用for循环，递归会导致频繁的创建调用销毁方法，效率低。

```java
public static void main(String[] args) {
        //递归累加求和
        int min = 1;
        int max = 4;
        int sum = a(min,max);
        System.out.println(sum);

        //递归计算阶乘n!
        int num = 4;
        int sum2 = b(num);
        System.out.println(sum2);
    }

    private static int a(int min,int max){
        if ( max==1){
            return 1;
        }else {
            return max + a(min, --max);
        }
    }

    private  static int b(int num){
        if (num == 1){
            return 1;
        }else {
            return num * b(--num);
        }
    }
```

遍历多级文件夹，遇到文件直接打印路径，判断为文件夹则递归遍历这个文件夹，不断重复，停止条件为没有文件夹
文件搜索，根据结尾扩展名，使用String类的==endswith==方法


```java
 public static void main(String[] args) {
        File f = new File("D:\\Coding\\demo01\\demo1\\src");
        //遍历所有文件
        a(f);
        System.out.println("______________");
        //文件搜索
        b(f);

    }

    private static void a(File f){
        //返回路径里的所有File文件到数组里
        File[] f1 =  f.listFiles();
        //遍历
        for (File f2 : f1) {
            //是文件就直接打印路径
            if (f2.isFile()){
                System.out.println("文件名："+f2.getAbsolutePath());
            }else {//是文件夹就对其进行递归，不断重复，知道都是文件
                System.out.println("目录列表："+f2.getAbsolutePath());
                a(f2);
            }
        }
    }

    private static void b(File f){
        File[] f1 = f.listFiles();
        for (File f2:f1) {
            if (f2.isFile()){
                //是文件就判断是不是.java后缀
                //endswith判断后缀，因为是String类的方法 要把f2转化（getname），toLowerCase是将JAVA这种大写后缀统一降为小写
                if (f2.getName().toLowerCase().endsWith(".java")){
                    System.out.println(f2);
                }
            }else {
                b(f2);
            }
        }
```
### 2.4.1 文件过滤器：
File类的list方法可以接受FilenameFilter类型作参数，通过该参数可以进行筛选
FilenameFilter是接口，只有一个抽象方法accept,可以使用lambda表达式书写
boolean ==accept==(File dir, String name)
          对指定File子目录和文件迭代，如果符合条件则返回true，则list方法列出该子目录或者文件
```java
public static void main(String[] args) {
        File f = new File("D:\\Coding\\demo01\\demo1\\src\\Interface");
//        a(f);
        //如果是以.java结尾，或者是文件夹，则返回true。
        String[] nameList = f.list((dir,name) -> name.endsWith(".java") || new File(name).isDirectory());
        for(String name : nameList){
            System.out.println(name);
        }
    }
//    private static void a(File f){
//        File[] f1 = f.listFiles();
//        for (File f2 : f1){
//            if (f2.isDirectory()){
//                a(f2);
//            }
//            String[] f3 = f2.list((dir, name) -> name.toLowerCase().endsWith(".java"));
//            for(String f4:f3){
//                System.out.println(f4);
//            }
//        }
//    }
```
# 3、流Stream
Java传统的流类型（类或抽象类）在java.io包中，Java把各种输入输出源抽象为流stream，流是从起源source到接收sink的数据

IO分为分为输入流（从外部读取到内存程序）和输出流（从程序到外部），各自又分为字符流和字节流（二者区别仅仅为操作的数据单元不同，字符16位，字节8位），

**输入流两个基类：InputStream（字节流），Reader（字符流）
输出流两个基类：OutputStream（字节流），Writer（字符流）**

如果处理文本内容，建议使用字符流，如果处理二进制内容，建议字节流。

按照流的角色分类可以分为  节点流(对直接和指定文件关联)和 处理流，处理流是对节点流的一种封装



IO资源不属于内存，垃圾回收机制不回收，使用close方法手动关闭流,IO类都实现了AutoCloseable接口,都可通过trycatch关闭IO流


## 3.1 输入流 
### 3.1.1 字节输入流抽象基类:InputStream：
int read()：  从输入流中读取单个字节，返回字节数据
int ==read==(byte[] b)： 从输入流中最多读取b.length个字节，并存储在字节数组b中，返回读取的字节数
int read(byte[] b,int off,int len)：  从输入流中最多读取b.length个字节的数据，存储在数组b中，不是从数组起点开始，而是从off位置开始，返回读取的字节数
void ==close==() 
          关闭此输入流并释放与该流关联的所有系统资源。 




### 3.1.2 字符输入流抽象基类:Reader：
int read()：  从输入流中读取单个字节，返回字节数据
int ==read==(char[] b)： 从输入流中最多读取b.length个字节，并存储在字节数组b中，返回读取的字节数
int read(char[] b,int off,int len)：  从输入流中最多读取b.length个字节的数据，存储在数组b中，不是从数组起点开始，而是从off位置开始，返回读取的字节数
void ==close==() 
          关闭此输入流并释放与该流关联的所有系统资源。

二者几乎一样。


InputStream/Reader都是抽象类，**不能直接创建实例**，但他们有读取文件的输入流：
FileInputStream和FileReader，
二者都是节点流

### 3.1.3 字节输入流:FileInputStream
是抽象类InputStream的实现类，可以拿来创建对象。FileInputStream继承了InputStream的方法。
构造方法可以传路径：

FileInputStream(==File file==) 
         	  通过打开一个到实际文件的连接来创建一个 FileInputStream，该文件通过文件系统中的 File 对象 file 指定。
### 3.1.2 字符输入流:FileReader
是抽象类Reader的实现类，可以拿来创建对象。FileReader继承了Reader的方法。
构造方法可以传路径：
FileReader(File file) 
          在给定从中读取数据的 File 的情况下创建一个新 FileReader。

```java
public static void main(String[] args) throws IOException {
        //FileInputStream
        //创建字节输入流，读取自身.name是路径名
        FileInputStream fis = new FileInputStream("D:\\Coding\\demo01\\demo1\\src\\IO\\Demo06_IO.java");
        //创建长度为24的竹筒
        byte[] b = new byte[1024];
        //计数器
        int i = 0;
        //循环输入,只要还有数据就一直执行
        while((i = fis.read(b)) >0){
            //取出数据，将数组转换成字符串
            System.out.println(new String(b,0,i));
        }
        //打印源代码
        //使用close方法关闭输入流
        fis.close();

        //FileReader
        try (
                FileReader f = new FileReader("D:\\Coding\\demo01\\demo1\\src\\IO\\Demo06_IO.java");
        ){
            char[] f1 = new char[32];
            int f3 = 0;
            while ((f3 = f.read(f1)) > 0) {
                System.out.println(new String(f1, 0, f3));
            }
        }catch (IOException e){
            e.printStackTrace();
        }
    }
```
## 3.2 输出流
输出流抽象基类：
抽象字节输出流OutputStream、抽象字符输出流Writer

方法与输入流基本相似，不过将read换成了write。

Writer还包换了额外方法：

void ==writer==(String str): 将str字符串里的字符输出到指定输出流中
void write(String str,int off,int len): 将str字符串里从off开始，长为len的字符输出到指定输出流中

**win下简体中文字符集默认GBK，linux下默认UTF-8**
### 3.2.1 字节输出流:FileOutputStream
构造方法：
FileOutputStream(File file) 
          创建一个向指定 File 对象表示的文件中写入数据的文件输出流。
          
成员方法
 void ==write==(byte[] b) ：
          将 b.length 个字节从指定 byte 数组写入此文件输出流中。 
          
 void write(byte[] b, int off, int len) 
          将指定 byte 数组中从偏移量 off 开始的 len 个字节写入此文件输出流。 
 void write(int b) 
          将指定字节写入此文件输出流。
   

 
  ### 3.2.2 字符输出流：FileWriter
FileWriter(File file) 
          根据给定的 File 对象构造一个 FileWriter 对象。

```java
public static void main(String[] args) {
        try(
                //创建字节输入流
                FileInputStream f0 = new FileInputStream("D:\\Coding\\demo01\\demo1\\src\\IO\\Demo05.java");
                //创建字节输出流
                FileOutputStream f1 = new FileOutputStream("D:\\Coding\\test.txt");
        ){
          byte[] by = new byte[8];
          int i = 0;
          while ( (i = f0.read(by) ) > 0){
              f1.write(by,0,i);
          }
        }catch (IOException e){
            e.printStackTrace();
        }

        //直接输出字符串内容
        try (
                FileWriter writer = new FileWriter("D:\\Coding\\test2.txt");
        ){
            //win平台换行符\r\n,Linux平台\n
            writer.write("ddd \r\n");
            writer.write("666 \r\n");
        }catch (IOException e){
            e.printStackTrace();
        }


    }
```
## 3.3 处理流
以上流程比较繁琐，使用处理流(高级流，节点流又叫低级流)简化：
处理流可以忽视底层设备节点流的差异，并提供更方便的方法，除了方便程序员，本身执行效率也高

使用PrintStream包装OutputStream(@1),再直接使用PrintLn方法打印

关闭流时，只需关闭最上层的处理流即可，系统自动关闭其包装的节点流

除了用数组作为节点外，字符流还可以用字符串作为物理节点StringReader，见下两层代码

```java
public static void main(String[] args) {
        try(
                //创建字节输出流
                FileOutputStream f1 = new FileOutputStream("D:\\Coding\\test3.txt");
                //@1
                //包装该节点流，节点流当做参数传给构造器
                PrintStream ps = new PrintStream(f1);){
            //直接打印字符串
            ps.println("处理流 \r\n");
            //也可以输出对象
            ps.println(new Demo08());
        }catch (IOException e){
            e.printStackTrace();
        }

        String str = "fighting!\n";
        char[] ch = new char[32];
        int hasRead = 0;
        try(
                StringReader sr = new StringReader("D:\\Coding\\test2.txt");
                ) {
            while ((hasRead = sr.read(ch)) > 0){
                System.out.println(new String(ch,0,hasRead));
            }
        }catch (IOException e){
            e.printStackTrace();
        }
```
## 3.4 转换流
转换流：用于将字节流转换为字符流（不存在字符流转字节流，没有必要）

InputStreamReader：将字节输入流转换为字符输入流
OutputStreamWriter：将字节输出流转换为字符输出流

代码示范 将键盘输入System.in转换为字符输出流的过程，
由于InputStreamReader依然不是很方便，再包装成BufferedReader,
利用BufferedReader的readLine方法一次读取一行（以换行符为标志，没有则程序阻塞，按回车继续。）
 String ==readLine==()
          读取一个文本行。
      

```java
public static void main(String[] args) {
        try (
                //将System.in对象转换为Reader对象
                InputStreamReader a = new InputStreamReader(System.in);
                //将普通的Reader包装成BufferedReader
                BufferedReader b = new BufferedReader(a);

                ){
            String line = null;

            while ((line = b.readLine()) != null){
                if (line.equals("exit")){
                    System.exit(1);
                }
                System.out.println("输出内容：" + line);
            }
        }catch (IOException e){
            e.printStackTrace();
        }
    }
```
将GBK编码的文本文件，转换为UTF-8编码的文本文件。
        案例分析
        1. 指定GBK编码的转换流，读取文本文件。
        2. 使用UTF-8编码的转换流，写出文本文件。
        案例实现
    
```java
public static void main(String[] args) throws IOException {
        // 1.定义文件路径
        String srcFile = "D:\\Coding\\test.txt";
        String destFile = "D:\\Coding\\GBK.txt";
        // 2.创建流对象
        // 2.1 转换输入流,指定GBK编码
        InputStreamReader isr = new InputStreamReader(new FileInputStream(srcFile) , "GBK");
        // 2.2 转换输出流,默认utf8编码
        OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream(destFile));
        // 3.读写数据
        // 3.1 定义数组
        char[] cbuf = new char[1024];
        // 3.2 定义长度
        int len;
        // 3.3 循环读取
        while((len=isr.read(cbuf))!=-1){
        // 循环写出
        osw.write(cbuf,0,len);
        }
        // 4.释放资源
        osw.close();
        isr.close();
        }
```


## 3.5 缓冲流
缓冲流：又叫高效流，是四个基本流的增强，速度非常快。在创建流对象时，会创建一个内置的默认大小的缓冲区数组，通过缓冲区读写，减少系统IO次数，从而提高读写的效率

字节缓冲流：
BufferedInputStream （输入流），
BufferedOutputStream（输出流）
构造方法：
public BufferedInputStream(InputStream in) ：创建一个新的缓冲输入流。
public BufferedOutputStream(OutputStream out) ：创建一个新的缓冲输出流。

字符缓冲流：
BufferedReader（输入流） ，
BufferedWriter（输出流）
构造方法：
public BufferedReader(Reader in) ：创建一个新的缓冲输入流。
public BufferedWriter(Writer out) ：创建一个新的缓冲输出流。

复制文件速度测试：

```java
public static void main(String[] args) throws FileNotFoundException {
        //记录开始时间
        long start = System.currentTimeMillis();
        //创建流对象
        try(
                BufferedInputStream a = new BufferedInputStream(new FileInputStream("E:\\素材\\哔哩哔哩\\67370009\\1\\67370009_1_0.mp4"));
                BufferedOutputStream b = new BufferedOutputStream(new FileOutputStream("E:\\素材\\哔哩哔哩\\67370009\\1.mp4"));
                ){
             int len = 0;
             byte[] byt = new byte[8*1024];
             while ((len = a.read(byt)) > 0){
                 b.write(byt,0,len);
             }
        }catch (IOException e){
                    e.printStackTrace();
        }
        long end = System.currentTimeMillis();
        System.out.println(end-start);
    }
```
## 3.6 与io有关的属性集合Properties
java.util.Properties 继承于Hashtable，来表示一个持久的属性集。它使用键值结构存储数据，每个键及其对应值都是一个字符串。

该类也被许多Java类使用，比如获取系统属性时，System.getProperties 方法就是返回一个Properties 对象。

构造方法：
public Properties() :创建一个空的属性列表。

基本方法:
public Object ==setProperty==(String key, String value) ：保存一对属性。
public String ==getProperty==(String key) ：使用此属性列表中指定的键搜索属性值。
public Set<String> ==stringPropertyNames==() ：所有键的名称的集合。
 void ==load==(InputStream inStream) ：     从输入流中读取属性列表（键和元素对）。 
 void load(Reader reader) ：  按简单的面向行的格式从输入字符流中读取属性列表（键和元素对）。 
 void ==store==(OutputStream out, String comments) ：  以适合使用 load(InputStream) 方法加载到 Properties 表中的格式，将此 Properties 表中的属性列表（键和元素对）写入输出流。 
 void store(Writer writer, String comments) ： 以适合使用 load(Reader) 方法的格式，将此 Properties 表中的属性列表（键和元素对）写入输出字符。 


```java
public static void main(String[] args) {
        Properties properties = new Properties();
        //添加键值对元素，键和值都默认的字符串
        properties.setProperty("1","one");
        properties.setProperty("2","two");
        properties.setProperty("3","three");
        //打印属性集对象
        System.out.println("属性集："+properties);
        //通过键，获取属性值
        System.out.println(properties.getProperty("1"));
        System.out.println(properties.getProperty("2"));
        System.out.println(properties.getProperty("3"));
        //遍历属性集，获取包含所有键的集合
        Set<String> set = properties.stringPropertyNames();
        //打印键值对
        for (String str : set) {
            System.out.println(str + "对应" + properties.getProperty(str) );
        }
```
### 3.6.1 store输出和load输入
***
Properties集合的store方法，将集合中的临时数据写入到硬盘中保存

1、字节输出流：
void ==store==(OutputStream out, String comments)：
以适合使用 load(InputStream) 方法加载到 Properties 表中的格式，将此 Properties 表中的属性列表（键和元素对）写入输出流。

3、字符输出流：
void ==store==(Writer writer, String comments)：
              以适合使用 load(Reader) 方法的格式，将此 Properties 表中的属性列表（键和元素对）写入输出字符。
              
3、参数：
OutputStream out:字节输出流，不能写中文
writer：字符输出流，可以写中文
comments：注释，说明文件用处。不能用中文，默认Unicode编码，可用空的字符串" "代替
——————————————————————————————————
Properties集合里的load方法：
可以把键值对的文件，读取到集合里使用

1、字节输入流：
void ==load==(InputStream inStream)
            从输入流中读取属性列表（键和元素对）。
            
2、字符输入流：
void ==load==(Reader reader)
            按简单的面向行的格式从输入字符流中读取属性列表（键和元素对）。
            
参数：
InputStream：字节输入流，不能读取含有中文的键值对
Reader：能读取中文键值对

使用步骤：
1.创建属性集合对象
2.用load方法读取含有键值对的文件
3.遍历属性集合

**注意**：
1.存储键值对的文件中，键与值默认的连接符号使用=或者空格
2.存储键值对的文件中，可以使用#进行注释，被注释的键值将不会被读取
3.存储键值对的文件中，键与值默认都是字符串，不用再加引号

```java
public static void main(String[] args) throws IOException {
        //创建Properties属性集合，并初始化数据
        Properties properties2 = new Properties();
        properties2.setProperty("1","one");
        properties2.setProperty("2","two");
        properties2.setProperty("3","three");
        //创建字符输出流对象（字节输出流不能用中文）
        FileWriter fw = new FileWriter("D:\\Coding\\属性集合.txt");
        //使用集合的store方法，将集合的临时数据写入硬盘.注释不能写中文
        properties2.store(fw,"hello");
        //关闭输出流
        fw.close();


        //创建属性集合对象
        Properties pp = new Properties();
        //使用load方法读取文件
        pp.load(new FileReader("D:\\Coding\\属性集合.txt"));
        //将key值统一到集合中
        Set<String> set2 = pp.stringPropertyNames();
        for (String str2:set2) {
//          //通过key值访问value
            String property = pp.getProperty(str2);
            System.out.println(property);

        }
    }
```
## 3.6.2 对象的序列化
把对象以流的方式，写入到文件中保存，叫写对象，或对象的序列
ObjectOutputStream：对象的序列化流

把文件中保存的对象，以流的方式读取出来，叫对象的反序列化
ObjectInputStream：对象的反序列化流

一、**ObjectOutputStream**继承了OutputStream,其构造方法的参数是传入一个OutputStream

成员方法： void ==writeObject==(Object obj)
                将指定的对象写入ObjectOutputStream
                
使用步骤：
1.创建ObjectOutputStream对象，构造方法传递字节输出流
2.使用ObjectOutputStream对象的writeObject把对象写入文件中
3.资源释放

**注意**：
序列化和反序列化时，会抛出NotSerizable未序列化异常
类通过实现java.io.**Serialzable**接口以实现其序列化功能，未实现此接口的类无法使其序列化或者反序列化
Serializable接口叫标记型接口，接口里没有什么方法。实现该接口时会给类添加一个标记
有这个标记才允许序列化或者反序列化，没有则抛异常。

二、java.io.**ObjectInputStream** extends InputStream
作用：把文件中保存的对象，以流的方式读取出来使用

构造方法：
传入字节输入流InputStream

成员方法：
Object readObject()：从ObjectInputStream读取对象
该方法抛出了ClassNotFoundException(class文件找不到异常)，说明不存在对象的class文件

反序列化前提：
1.类必须实现Serilizable接口
2.必须存在对应的class文件

使用步骤：
1、创建ObjectInputStream对象，构造方法传入字节输入流
2、使用readObject方法读取保存对象的文件
3.释放资源
4.打印对象

```java
public static void main(String[] args) throws IOException, ClassNotFoundException {
        //创建ObjectOutputStream
        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("D:\\Coding\\ObjectputStream.txt"));
        Demo00_person person = new Demo00_person("小明", 12);
        oos.writeObject(person);
        oos.close();

        //
        ObjectInputStream ois = new ObjectInputStream(new FileInputStream("D:\\Coding\\ObjectputStream.txt"));
        Object o = ois.readObject();//声明了ClassNotFound异常
        ois.close();
        System.out.println(o);

        Demo00_person obj1 = (Demo00_person)o;
        System.out.println(obj1.getName()+obj1.getAge());

    }
```
## 3.6.3 对象集合的序列化
transient：瞬态关键字
static：静态关键字
 内存先加载静态关键字，被static修饰的成员变量不能被序列化，对象才能被序列化
被transient修饰的成员变量，不能被序列化，但又没有static静态的含义

序列化集合：
如果想保存多个对象，可以把多个对象保存在集合中， 对集合进行序列化和反序列化

步骤：
1、创建要保存的多个对象
2、创建对象数组ArrayList，将对象添加进数组
3、写序列化方法，创建序列化流，传入字节输出流，指定输出路径，用writeObject方法将对象数组写入序列化流中，关闭流
//4、反序列化：创建反序列化流，传入字节输入流，指定操作文件路径
//5、readObject从反序列化流中读取文件，强转为ArrayList类型，增强for遍历

```java
public static void main(String[] args) throws IOException, ClassNotFoundException {
        Demo00_person person1 = new Demo00_person("人",12);
        Demo00_person person2 = new Demo00_person("鬼",1);
        Demo00_person person3 = new Demo00_person("仙",13);
        ArrayList<Demo00_person> arrayList = new ArrayList<>();
        arrayList.add(person1);
        arrayList.add(person2);
        arrayList.add(person3);
        //序列化
        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("D:\\Coding\\序列化集合.txt"));
        oos.writeObject(arrayList);
        oos.close();
        //反序列化
        ObjectInputStream ois = new ObjectInputStream(new FileInputStream("D:\\Coding\\序列化集合.txt"));
        ArrayList<Demo00_person> arrayList1 = (ArrayList<Demo00_person>)ois.readObject();
        for (Demo00_person obj2 : arrayList1) {
            System.out.println(obj2.getName()+obj2.getAge());
        }
    }
```
## 打印流
java.io.PrintStream：
打印流，继承了OutputStream,为其他输出流添加了功能，使它们能够方便地打印各种数据值表示形式

1、只负责数据输出，不负责数据读取
2、与其他输出流不同，PrintStream永远不会抛出IOException
3、特有方法：print()\println(),可以输出任意类型的值

构造方法
PrintStream(File file)：
          创建具有指定文件且不带自动行刷新的新打印流。
PrintStream(OutputStream out)：
          创建新的打印流。
PrintStream(String fileName)：
          创建具有指定文件名称且不带自动行刷新的新打印流。
          
成员方法
void ==close==()：
          关闭流。
void ==flush==()：
          刷新该流的缓冲。
void ==write==(int b)：
          将指定的字节写入此流。
void ==write==(byte[] b)：
           将b.length字节从指定的字节数组写入此输入流。
void ==write(==byte[] buf, int off, int len)：
          将 len 字节从指定的初始偏移量为 off 的 byte 数组写入此流。
          
注意：
如果使用继承自父类的write方法写数据，查看数据的时候会查看编码表 97->a
用自己特有的print|println方法写数据，写的数据原样输出97->97

改变输出语句的目的地（默认控制台输出），使用System.setOut方法改变输出语句的目的地改为参数中传递的打印流的目的地
静态方法：
static void ==setOut==(PrintStream out)
          重新分配“标准”输出流。
      

```java
java.io.PrintStream：打印流，继承了OutputStream,为其他输出流添加了功能，使它们能够方便地打印各种数据值表示形式

1、只负责数据输出，不负责数据读取
2、与其他输出流不同，PrintStream永远不会抛出IOException
3、特有方法：print()\println(),可以输出任意类型的值
构造方法：
PrintStream(File file)
          创建具有指定文件且不带自动行刷新的新打印流。
PrintStream(OutputStream out)
          创建新的打印流。
PrintStream(String fileName)
          创建具有指定文件名称且不带自动行刷新的新打印流。
成员方法：
void close()
          关闭流。
void flush()
          刷新该流的缓冲。
void write(int b)
          将指定的字节写入此流。
void write(byte[] b)
           将b.length字节从指定的字节数组写入此输入流。
void write(byte[] buf, int off, int len)
          将 len 字节从指定的初始偏移量为 off 的 byte 数组写入此流。
注意：
如果使用继承自父类的write方法写数据，查看数据的时候会查看编码表 97->a
用自己特有的print|println方法写数据，写的数据原样输出97->97

改变输出语句的目的地（默认控制台输出），使用System.setOut方法改变输出语句的目的地改为参数中传递的打印流的目的地
静态方法：
static void setOut(PrintStream out)
          重新分配“标准”输出流。
```

