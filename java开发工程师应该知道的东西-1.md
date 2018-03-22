### Java跨平台原理
使用特定编译器编译的程序只能在对应的平台运行，这里也可以说编译器是与平台相关的，编译后的文件也是与平台相关的。我们说的语言跨平台是编译后的文件跨平台，而不是源程序跨平台，如果是源程序，任何一门语言都是跨平台的语言.  
Java先编译后解释,同一个.class文件在不同的虚拟机会得到不同的机器指令（Windows和Linux的机器指令不同），但是最终执行的结果却是相同的
### int 类型究竟多少字节？
编译器可以根据自身硬件来选择合适的大小，但是需要满足约束：short和int型至少为16位，long型至少为32位，并且short型长度不能超过int型，而int型不能超过long型。这即是说各个类型的变量长度是由编译器来决定的，而当前主流的编译器中一般是32位机器和64位机器中int型都是4个字节。  

 |类型 | 大小
---|---
int |   4 
char |  2 
byte |  1 
short | 2 
long |  8
float | 4 
double |    8

数据类型占内存的位数实际上与操作系统的位数和编译器（不同编译器支持的位数可能有所不同）都有关 ，具体某种数据类型占字节数得编译器根据操作系统位数两者之间进行协调好后分配内存大小。具体在使用的时候如想知道具体占内存的位数通过sizeof(int)可以得到准确的答案。 
### java的面向对象的四大特征
1. 抽象  
    抽象就是对现实的一类事物，抽取其特点，并把这些特点整合一起，用java语言表示来表示该类事物  
2. 封装  
    封装就是把属于同一类事物的共性（包括属性与方法）归到一个类中，以方便使用。封装也称为信息隐藏，是指利用抽象数据类型将数据和基于数据的操作封装在一起，使其构成一个不可分割的独立实体。  
3. 继承  
   -    重写(Overriding):           方法重写又称为方法覆盖，子类和父类具有相同的方法名称、相同返回类型、相同参数！如果子类打算调用父类的方法 使用，可以在具有和父类相同的情况下，重写方法的逻辑！如果需要使用父类方法可以使用supper关键字引用父类！
   -    重载(Overloading):   子类重载父类_具有相同的方法和不同的参数或类型，也就是方法名相同但是参数不同或返回类型也可以不相同！
4. 多态
   从一定角度来看，封装和继承几乎都是为多态而准备的，实现多态的技术称为：动态绑定（dynamic binding），是指在执行期间判断所引用对象的实际类型，根据其实际的类型调用其相应的方法。  
     指向子类的父类引用由于向上转型了，它只能访问父类中拥有的方法和属性，而对于子类中存在而父类中不存在的方法，该引用是不能使用的，尽管是重载该方法。若子类重写了父类中的某些方法，在调用该些方法的时候，必定是使用子类中定义的这些方法（动态连接、动态调用）。

 - . 实现形式 Java实现多态有三个必要条件：继承、重写、向上转型。
 -   基于继承的实现方式
 ```java
 class Wine {
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Wine(){
    }

    public String drink(){
        return "喝的是 " + getName();
    }

    /**
     * 重写toString()
     */
    public String toString(){
        return null;
    }
}

class JNC extends Wine{
    public JNC(){
        setName("JNC");
    }

    /**
     * 重写父类方法，实现多态
     */
    public String drink(){
        return "喝的是 " + getName();
    }

    /**
     * 重写toString()
     */
    public String toString(){
        return "Wine : " + getName();
    }
}
class JGJ extends Wine{
    public JGJ(){
        setName("JGJ");
    }

    /**
     * 重写父类方法，实现多态
     */
    public String drink(){
        return "喝的是 " + getName();
    }
    /**
     * 重載方法,父类上转不能调用
     */
    public String drink(int a){
        return "喝的是 " + getName();
    }
    /**
     * 重写toString()
     */
    public String toString(){
        return "Wine : " + getName();
    }
}

public class Test {
    public static void main(String[] args) {
        //定义父类数组
        Wine[] wines = new Wine[2];
        //定义两个子类
        JNC jnc = new JNC();
        JGJ jgj = new JGJ();
        //父类引用子类对象
        wines[0] = jnc;
        wines[1] = jgj;

        for(int i = 0 ; i < 2 ; i++){
            System.out.println(wines[i].toString() + "--" + wines[i].drink());
        }
        System.out.println("-------------------------------");

    }
}

 ```
 
 - 基于接口的实现方式  
 
 ```java
  interface Wine {
    public String getName();
    public void setName(String name);
    public String drink();
    public String toString();
}

class JNC implements Wine{
    public JNC(){
        setName("JNC");
    }

    @Override
    public String getName() {
        return null;
    }

    @Override
    public void setName(String name) {

    }

    @Override
    public String drink() {
        return null;
    }


}
class JGJ implements Wine{
    public JGJ(){
        setName("JGJ");
    }

    @Override
    public String getName() {
        return null;
    }

    @Override
    public void setName(String name) {

    }

    /**
     * 重写父类方法，实现多态
     */
    public String drink(){
        return "喝的是 " + getName();
    }
    /**
     * 重載方法
     */
    public String drink(int a){
        return "喝的是 " + getName();
    }
    /**
     * 重写toString()
     */
    public String toString(){
        return "Wine : " + getName();
    }
}

public class Test {
    public static void main(String[] args) {
        //定义父类数组
        Wine[] wines = new Wine[2];
        //定义两个子类
        JNC jnc = new JNC();
        JGJ jgj = new JGJ();
        Wine wine=new JGJ();
        wine.toString();
        //父类引用子类对象
        wines[0] = jnc;
        wines[1] = jgj;

        for(int i = 0 ; i < 2 ; i++){
            System.out.println(wines[i].toString() + "--" + wines[i].drink());
        }
        System.out.println("-------------------------------");

    }
}

  ```
 ### 头痛又经典的程序
 ```java
     public class A {  
        public String show(D obj) {  
            return ("A and D");  
        }  
      
        public String show(A obj) {  
            return ("A and A");  
        }   
      
    }  
      
    public class B extends A{  
        public String show(B obj){  
            return ("B and B");  
        }  
          
        public String show(A obj){  
            return ("B and A");  
        }   
    }  
      
    public class C extends B{  
      
    }  
      
    public class D extends B{  
      
    }  
      
    public class Test {  
        public static void main(String[] args) {  
            A a1 = new A();  
            A a2 = new B();  
            B b = new B();  
            C c = new C();  
            D d = new D();  
              
            System.out.println("1--" + a1.show(b));  
            System.out.println("2--" + a1.show(c));  
            System.out.println("3--" + a1.show(d));  
            System.out.println("4--" + a2.show(b));  //4--B and A .首先a2是A引用，B实例，调用show（B b）方法，此方法在父类A中没有定义，所以B中方法show(B b)不会调用（多态必须父类中已定义该方法），再按优先级为：this.show(O)、super.show(O)、this.show((super)O)、super.show((super)O)，即先查this对象的父类，没有重头再查参数的父类。查找super.show((super)O)时，B中没有，再向上，找到A中show(A a),因此执行。

            System.out.println("5--" + a2.show(c));  //同上
            System.out.println("6--" + a2.show(d));  //A and D .查找B中没有show(D d)方法，再查A中，有，执行。
            System.out.println("7--" + b.show(b));  
            System.out.println("8--" + b.show(c));  //B and B .
            System.out.println("9--" + b.show(d));        
        }  
    }  
 ```
