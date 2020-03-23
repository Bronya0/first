**集合**：集合是java中提供的一种容器，可以用来存储多个数据。

集合和数组既然都是容器，它们有啥区别呢？

* 数组的长度是固定的。集合的长度是可变的。
* 数组中存储的是同一类型的元素，可以存储基本数据类型值。集合存储的都是对象。而且对象的类型可以不一致。
* 在开发中一般当对象多的时候，使用集合进行存储。

集合按照其存储结构可以分为两大类，分别是
单列集合`java.util.Collection`和双列集合`java.util.Map`，
综述：

|根接 口  | Collection | Map |
|--|--|--|
| 子接口  | List、Set | 未学|
|实现类 |Vector、LinkedList、ArrayList，HashSet、TreeSet|HashMap|
|再子类|LinkedHashSet|LinkedHashMap|






# 一、Collection接口
* **Collection**：单列集合类的根接口，用于存储一系列符合某种规则的元素，它有两个重要的子接口，
* 分别是`java.util.List`和`java.util.Set`。
* 其中，`List`的特点是元素有序、元素可重复。
* `Set`的特点是元素无序，而且不可重复。
* `List`接口的主要实现类有`java.util.ArrayList`和`java.util.LinkedList`，
* `Set`接口的主要实现类有`java.util.HashSet`和`java.util.TreeSet`。
* Collection没有索引，不能直接遍历，先转换成数组

Collection是所有单列集合的父接口，因此在Collection中定义了单列集合(List和Set)通用的一些方法，
* 这些方法可用于操作==所有的==单列集合。方法如下：

* `public boolean add(E e)`：  把给定的对象添加到当前集合中 。可以不用接收
* `public void clear()` :清空集合中所有的元素。
* `public boolean remove(E e)`: 把给定的对象在当前集合中删除。
* `public boolean contains(E e)`: 判断当前集合中是否包含给定的对象。
* `public boolean isEmpty()`: 判断当前集合是否为空。
* `public int size()`: 返回集合中元素的个数。
* `public Object[] toArray()`: 把集合中的元素，存储到数组中。
```java
public static void main(String[] args) {
        //多态,接口指向实现类
        //此处ArrayList改成HashSet等其他集合也可以，方法一样
        Collection<String> collection1 = new ArrayList<>();
//        Collection<String> collection1 = new HashSet<>();
        //打印空，不是地址，说明重写了toString方法
        System.out.println(collection1);
        collection1.add("kiana");
        //打印[kiana]
        System.out.println(collection1);
        collection1.add("bronya");
        collection1.add("Fuka");
        //[kiana, bronya, Fuka]
        System.out.println(collection1);
        collection1.remove("bronya");
        //[kiana, Fuka]
        System.out.println(collection1);
        //判断有无元素Mei
        boolean criticalResult = collection1.contains("Mei");
        //false
        System.out.println(criticalResult);
        //判断集合是否为空
        boolean Result2 = collection1.isEmpty();
        //false
        System.out.println(Result2);
        int sizeResult = collection1.size();
        //2
        System.out.println(sizeResult);
        //把集合变成数组
        Object[] array = collection1.toArray();
        //遍历数组，kiana Fuka
        for (int i = 0; i <array.length ; i++) {
            System.out.println(array[i]);
        }
        //删除集合元素，集合本身还在
        collection1.clear();
        //输出[]
        System.out.println(collection1);
        }
```
# 二、迭代器
Iterator迭代器，是Collection集合通用的取出元素的方式，先判断集合有没有元素，有就取出一个元素，再继续判断有没有元素……直到把集合中所有元素全部取出。这种取出方式称作迭代。

boolean ==hasNext()==：(判断)如果仍有元素可以迭代，则返回 true。
 E ==next()==：(取出)返回迭代的下一个元素。
 void ==remove()==：从迭代器指向的 collection 中移除迭代器返回的最后一个元素（可选操作）。

