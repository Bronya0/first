## 1.概念：
* 异常 ：指的是程序在执行过程中，出现的非正常的情况，终会导致JVM的非正常停止。
* 在Java等面向对象的编程语言中，异常本身是一个类，
产生异常就是创建异常对象并抛出了一个异常对象。
* Java处理异常的方式是中断处理。
异常指的并不是语法错误,语法错了,编译不通过,不会产生字节码文件,根本不能运行.

## 2.Throwable体系：
* 所有异常或错误的超类：java.lang.Throwable ，其下有两个子类：
java.lang.Error 与 java.lang.Exception
平常所说的异常指 java.lang.Exception 。
* 异常与错误的区别
Error:无法通过处理解决的叫错误，只能事先避免。
Exception:异常产生后程序员可以通过代码的方式纠正，使程序继续运行，是必须要处理的。
* Throwable中的常用方法：
public void ==printStackTrace==() :打印异常的详细信息。
包含了异常的类型,异常的原因,还包括异常出现的位置,
在开发和调试阶段,都得使用printStackTrace。
public String ==getMessage==() :获取发生异常的原因。 提示给用户的时候,就提示错误原因。
public String ==toString==() :获取异常的类型和异常描述信息(不用)。
* 出现异常，把异常的类名复制到API中查找

## 3.异常的分类
编译时期异常:checked异常。在编译时期,就会检查,如果没有处理异常,则编译失败。(如日期格式化异常)
运行时期异常:runtime异常。在运行时期,检查异常.在编译时期,运行异常不会编译器检测(不报错)。(如数学异常)

```java
public class Demo01 {
    public static void main(String[] args) {
        int[] array = {1,2,3};
/*      System.out.println(array[3]);
异常如下：
Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException: Index 3 out of bounds for length 3
	at Exception.Demo01.main(Demo01.java:30)
其中，java.lang.ArrayIndexOutOfBoundsException表示异常的类型
Index 3 out of bounds for length 3表示异常的原因
at Exception.Demo01.main(Demo01.java:30)表示异常的位置
 */
    }
}
```
## 4.异常的处理
在java中，提供了一个throw关键字，它用来抛出一个指定的异常对象。那么，抛出一个异常具体如何操作呢？
1. 创建一个异常对象。封装一些提示信息(信息可以自己编写)。
2. 需要将这个异常对象告知给调用者，通过关键字throw完成。throw异常对象。
throw用在方法内，用来抛出一个异常对象，将这个异常对象传递到调用者处，并结束当前方法的执行。
* 抛出异常：
throw new 异常类名(参数)
* 判断参数合法性，看参数是否为空，统一使用Object类里的静态方法==requireNonNull==();

```java
public class Demo02 {
    public static void main(String[] args) {
        int[] array = {1,2,3};
        /*
        此时index=3越界了，看看运行结果
    Exception in thread "main" java.lang.IndexOutOfBoundsException: 小老弟，你给的角标越数组的界了
	at Exception.Demo02.getElement(Demo02.java:30)
	at Exception.Demo02.main(Demo02.java:23)
         */

        int num = getElement(array,3);
        System.out.println(num);

    }
    public static int getElement(int[] arr,int index){
        if(index < 0||index > arr.length-1){
            //此时角标越界了，使用throw抛出异常
            throw new ArrayIndexOutOfBoundsException("小老弟，你给的角标越数组的界了");
        }
        //该方法返回所给角标对应数组元素
        int Element = arr[index];
        return Element;
    }
    public static void method(Object obj){
        //对传递的obj进行参数的合法性判断
        //传给Object的方法requireNonNull
        Objects.requireNonNull(obj);
    }
}
```
* 声明异常关键字throws
        异常处理的第一种方式：交给别人处理
        声明异常：将问题标识出来，报告给调用者。如果方法内通过throw抛出了编译时异常，而没有捕获处理（稍后讲 解该方式），
        那么必须通过throws进行声明，让调用者去处理。
        关键字throws运用于方法声明之上,用于表示当前方法不处理异常,而是提醒该方法的调用者来处理异常(抛出异常).
        声明异常格式：
        修饰符 返回值类型 方法名(参数) throws 异常类名1,异常类名2…{   } 

* throws用于进行异常类的声明，若该方法可能有多种异常情况产生，那么在throws后面可以写多个异常类，用逗号隔开。

```java
public class Demo03 {
    public static void main(String[] args) throws IOException{//此处注意
        read("a.txt");
    }
    public static void read(String path) throws FileNotFoundException, IOException {
        if (!path.equals("a.txt")){
            throw new FileNotFoundException("文件不存在");
        }
        if (!path.equals("b.txt")){
            throw new IOException("文件不存在");
        }
    }
}
```
* 捕获异常：try...catch
如果异常出现的话,会立刻终止程序,所以我们得处理异常:
 1. 该方法不处理,而是声明抛出,由该方法的调用者来处理(throws)。
 2. 在方法中使用try-catch的语句块来处理异常。
try-catch的方式就是捕获异常。
* 捕获异常：Java中对异常有针对性的语句进行捕获，可以对出现的异常进行指定方式的处理。
捕获异常语法如下：
try(
    可能存在异常的代码
    )catch(定义一个变量，用来接受try中异常的对象){
    处理异常的代码
    }
    ...
    catch(异常类名 变量名){
    }
注意:try和catch都不能单独使用,必须连用。try中可能抛出多个异常对象，那么可以使用多个catch处理

