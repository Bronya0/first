 # java自学第3期——继承、多态、接口、抽象类、final关键字、权限修饰符、内部类
 ***
 
 ## 一.继承：
 关键字extends
 ```
 /*
定义一个父类：人类
定义父类格式：public class 父类名称{
}
定义子类格式：public class 子类名称 extends 父类名称{
}
 */
 ```
 * 代码示范：
 *![在这里插入图片描述](https://img-blog.csdnimg.cn/20190813154827633.png)要点：
 1.子类中在main方法中创建父类对象可调用父类方法；
 2.不加关键字直接访问本方法局部变量(可重名时区分)；
 3.使用this关键字访问本类成员变量(可重名时区分)；
 4.使用super关键字访问父类成员变量(可重名时区分)；
 5.如果存在方法的重名：父类与子类有重名方法：
 ——对象new的谁则优先调用谁的重名方法，没有则向上寻找；
前面介绍了成员变量、成员方法继承的访问特点，接下来是构造方法：
——子类构造方法（父类无参时）中有一个默认赠送的super()方法
——父类有参时，子类里调用super的()里传参，重载时谁对的上调用谁；
 6.如果要更新修改父类方法，本着设计原则尽量不去直接修改正在使用的类，
 则可以进行覆盖重写：格式：通过使用super关键字继承父类需要的方法。
 ```
@Override
方法外部相同(){
	super.父类方法();
	//这里添加新的内容
}
/*
小结super
super关键字作用：
 示例：
    public class demo06Super extends demo01people {

    public void method1(){
        System.out.println(super.num);	//1.在子类成员方法中访问父类的成员变量
    }
    public void method2(){
        super.methodChongMing();	// 2.在子类成员方法中访问父类的成员方法
    }
    public demo06Super(){
        super();	//3.在子类的构造方法中访问父类的构造方法
    }
}
*/
```
7.this关键字的作用小结：
```
this关键字的作用：
1.在本类的成员方法中访问本类的成员变量
2.在本类成员方法中访问本类的另一个成员方法
3.在本类的构造方法中访问本类的另一个构造方法：
(1)this(...)调用必须是构造方法的第一个语句，一个this；
(2)super和this两种构造调用不能同时使用
```
* 代码示范：
```
String name = "python";
public void method1(){
        String name = "python";
        System.out.println(name);//无关键字时直接访问本方法局部变量
        System.out.println(this.name);//访问本类成员变量
        System.out.println(super.num);//访问父类成员变量
        this.method2();//访问本类另一个成员方法
    }
```
8.最后，继承三大特点：
```
1.单继承：一个子类只有一个直接父类
2.多级继承：父类、子类、子类也可作父类再向下延伸，最上级为java.lang.object类
3.一个父类可以有多个子类
```
## 二、多态
1.多态性：父类引用指向子类对象
多态的一个用处：无论右边new的时候换成哪个子类对象，等号左边调用方法都不会发生变化
```
格式：
父类名称 对象名 = new 子类名称();
或者：
接口名 对象名 = new 实现类名称()。
```
2.成员方法：编译看左，运行看右；
  成员变量：编译看左，运行也看左；实例变量不具备多态性.
  代码示范：
```
//前面省略部分Zi类已经继承Fu类
  public static void main(String[] args) {

        Fu one = new Zi();//多态的写法
        one.methodcommon();//重名时，成员方法优先使用子类（new谁先调用谁）。

        one.fu1();//它的编译时类型为Fu，可以调用Fu中的方法
//      one.zi1(); 该调用编译时会报错，因为它的编译时类型为Fu,无法调用zi1方法；
        System.out.println(one.num);//优先看左边Fu类

        Zi two = (Zi) one;//将父类对象还原成子类对象。
        System.out.println(one instanceof Fu);//输出true,one可以做Fu类的实例对象
        System.out.println(one instanceof Zi);//输出true,one可以做Zi类的实例对象
    }
```
3.转型多态写法左父右子是正常的*向上转型*
4.向下转型：为了让对象调用子类方法（向上转型只能调用左边编译类型的父类方法）
*左子类右父类，是一个还原的过程，将父类对象还原成子类对象，且不能向下转成别的子类。*

```

格式：  子类名 对象名 = （子类名） 父类对象名；     （后者也可以是接口）

Fu fu = new Fu();//向上转型创建父类对象
Zi two = (Zi) one;//将父类对象还原成子类对象。
```
5.强制向下转型时，判断前面的对象是否是后面类型的实例，是否可以成功转换，从而保证代码更加健壮。
**格式：  对象   instanceof   类型**
得到一个boolean值结果(true/false)，判断前面的对象能否作为后面类型的实例
```
Zi two = (Zi) one;//将父类对象还原成子类对象。
        System.out.println(one instanceof Fu);//输出true,one可以做Fu类的实例对象
        System.out.println(one instanceof Zi);//输出true,one可以做Zi类的实例对象
```
## 三、接口
1.接口定义了某一批类需要遵守的规范。这就意味着接口里通常是定义一组公用方法。
2.接口是一种引用数据类型（类、接口、数组），注意其中的抽象方法。
3.接口可以继承接口，但不能继承类。
4.Java9 里可以有常量、抽象方法、默认方法、静态方法、私有方法。
5.备注：接口编译后生成的同样是.class的字节码文件
```
//格式
public interface 接口名称(首字母大写){
	//抽象方法
}
```
* 接口里的成员变量只能是常量，必须用public static final修饰,可省略
```
 //final即为不可改变
    public static final int MAX_NUMBER = 20;
```
* 接口里的[普通方法]只能是抽象方法，public abstract可以省略
```
 public abstract void out();
   				 void getDate();
```
* 接口中的[默认方法]需要用default修饰
```
    /*
    当接口新添加方法时，新方法写为默认方法，则可以不去动其实现类，默认方法自动被其继承
    默认方法同样可以被覆写。
     */
    public default void print() {
        foo();
        System.out.println("默认方法调用3");
    }
```
* 接口中定义[静态方法]，需要用static修饰
```
public static void staticTest() {
        System.out.println("静态方法！");
    }
```
* 当俩默认方法中有重复内容时，抽取出来定义私有方法
```
//定义私有默认方法,给默认方法调用，但不应被实现类使用，所以权限为私有
    private void foo() {
        System.out.println("默认方法调用2");
    }
    
//定义私有静态方法，给静态方法调用，但不应被实现类使用，所以权限为私有
    private static void bar() {
        System.out.println("bar私有静态方法");
    }
}
```
* 接口不能创建实例，但能用于声明引用类型变量，且必须引用到实现类的对象；
* 一个实现类可以同时实现多个接口
```
//格式：
.public class 实现类名称 impliments{
 	//必须覆写接口中所有抽象方法；
},	
```
* 在实现类(impliments)中进行接口的实现：
```
//
public class Demo01InterfaceImpl implements Demo01Interface,Demo02Interface {
    //1，2接口都有out抽象方法，但只需覆写一次
    @Override
    public void out() {
        System.out.println("抽象方法覆写！");
    }

    @Override
    public void getDate() {
        System.out.println("抽象方法覆写！");
    }

    @Override
    public void print(){
        System.out.println("冲突的默认方法也需要覆写！");//不冲突则不用覆写
    }
}
```
* 然后在main类里创建实现类的对象，进行调用
```
public class Impliments  {

    public static void main(String[] args) {
  	    //创建实现类的对象
        Demo01InterfaceImpl ImplementationObject1 = new Demo01InterfaceImpl();
        ImplementationObject1.getDate();
        ImplementationObject1.out();
        ImplementationObject1.print();//调用实现类里继承自接口的默认方法。
        
		//接口里的静态方法只能通过接口名称直接调用。
        Demo01Interface.staticTest();

		//通过接口名直接访问常量。
        System.out.println(Demo01Interface.MAX_NUMBER);
    }
}
```
* 接口是多继承，一个接口可以有多个父接口。
* 多个父接口中的抽象方法如果存在重名，正常覆写；但默认方法重名需要带有default关键字覆写。
* 正常不冲突抽象方法不需要再覆写（实现类多个接口时也是如此）。
```
//接口的多继承，同样使用extends关键字
public interface Demo03InterfaceExtends extends Demo01Interface,Demo02Interface {
    @Override
    default void print() {
		//覆写父接口的默认方法不能省略default关键字
    }
}
```
## 四、抽象类
* 抽象方法：加上abstract关键字，去掉大括号，直接分号结束；
* 抽象类：抽象方法所在的类必须是抽象类，再class之前写上abstract即可
* 不能直接new抽象类对象，必须用一个子类来继承抽象父类
（抽象类对象调用的方法存在没有具体内容的方法体，因此没有意义）
* 创建子类对象，不可创建抽象父类对象；
```
public abstract class demo01Abstract {

    public abstract void method1();
    public abstract void method2();
}
```
* 子类必须覆写抽象类中的所有抽象方法（去掉abstrat，补上大括号）。
（假设不全部覆写，对象调用的方法存在抽象方法，没有意义）
```
//使用extends继承抽象父类
public abstract class demo02Zi extends demo01Abstract {

    //只覆写了一个抽象方法，没有将抽象方法全部覆写完，也就是说本子类还存在着继承下来的未覆写的抽象方法
    //所以该类也同样是抽象类
    
    @Override
    public void method1() {
        System.out.println("已经覆写第method1方法！");
    }
}
```
```
//在这个子子类中已经将所有抽象方法全部覆写，所以该类不再是抽象类！
public class demo03Sun extends demo02Zi {
    @Override
    public void method2() {
        System.out.println("已经覆写method2方法！");
    }
}
```
* 最后在main类中的main方法里创建对象进行调用即可。
## 五、final关键字
* final表示它修饰的类、方法、成员变量、局部变量不可改变。
* final修饰的类不可被继承下去。无子类，不可被覆盖重写。
```
public final class 类名(){}
```
* final修饰的方法不可被覆盖重写。
```
public final void method(){}
```
* 类和方法不能同时使用final和abstract，二者矛盾。abstract表示抽象，是待定的；final表示最终的，是确定的。
* final修饰局部变量，若是基本数据类型的数值，则不可改变；若是引用数据类型，则地址值不可改变，但地址指向的对象的内容可以改变。
* final修饰成员变量，也不可变，但必须要进行手动赋值，成员变量加final后不允许再拥有默认值。
```
public final class Demo01Final {

    public static void main(String[] args) {
        int NUM = 1;
        System.out.println(NUM);
        NUM = 2;//可以改变
        System.out.println(NUM);
        
        final int NUM1 = 2;
//      NUM1 = 3;  错误，NUM是确定的值，不可被改变。
    }
}
```
## 六、权限修饰符
* 权限大小：
四种权限修饰符，访问权限从大到小：
             **public > protected > 空 > private**
同一个类：   yes      yes         yes     yes
同一个包：   yes     yes         yes     no
不同包子类： yes     yes         no      no
不同包非子类：yes    no          no      no
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190813223826242.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0ODEwMzI3,size_16,color_FFFFFF,t_70)
## 七、内部类
一个外部类包含的一个嵌套的类，叫内部类。
分类：
**1.成员内部类。
2.局部内部类（包含匿名内部类）。**
* 注意：内部类可以无限制地访问外部类，外部类访问内部类需要内部类对象。
* 各个类对应可以使用的权限修饰符如下：
**外部类：public/(default)
成员内部类:public.protect.(default).private
局部内部类：什么都没有，注意并不是(default)**
* 内部类定义示例：
```
public class main {
    private String name = "成员变量";

    //定义一个成员内部类
    public class innerClass {
        private String innername = "内部成员变量";
    }

		// 定义一个成员内部类内的成员方法
        public void innermethod1() {
            System.out.println("内部类方法");
            System.out.println(name);
        }
        
  }
```
**使用内部类的两种方式：**
1.直接：在main方法中创建内部类对象：格式：
		外.内 = new 外().内();