Iterator是接口 不能直接使用，需要接口的实现类。注意，Iterator接口也是有泛型的，与集合相同。Collection接口里有个方法：
iterator() ，返回的就是Iterator的实现类对象。
Iterator<E> ==iterator()==： 返回在此 collection 的元素上进行迭代的迭代器。

```java
public static void main(String[] args) {
        Collection<String> collection1 = new ArrayList<>();
        collection1.add("板鸭");
        collection1.add("芽衣");
        //[板鸭, 芽衣]
        System.out.println(collection1);
        //第一步，获取这个集合的迭代器。接口指向实现类对象
        Iterator<String> iterator = collection1.iterator();
        //第二步，判断有没有元素
        System.out.println(iterator.hasNext());
        //第三步，取出元素
        String next = iterator.next();
        //板鸭
        System.out.println(next);
        //true
        System.out.println(iterator.hasNext());
        //芽衣
        System.out.println(iterator.next());
        //已经没有元素了，再取会抛出NoSuchElementException没有元素异常
//        System.out.println(iterator.next());

        //优化
        Collection<String> collection2 = new ArrayList<>();
        collection2.add("板鸭");
        collection2.add("芽衣");
        Iterator<String> iterator1 = collection2.iterator();
        //板鸭芽衣
        while(iterator1.hasNext()){
            System.out.print(iterator1.next());
            
        }
    }
```
# 三、泛型
* 泛型：一种未知的数据类型，定义集合时若暂未确定集合内元素类型，可使用泛型。
* 泛型也可以看作是一个变量，用来接收数据类型。
* E e:Element 元素
* T t:Type 类型
* 创建集合对象的时候，就会确认泛型的数据类型
*不使用泛型时，默认Object类型，可以存储任意类型的数据。坏处：不安全，会引发异常。
* 好处：避免了类型转换的麻烦；把运行期异常，提升到了编译期。

```java
public static void main(String[] args) {
        method1();
        method2();
    }
    /*
    创建集合对象，不使用泛型时，默认Object类型，可以存储任意类型的数据。
    坏处：不安全，会引发异常。
     */
    private static void method1(){
        ArrayList array1 = new ArrayList();//创建集合
        array1.add("女武神");//存储字符串
        array1.add(12);//存储整数
        //使用迭代器遍历
        Iterator iterator1 = array1.iterator();
        while(iterator1.hasNext()){
            Object obj1 = iterator1.next();
            System.out.println(obj1);//输出：女武神 12

            //假如要使用String类里的lenghth()输出字符串长度，
            // 需要把Object类型的obj1向下转型为String类型
            String string1 = (String)obj1;
            System.out.println(string1.length());//3
            //上面产生了ClassCastException类型转换异常，这就是不使用泛型的缺点
        }
    }
    private static void method2(){
        ArrayList<String> list = new ArrayList<>();
        list.add("Valkyrie");

        Iterator<String> iterator2 = list.iterator();
        while(iterator2.hasNext()){
            System.out.println(iterator2.next());
        }
    }
```
* 定义一个含有泛型的数据类型，模拟ArrayList集合。
* 泛型可以接收任意的数据类型，可以使用Integer，String、Object……
* 创建对象的时候确定泛型的数据类型。
* 不写泛型默认是Object类型