* 如何获取异常的信息：Throwable类中定义了三个查看方法：
 public String ==getMessage==() :返回此throwable的简短描述。
 public String ==toString==() :返回此throwable的详细描述。
 public void ==printStackTrace==() :打印异常对象，默认此方法，打印的异常信息是最全面的。
 包含了异常的类型,异常的原因,还包括异常出现的位置,在开发和调试阶段,都得使用printStackTrace();

* ﬁnally代码块：有一些特定的代码无论异常是否发生，都需要执行。另外，因为异常会引发程序跳转，导致有些语句执行 不到。而ﬁnally就是解决这个问题的，在ﬁnally代码块中存放的代码都是一定会被执行的。
什么时候的代码必须终执行？
当我们在try语句块中打开了一些物理资源(磁盘文件/网络连接/数据库连接等),我们都得在使用完之后,终关闭打开 的资源。
* ﬁnally的语法:
try...catch....ﬁnally:自身需要处理异常,最终还得关闭资源。
注意:
ﬁnally不能单独使用。
只有try或者catch中调用了退出JVM的方法时finally才不会运行，否则finally永远会执行

```java
public class Demo04_try_catch {
    public static void main(String[] args) {
        try {
            readProblem("a.tx");
        } catch (IOException obj) {//try中什么类型异常，括号内就写什么类型
            System.out.println("1:"+obj.getMessage());
            System.out.println("2:"+obj.toString());
            //printStackTrace最全面，默认打印异常对象也是使用该方法
            obj.printStackTrace();
            System.out.println("3:"+obj);
        }finally{//有一些特定的代码无论异常是否发生       论代码是否异常，fanally代码块都会执行
            System.out.println("fianlly代码块执行！");
        }


    }
    public static void readProblem(String path1) throws IOException {
        if (!path1.equals("b.txt")){
            throw new IOException("文件后缀名不对");
        }
    }
}
```
## 5.异常注意事项
(一).捕获多个异常如何处理
1. 多个异常分别处理。
2. 多个异常一次捕获，多次处理。
3. 多个异常一次捕获一次处理。一般使用一次捕获多次处理方式
注意：
a.多个catch内异常类不能相同，且若有子父类关系，子类需要写在上面
b.如果父类抛出了多个异常,子类重写父类方法时,抛出和父类相同的异常或者是父类异常的子类或者不抛出异常。
c.父类方法没有抛出异常，子类重写父类该方法时也不可抛出异常。此时子类产生该异常，只能捕获处理，不能声明抛出.
d.运行时异常(runtime)被抛出可以不处理。即不捕获也不声明抛出。
e.如果ﬁnally有return语句,永远返回ﬁnally中的结果,即避免在finally中写return语句.

```java
public class Demo05_Tips {
    public static void main(String[] args){
       try{
           int[] arr1 = {1,2,3};
           System.out.println(arr1[3]);
            //try里多个异常一次捕获
           List<Integer> list = List.of(1,2,3);
           System.out.println(list.get(3));
       } catch(ArrayIndexOutOfBoundsException a){//通过catch括号内参数类型接受对应异常对象
           System.out.println(a);
       } catch (IndexOutOfBoundsException a){
           System.out.println(a);
       }
    }
    public void show0() throws FileNotFoundException{};
    public void show1() throws IndexOutOfBoundsException{};
    public void show2() throws ArrayIndexOutOfBoundsException{};
    public void show3() throws IOException{};
}
```

```java
class zi extends Demo05_Tips{
    //子类重写父类方法时，可以抛出和父类相同的异常
        public void show0() throws FileNotFoundException{};
        //抛出父类异常类的子类
        public void show1() throws ArrayIndexOutOfBoundsException{};
        //可以不抛出异常
        public void show2() {};
    }
```
定义好异常类后，进行测试：
如果用户注册重复，返回异常信息


## 6.自定义异常类:
java提供的异常类不够，需要进行自定义异常类：

```java
public class ***Exception extends Exception/RuntimeException{
        //添加一个空参数的构造方法
        //添加一个带异常信息的构造方法
}
```

注意：
1.自定义异常类一般以Exception结尾，表示这是异常类
2.自定义异常类必须继承Exception类或者RuntimeException类，
继承Exception类就是编译期异常，选择throws或者try catch
继承RuntimeException就是运行期异常，交给虚拟机处理.

```java
//首先定义一个登陆异常类RegisterException：
//此处继承异常类,本身类名是就是自定义异常类名
public class RegisterException extends Exception{
    //模拟注册操作，如果用户名已存在，则抛出异常并提示：亲，该用户名已经被注册。
    //原有用户名
    //添加一个空参数的构造方法
    public RegisterException() {
        super();
    }
    //添加一个带异常信息的构造方法,让父类来处理
    public RegisterException(String s){
        super(s);
    }
}
```
使用：
```java
public class Demo07_test {

    static String[] username0 = {"1","2","3"};
    public static void main(String[] args) throws RegisterException {//声明自定义异常类
        System.out.println("请输入用户名");
        Scanner sc = new Scanner(System.in);
        String str = sc.next();
        Check(str);
    }
    public static void Check(String str) throws RegisterException {//声明自定义异常类
        for (String name : username0){
            if (name.equals(str)){
                throw new RegisterException("该用户名已经被注册");
            }
        }

        System.out.println("注册成功。");
    }
}
```