```
外部类名称.内部类名称 对象名 = new 外部类名称().new 内部类名称();

 public static void main(String[] args) {
        //直接创建内部类的对象。
        InnerclassUsing.innerClass object = new InnerclassUsing().new innerClass();
        object.innermethod1();
    }
```
2.间接：在外部类的方法中，只用内部类，main只是调用外部类的方法，通过外部类方法访问内部类。 
```
public class InnerclassUsing {

    private String name = "成员变量";

    //定义一个成员内部类
    public class innerClass {
    
        private String innername = "内部成员变量";

        // 定义一个成员内部类的成员方法
        public void innermethod1() {
            System.out.println("内部类方法");
            System.out.println(name);
        }
    }

    public void outMethod() {
        System.out.println("外部类成员方法");
        
  //    System.out.println(innername);  错误。
  
        //通过匿名对象访问内部类变量和方法
        System.out.println(new innerClass().innername);
        
        //创建匿名对象并调用innermethod1方法。
        new innerClass().innermethod1();
    }
    
 }

```
**内部类的重名变量访问格式：**
* 本方法： 空
* 本类成员变量：this.
* 外部类成员变量：外部类的名称.this.
```
public class Demo03CommonName {
	//外部类成员变量
    int NAME = 1;

    public class innerClass{
    	//内部类成员变量
        int NAME = 2;
        
        public void method (){
        	//内部类局部变量
            int NAME = 3;
            
            System.out.println(NAME);//3,局部变量，就近原则。
            System.out.println(this.NAME);//2,本类成员变量。
            System.out.println(Demo03CommonName.this.NAME);//1,外部类的成员变量；
        }
    }
}
```
**如果一个类定义在方法内部，那么这个类叫局部内部类。
局部内部类只能被当前所属的方法所使用，外部不可。**
格式：
```
public class Localinnerclass {
    String NAME = "外部成员变量";

    public void method(){
    
    	//定义局部内部类
        class localLinnerClass{
            String ONE = "局部内部类成员变量";
            
            public void nmethod2(){
                System.out.println("局部内部类的成员方法");
                System.out.println(ONE);
            }
        }

    }
}
```
**局部内部类，如果希望访问所在方法的局部变量，该变量应该满足【有效final】的条件**
```
public class Demo04Final {

    public void method1(){
    
        //这里即使不写final的话，不去修改 也算是有效final [从java8开始]
        final String NAME = "bilibili";

        class LocalInnerclass{
        
            public void localInnerMethod(){
            	//局部内部类内的方法访问所在类外部的方法的局部变量
                System.out.println(NAME);
            }
        }

    }
}
```
**当接口是实现类只使用唯一一次时，可以使用匿名内部类**
* 格式
```
接口名称 对象名 = new 接口名称(){
    //这里进行抽象方法的覆写。
};    //别忘了结尾的分号
```
**匿名内部类在创建对象的时候，只能使用唯一一次
匿名对象在使用方法时，只能使用唯一一次
匿名内部类是省略了（实现类/子类名称），而匿名对象则是省略了对象名称，二者不同。**
```
//当实现类只使用一次时，可以使用匿名内部类的写法！
        Anonymous obj6 = new Anonymous() {
            //覆写匿名内部类里所有抽象方法！
            @Override
            public void method1() {
                System.out.println("bilibili?");
            }
            @Override
            public void method2(){
                System.out.println("覆写method!");
            }
        };
        obj6.method1();//打印内容
        obj6.method2();
```
**将类作为成员变量的写法**
```
//定义一个武器类
public class Weapon {
    private String itsname;//武器名称
    private int attacknum;//武器攻击力
		
	//无参构造方法
    public Weapon() {
    }
	//全参方法
    public Weapon(String itsname, int attacknum) {
        this.itsname = itsname;
        this.attacknum = attacknum;
    }
	//get名字
    public String getItsname() {
        return itsname;
    }
	//set名字
    public void setItsname(String itsname) {
        this.itsname = itsname;
    }
	//get攻击力
    public int getAttacknum() {
        return attacknum;
    }
	//set攻击力
    public void setAttacknum(int attacknum) {
        this.attacknum = attacknum;
    }
}

```
```
//定义一个女武神类
public class Valkyrie1 {
    private String name;//女武神之名
    private Weapon weapon;//将武器类变为成员变量 交给女武神类

	//女武神攻击方法
    public void attack(){
        System.out.println(name + "使用的" + weapon.getItsname() + "具有" + weapon.getAttacknum() +"点攻击力");
    }

    
	//无参构造
    public Valkyrie1() {};
	//全参
    public Valkyrie1(String name, Weapon weapon) {
        this.name = name;
        this.weapon = weapon;
    }
	//get女武神名字
    public String getName() {
        return name;
    }
	//set女武神名字
    public void setName(String name) {
        this.name = name;
    }
	//get女武神武器
    public Weapon getWeapon() {
        return weapon;
    }
	//set女武神武器
    public void setWeapon(Weapon weapon) {
        this.weapon = weapon;
    }
}
```
* 定义了女武神类和武器类后，在main方法中进行调用
```
 		Valkyrie1 valkyrie = new Valkyrie1();//创建一个女武神
        valkyrie.setName("bronya"); //set女武神名字叫bronya
        
        Weapon weapon = new Weapon("真理之钥",1000);//创建一把武器，并同时赋予名称和攻击力
//     也可以分开写： weapon.setItsname("真理之钥");//武器名字
//     也可以分开写： weapon.setAttacknum(1000);//武器攻击力

        valkyrie.setWeapon(weapon);//将定义好的武器交给女武神
        
        valkyrie.attack();//最后，女武神使用这把武器进行攻击
```