```java
//添加泛型E：
public class Demo05_class<E> {
    private E name;

    public E getName() {
        return name;
    }

    public void setName(E name) {
        this.name = name;
    }
}
```
* 测试类
对象类写成泛型<E>，不把它写死，要什么类型就在创建对象的时候定义
```java
 */
public class Demo05_class1 {

    public static void main(String[] args) {
        //不写泛型默认是Object类型
        Demo05_class obj = new Demo05_class();
        obj.setName("字符串");

        //创建Demo05_class对象，使用Integer泛型
        Demo05_class<Integer> obj2 = new Demo05_class<>();
        obj2.setName(123);
        System.out.println(obj2.getName());

        //使用含有泛型的方法
        Demo05_method obj3 = new Demo05_method();
        //传递什么类型，泛型就是什么类型
        obj3.method1("666");//String
        obj3.method1(666);//Integer
        //静态方法通过类名点方法名直接使用。
        Demo05_method.method2("666");

        //创建接口实现类对象方法1
        Demo05_InterfaceImpl obj4 = new Demo05_InterfaceImpl();
        obj4.method4("888");
        //创建接口实现类对象方法2,在创建对象时确认泛型
        Demo05_InterfaceImpl2<String> obj5 = new Demo05_InterfaceImpl2();
        obj5.method4("字符串");

    }
```
* 定义接口的实现类,指定接口的泛型
```java
public interface Iterator<E>{
    E next();
}
public class Demo05_InterfaceImpl implements Demo05_Interface<String> {
    @Override
    public void method4(String i){
        System.out.println(i);
    }
}
```
* 接口和实现类后都写上泛型， 重写时泛型作为参数进入方法

```java
public class Demo05_InterfaceImpl2<I> implements Demo05_Interface<I> {
    @Override
    public void method4(I i){
        System.out.println("重写2");
    }

}
```
* 定义含有泛型的方法：
泛型定义在方法的修饰符和返回值之间，参数列表里可以通过泛型使用
在调用方法的时候确认泛型的使用类型

```java
public class Demo05_method {

    public <E> void method1(E one) {
        System.out.println(one);
    }
    //定义一个含有泛型的静态方法
    public static <S> void method2(S s){
        System.out.println("含有泛型的静态方法");
    }
}
```
*    泛型通配符<?>在参数传递的时候使用

```java
public static void main(String[] args) {
        ArrayList<String> list1 = new ArrayList<>();
        list1.add("字符串");
        ArrayList<Integer> list2 = new ArrayList<>();
        list2.add(666);
        //该方法能同时调用不同类型的集合
        method1(list1);//字符串
        method1(list2);//666
    }
    //泛型通配符在参数传递的时候使用
    public static void method1(ArrayList<?> list){
        Iterator<?> iterator1 = list.iterator();
        while(iterator1.hasNext()){
            System.out.println(iterator1.next());
        }
    }
```
## 斗地主案例实现

```java
public static void main(String[] args) {
        //定义存储54张牌的ArrayList集合
        ArrayList<String> array1 = new ArrayList<>();
        //定义两个数组，一个存储花色，一个存储序号
        String[] colors = {"黑桃","红桃","方块","梅花"};
        String[] nubers = {"3","4","5","6","7","8","9","10","J","Q","K","A","2"};
        //集合里存储大王小王
        array1.add("大王");
        array1.add("小王");
        //循环嵌套两个数组，组装52张牌
        for (String numbers:nubers){
            for (String colours : colors) {
                //System.out.println(colours+numbers);
                //把组装好的牌存储到集合中
                array1.add(colours+numbers);
            }
        }
//        System.out.println(array1);
        //洗牌，通过Collections里的shuffle方法
        Collections.shuffle(array1);
//        System.out.println(array1);
        //定义四个集合，存储玩家的牌
        ArrayList<String> play01 = new ArrayList<>();
        ArrayList<String> play02 = new ArrayList<>();
        ArrayList<String> play03 = new ArrayList<>();
        ArrayList<String> dipai = new ArrayList<>();
        //发牌
        System.out.println(array1.size());
        for (int i = 0; i < array1.size() ; i++) {
            //获取每一张牌
            String str1 = array1.get(i);
            if ( i>=51){
                //底牌
                dipai.add(str1);
            }else if (i%3==0){
                //玩家1发牌
                play01.add(str1);
            }else if (i%3==1){
                //玩家2发牌
                play02.add(str1);
            }else if (i%3==2){
                //玩家3发牌
                play03.add(str1);
            }
        }
        //看牌
        System.out.println("1号：" + play01);
        System.out.println("2号：" + play02);
        System.out.println("3号：" + play03);
        System.out.println("底牌："+ dipai);
    }
```
# 四、List接口
Collection下一个List一个Set。
List集合下的实现类中，ArrayList查找快，增删慢；LinkedList是List借口的链表实现，查询慢，增删快。

import java.util.List;
* List:有序集合（存取顺序相同），有索引，允许存储重复元素。底层是数组。此实现是多线程，非同步。
* 方法：
 boolean ==add(E e)==
          向列表的尾部添加指定的元素（可选操作）。
 void ==add==(int index, E element)
          在列表的指定位置插入指定元素（可选操作）。
 E ==get==(int index)
          返回列表中指定位置的元素。
 ListIterator<E> ==listIterator()==
          返回此列表元素的列表迭代器（按适当顺序）。
 ListIterator<E> ==istIterator(int index)==
          返回列表中元素的列表迭代器（按适当顺序），从列表的指定位置开始。
 E ==remove==(int index)
          移除列表中指定位置的元素（可选操作）。
 boolean ==remove==(Object o)
          从此列表中移除第一次出现的指定元素（如果存在）（可选操作）。
```java
public static void main(String[] args) {
        //多态
        List<String> list1 = new ArrayList<>();
        list1.add("光");
        list1.add("翼");
        list1.add("展");
        list1.add("开");
        list1.add(0,"kiana");
        System.out.println(list1.get(0));//kiana
        ListIterator<String> listIterator1 = list1.listIterator();
        ListIterator<String> listIterator2 = list1.listIterator(1);
        while(listIterator1.hasNext()){
            System.out.println(listIterator1.next());//kiana 光 翼 展 开
        }
        while(listIterator2.hasNext()){
            System.out.println(listIterator2.next());//光翼展开
        }
        list1.remove(0);
        System.out.println(list1);//[光, 翼, 展, 开]
        list1.remove("光");
        System.out.println(list1);//[翼, 展, 开]

    }
```
## 实现类LinkedList
* LinkedList集合查询慢，增删快，含有大量用于修改首尾的方法。
如果使用LinkedList特有的方法，则不能使用多态
* 方法：
void ==addFirst==(E e)：
          将指定元素插入此列表的开头。
 void ==addLast==(E e)：
          将指定元素添加到此列表的结尾。
 void ==clear()==：
          从此列表中移除所有元素。
 boolean ==contains==(Object o)：
          如果此列表包含指定元素，则返回 true。
 E ==get==(int index)：
          返回此列表中指定位置处的元素。
 E ==getFirst==()：
          返回此列表的第一个元素。
 E ==getLast()==：
          返回此列表的最后一个元素。
E ==removeFirst()==：
          移除并返回此列表的第一个元素。
E ==removeLast()==：
          移除并返回此列表的最后一个元素。


```java
public static void main(String[] args) {
        LinkedList<String> list1 = new LinkedList<>();
        list1.add("kiana");
        list1.addFirst("love");
        list1.addLast("is me");
        System.out.println(list1);//[love, kiana, is me]
        System.out.println(list1.get(1));//kiana
        if (list1.contains("love")){
            System.out.println("含有lova");
        }
        list1.removeFirst();
        System.out.println(list1);//[kiana, is me]
    }
```
* Vector集合，实现可增长的对象数组，底层是数组，已经被ArrayList取代，单线程
# 五、Set接口
Collection的另一个孩子，

1.不能存储重复元素。
2.没有索引，不能使用普通的for循环遍历。
3.底层是哈希表结构，查询速度很快，存取元素顺序可能不一致。
4.HashSet集合类实现Set接口；
* 方法：
 boolean ==add==(E e)：
          如果 set 中尚未存在指定的元素，则添加此元素（可选操作）。
 boolean ==addAll==(Collection<? extends E> c)：
          如果 set 中没有指定 collection 中的所有元素，则将其添加到此 set 中（可选操作）。
 void ==clear==()：
          移除此 set 中的所有元素（可选操作）。
 boolean ==contains==(Object o)：
          如果 set 包含指定的元素，则返回 true。
 boolean ==containsAll==(Collection<?> c)：
          如果此 set 包含指定 collection 的所有元素，则返回 true。
 boolean ==equals==(Object o)：
          比较指定对象与此 set 的相等性。
 int ==hashCode==()：
          返回 set 的哈希码值。
 boolean ==isEmpty==()：
          如果 set 不包含元素，则返回 true。
 Iterator<E> ==iterator()==：
          返回在此 set 中的元素上进行迭代的迭代器。
 boolean ==remove(Object o)==：
          如果 set 中存在指定的元素，则将其移除（可选操作）。
 boolean ==removeAll==(Collection<?> c)：
          移除 set 中那些包含在指定 collection 中的元素（可选操作）。
 boolean ==retainAll==(Collection<?> c)：
          仅保留 set 中那些包含在指定 collection 中的元素（可选操作）。
 int ==size==()：
          返回 set 中的元素数（其容量）。
 Object[] ==toArray==()：
          返回一个包含 set 中所有元素的数组。
<T> T[]  ==toArray==(T[] a)：
          返回一个包含此 set 中所有元素的数组；返回数组的运行时类型是指定数组的类型。
```java
public static void main(String[] args) {
        Set<String> setList1 = new HashSet<>();
        setList1.add("Mei");
        setList1.add("逐火之蛾");
        setList1.add("Mei");//重复元素存储失败
        System.out.println(setList1.contains("Mei"));
        Iterator<String> iterator1 = setList1.iterator();
        while (iterator1.hasNext()){
            System.out.println(iterator1.next());
        }
        System.out.println(setList1.size());
```
## HashSet实现类
* 哈希值是一个十进制的整数，由系统随机给出。
Object类的hashCode方法返回对象的哈希码值：
int ==hashCode()==
jdk1.8之后，哈希表=数组+红黑树(加快查询速度)
存储数据到集合中，首先计算元素的哈希值
* 没有重写时，HashSet比较的是两个对象的地址值，
存储自定义类型的元素时必须重写hashCode和equals方法同时比较元素和哈希值更稳妥。
按Alt+Insert重写方法

```java
 @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Demo11_Hash that = (Demo11_Hash) o;
        return Objects.equals(name, that.name) &&
                Objects.equals(age, that.age);
    }
@Override
    public int hashCode() {
        return super.hashCode();
    }
```
## LinkedHashSet（继承HashSet）
* 具有可预知迭代顺序的Set接口的哈希表和链表实现
LinkedHashSet集合extends了HashSet集合,
LinkedHashSet集合底层是哈希表(链表+数组/红黑树)+链表，多的链表用来记录元素存储顺序，保证元素有序

```java
public static void main(String[] args) {
        HashSet<String> set1 = new HashSet<>();
        set1.add("a");
        set1.add("a");
        set1.add("c");
        set1.add("b");
        System.out.println(set1);//[a, b, c],无重复，无顺序

        LinkedHashSet<String> linkedset2 = new LinkedHashSet<>();
        linkedset2.add("a");
        linkedset2.add("a");
        linkedset2.add("c");
        linkedset2.add("b");
        System.out.println(linkedset2);//[a, c, b],有顺序，无重复
    }
```
# 六、可变参数
* 可变参数：当方法的参数列表数据类型已经确定，但参数的数据类型不确定，就可以使用可变参数
使用格式：定义方法时使用
修饰符 返回值类型 方法名(数据类型...变量名){}
原理：可变参数底层是数组，根据传递参数个数不同，会创建不同长度的数组，来存储这些参数。
传递的参数个数可以是0个（不传递），1,2，3...很多个。
可变参数的终极写法：
(Object...obj)
* 注意：
一个方法的参数列表，只能有一个可变参数。
如果方法的参数有多个，那么可变参数必须写在参数列表的末尾。

```java
public static void main(String[] args) {
        Sum1();
        System.out.println(Sum2(222,111,223,3));//559
        Sum3(33,"w",3.14);
    }
    //定义一个方法，计算两个int整数的和
    public static void Sum1(){
        System.out.println("请输入俩数字");
        Scanner num1 = new Scanner(System.in);
        Scanner num2 = new Scanner(System.in);
        int sum = num1.nextInt()+num2.nextInt();
        System.out.println(sum);
    }
    //计算未定数目的参数的和
    public static int Sum2(int...num3){
        int Sum= 0;
        for (int i : num3) {
            Sum += i;
        }
        return Sum;
    }
    public static void Sum3(Object...obj){
        for (Object o : obj) {
            System.out.println(o);
        }
    }

```
# 七、Collections工具类
Collections工具类：java.utils.Collections
* 方法：
public static <T> ==boolean addAll==(Collection<T> c,T...elements)
        往集合中添加多个元素
public static void ==shuffle==(List<T> list)
        打乱集合顺序
public static <T> void ==sort==(List<T> list)
        将集合中的元素按照默认顺序排序
public static <T> void ==sort==(List<T> list,Comparator <? super T>)
         将集合中的元素按照指定规则排序
* 注：被排序的集合里的元素，必须实现Comparable接口，重写接口中的compareTo定义排序的规则

```java
public static void main(String[] args) {
        ArrayList<String> list1 = new ArrayList<>();
        //addAll
        Collections.addAll(list1,"k","i","a","n","a");
        System.out.println("addAll："+list1);
        //shuffle
        Collections.shuffle(list1);
        System.out.println("打乱后："+list1);
        //sort默认(数字默认大小升序，字符串默认自然升序abcdefg...)
        Collections.sort(list1);
        System.out.println("默认排序后："+list1);//[a, a, i, k, n]
        //sort指定，重写排序规则
        ArrayList<Integer> list2 = new ArrayList<>();
        list2.add(1);
        list2.add(3);
        list2.add(2);
        Collections.sort(list2, new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                //o1-o2代表升序，o2-o1代表降序
                return o1-o2;
            }
        });
        System.out.println(list2);//1,2,3
        //自定义类型Demo00_Person的排列
        ArrayList<Demo00_Person> list3 = new ArrayList<>();
        list3.add(new Demo00_Person("琪亚娜",17));
        list3.add(new Demo00_Person("芽衣",18));
        list3.add(new Demo00_Person("板鸭",15));
        list3.add(new Demo00_Person("舰长",20));
        System.out.println("自定义类型排列前："+list3);
        Collections.sort(list3, new Comparator<Demo00_Person>() {
            @Override
            public int compare(Demo00_Person o1, Demo00_Person o2) {
                //先按年龄排列
                int result = o1.getAge() - o2.getAge();
                //如果年龄相同，按照名字排列
                if(result == 0){
                    result = o1.getName().charAt(0)-o2.getName().charAt(0);
                }
                return result;
            }
        });
        System.out.println("自定义类型排列后："+list3);
```
# 八、集合数据结构
和集合相关的数据结构：栈、队列，数组，链表，红黑树。
* 栈：先进后出
元素存储到集合中时，先存储的元素在栈的底部，后进入的元素在栈的顶部，
取出元素时，先取出栈顶部的元素，栈底部的元素后取出。栈的出口和入口相同。
例如：入栈顺序123，出栈的顺序即为321。
***
* 队列：先进先出
入口和出口不同，先入者接近出口，后入者离出口较远。
***
* 数组：查询快，增删慢。
查询快：数组的地址是连续的，通过首地址找到数组，再通过索引找到想要的数组元素。
增删慢：数组的长度是固定的，增删数组需要创建一个新的数组，并将源数组的数据复制到新数组中。
源数组在内存中被销毁（垃圾回收）。
***
* 链表(linked list)：由一系列节点node（链表中的元素）组成，节点在运行时动态生成。
每个节点包含一个存储数据元素的数据域，和两个分别存储自己和下一个节点地址的指针域。
链表结构通常有单向链表和双向链表。多个节点间通过地址相连。
* 单向链表：链表中只有一条链子，不能保证元素的顺序。存储、取出的数据可能不一致。
* 双向链表：有两条链子，其中一条专门记录元素的顺序，所是一个有序的集合.
查询慢：想查找某个元素，需要通过连接的节点，一次向后查找指定元素。地址不连续，每次查询需要从头开始。
增删快：增删元素对链表整体结构没有影响，所以快。
***
* 红黑树：
二叉树：分支不能超过两格的树
排序树/查找树：在二叉树的基础上，元素是有大小的，左子树小，右子树大
红黑树：节点红色或黑色，根节点黑色，叶子节点黑色，每个红色的节点的子节点都是黑色，任何节点到其
每一个叶子节点的所有路径上黑色节点的数量是相同的。
# 九、Map集合接口
java里集合除了Collections(List/Set)外就是Map集合，用来存放映射关系的对象。
* Collection中的集合，元素是孤立存在的（理解为单身），向集合中存储元素采用一个个元素的方式存储。
* Map 中的集合，元素是成对存在的(理解为夫妻)。每个元素由键与值两部分组成，通过键可以找对所对应的值。
* Collection 中的集合称为单列集合， Map 中的集合称为双列集合。
需要注意的是， Map 中的集合不能包含重复的键，值可以重复；每个键只能对应一个值。
* java.util.Map<k,v>  ：key键，value值，通过key可以找到对应的value，
value可以重复，一个key对应一个value，key不可以重复。
key和map数据类型可以相同可以不同，
靠key维护他们之间的关系。
## HashMap：
Map的两个常用子类：HashMap和LinkedHashMap

* HashMap：存储数据采用的哈希表结构，元素的存取顺序不能保证一致。
由于要保证键的唯一、不重复，需 要重写键的hashCode()方法、equals()方法。
* LinkedHashMap：HashMap下有个子类LinkedHashMap，存储数据采用的哈希表结构+链表结构。
通过链表结构可以保证元素的存取顺序一致；通过哈希表结构可以保证的键的唯一、不重复，
需要重写键的 hashCode()方法、equals()方法。

tips：Map接口中的集合都有两个泛型变量,在使用时，要为两个泛型变量赋予数据类型。
两个泛型变量的数 据类型可以相同，也可以不同。
tips：Map集合不能直接使用迭代器或者foreach进行遍历。但是转成Set之后就可以使用了。

Map接口中定义了很多方法，常用的如下：
public V ==put==(K key, V value) : 把指定的键与指定的值添加到Map集合中。
public V ==remove==(Object key) : 把指定的键 所对应的键值对元素 在Map集合中删除，返回被删除元素的值。
public V ==get==(Object key) 根据指定的键，在Map集合中获取对应的值。
public Set<K> ==keySet==() : 获取Map集合中所有的键，存储到Set集合中。
public Set<Map.Entry<K,V>> ==entrySet==() : 获取到Map集合中所有的键值对对象的集合(Set集合)。

```java
public static void main(String[] args) {
        HashMap<Integer,String> list1 = new HashMap<>();
        list1.put(0,"kiana");
        list1.put(1,"love");
        list1.put(2,"me");
        //kiana
        System.out.println(list1.get(0));
        boolean b = list1.containsKey(5);
        //false
        System.out.println(b);
        //遍历Map集合，先取出key到set集合，再用Iterator或者增强for
        Set<Integer> setlist1 = list1.keySet();
        Iterator<Integer> iterator = setlist1.iterator();
        while (iterator.hasNext()){
            Integer key = iterator.next();
            String value = list1.get(key);
            System.out.println(key+" 对应 "+value);

        }
        for (Integer key1 : setlist1){
            //此处setList1可以直接写List1.keySet简化一步
            System.out.println(list1.get(key1));
        }
```
## Entry：
Map 中存放的是两种对象，一种称为key(键)，一种称为value(值)，
它们在在 Map 中是一一对应关系，这一对对象又称做 Map 中的一个 Entry(项) 
Entry 将键值对的对应关系封装成了对象。
即键值对对象，这样我们在
遍历 Map 集合时，就可以从每一个键值对（ Entry ）对象中获取对应的键与对应的值。
既然Entry表示了一对键和值，那么也同样提供了
* 获取对应键和对应值的方法：
public K ==getKey==() ：获取Entry对象中的键。
public V ==getValue()== ：获取Entry对象中的值。
在Map集合中也提供了
* 获取所有Entry对象的方法：
public Set<Map.Entry<K,V>> ==entrySet==() : 获取到Map集合中所有的键值对对象的集合(Set集合)。

```java
public static void main(String[] args) {
        HashMap<Integer,String> list2 = new HashMap<>();
        //添加三个entry对象
        list2.put(1,"1");
        list2.put(2,"2");
        list2.put(3,"3");
        //获取entry对象到set集合中
        Set<Map.Entry<Integer, String>> obj1 = list2.entrySet();
        //增强for遍历set集合，集合里通过getKey、getValue方法活动对象的键值
        for (Map.Entry<Integer, String> integerStringEntry : obj1) {
            Integer key = integerStringEntry.getKey();
            String value = integerStringEntry.getValue();
            System.out.println(key+value);//11,22,33
        }
```
* Map集合存储自定义类型的方法：
当给HashMap中存放自定义对象时，
如果自定义对象作为key存在，
这时要保证对象唯一，必须复写对象的 hashCode和equals方法.
如果要保证map中存放的key和取出的顺序一致，可以使用 java.util.LinkedHashMap 集合来存放。

```java
public static void main(String[] args) {
        //key写为自定义类型
        HashMap<Dem00_Person,String> map1 = new HashMap<>();
        //创建自定义对象为参数：
        map1.put(new Dem00_Person("Kiana",16),"大老婆");
        map1.put(new Dem00_Person("Mei",17),"二老婆");
        map1.put(new Dem00_Person("Bronya",14),"三老婆");
        map1.put(new Dem00_Person("Teresa",12),"小老婆");
        //取出元素，键找值方式,注意Set类型写法
        Set<Map.Entry<Dem00_Person,String>> obj2 = map1.entrySet();
```
##  集合工厂方法 ==of==():
* JDK9对集合添加元素的优化：
通常，我们在代码中创建一个集合（例如，List 或 Set ），并直接用一些元素填充它。
实例化集合，几个 add方法 调用，代码显得重复。
Java 9，添加了几种集合工厂方法,更方便创建少量元素的集合、map实例。
新的List、Set、Map的静态工厂方法of可以更方便地创建集合的不可变实例。
* 使用前提：
1.集合中元素的数确定且不再改变
            2.只适用于List、Set、Map接口，不适用于它们的实现类；
            3.Set和Map存的元素不能有重复,List可以
```java
public static void main(String[] args) {
        List<String> list1 = List.of("k","i","a","n","a","a");
//        list1.add("aa");
//        UnsupportedOperationException不支持操作异常，of返回的集合不可再改变
//        Set<String> set1 = Set.of("a", "a");
//        Set集合存储重复元素，抛出IllegalArgumentException非法参数异常
    }
```
# 十、Debug
IDEA中：
左边设置断点，右键选择Debug运行程序，程序停止在了断点行不再运行
Debug后，下方按钮从左到右：
* Console：切换到控制台
* Step Over(F8):代码向下执行一行
* Step Into(F7):进入要调用的方法
* Step Out(shift + F8):跳出方法
左边：
* Resume Program(F9)：调到下一个断点
* Stop(Ctrl+F2)：停止调试的程序，退出Debug模式 

