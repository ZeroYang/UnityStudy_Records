#C# 泛型

泛型：通过参数化类型来实现在同一份代码上操作多种数据类型。利用“参数化类型”将类型抽象化，从而实现灵活的复用。

	class Program
	{
	    static void Main(string[] args)
	    {
	        int obj = 2;
	        Test<int> test = new Test<int>(obj);
	        Console.WriteLine("int:" + test.obj);
	        string obj2 = "hello world";
	        Test<string> test1 = new Test<string>(obj2);
	        Console.WriteLine("String:" + test1.obj);
	        Console.Read();
	    }
	}
	
	class Test<T>
	{
	    public T obj;
	    public Test(T obj)
	    {
	        this.obj = obj;
	    }
	}

  输出结果是：

    int:2
	String:hello world

**程序分析：**  
1、  Test是一个泛型类。T是要实例化的范型类型。如果T被实例化为int型，那么成员变量obj就是int型的，如果T被实例化为string型，那么obj就是string类型的。  
2、  根据不同的类型，上面的程序显示出不同的值。

##C#泛型特点：
1、如果实例化泛型类型的参数相同，那么JIT编辑器会重复使用该类型，因此C#的动态泛型能力避免了C++静态模板可能导致的代码膨胀的问题。  
2、C#泛型类型携带有丰富的元数据，因此C#的泛型类型可以应用于强大的反射技术。  
3、C#的泛型采用“基类、接口、构造器，值类型/引用类型”的约束方式来实现对类型参数的“显示约束”，提高了类型安全的同时，也丧失了C++模板基于“签名”的隐式约束所具有的高灵活性
 
##C#泛型继承：
C#除了可以单独声明泛型类型（包括类与结构）外，也可以在基类中包含泛型类型的声明。但基类如果是泛型类，它的类型要么以实例化，要么来源于子类（同样是泛型类型）声明的类型参数，看如下类型

	class C<U,V>
	class D:C<string,int>
	class E<U,V>:C<U,V>
	class F<U,V>:C<string,int>
	class G:C<U,V>  //非法

E类型为C类型提供了U、V，也就是上面说的来源于子类  
F类型继承于C<string,int>，个人认为可以看成F继承一个非泛型的类  
G类型为非法的，因为G类型不是泛型，C是泛型，G无法给C提供泛型的实例化

##泛型接口：
泛型接口的类型参数要么已实例化，要么来源于实现类声明的类型参数
 
##泛型委托：
泛型委托支持在委托返回值和参数上应用参数类型，这些参数类型同样可以附带合法的约束

	delegate bool MyDelegate<T>(T value);
	class MyClass
	{
	    static bool F(int i){...}
	    static bool G(string s){...}
	    static void Main()
	    {
	        MyDelegate<string> p2 = G;
	        MyDelegate<int> p1 = new MyDelegate<int>(F);
	    }
	}
 
##泛型方法：
1、C#泛型机制只支持“在方法声明上包含类型参数”——即泛型方法。  
2、C#泛型机制不支持在除方法外的其他成员（包括属性、事件、索引器、构造器、析构器）的声明上包含类型参数，但这些成员本身可以包含在泛型类型中，并使用泛型类型的类型参数。  
3、泛型方法既可以包含在泛型类型中，也可以包含在非泛型类型中。  
 
泛型方法声明：如下  

	public static int FunctionName<T>(T value){...}
 
**泛型方法的重载：**

	public void Function1<T>(T a);
	public void Function1<U>(U a);

这样是不能构成泛型方法的重载。因为编译器无法确定泛型类型T和U是否不同，也就无法确定这两个方法是否不同
 
	public void Function1<T>(int x);
	public void Function1(int x);

这样可以构成重载
 
	public void Function1<T>(T t) where T:A;
	public void Function1<T>(T t) where T:B;

这样不能构成泛型方法的重载。因为编译器无法确定约束条件中的A和B是否不同，也就无法确定这两个方法是否不同
 
**泛型方法重写：**  
在重写的过程中，抽象类中的抽象方法的约束是被默认继承的。如下：

	abstract class Base
	{
	    public abstract T F<T,U>(T t,U u) where U:T;
	    public abstract T G<T>(T t) where T:IComparable;
	}
	 
	class MyClass:Base
	{
	    public override X F<X,Y>(X x,Y y){...}
	    public override T G<T>(T t) where T:IComparable{}
	}

对于MyClass中两个重写的方法来说  
F方法是合法的，约束被默认继承  
G方法是非法的，指定任何约束都是多余的  

##泛型约束：
1、C#泛型要求对“所有泛型类型或泛型方法的类型参数”的任何假定，都要基于“显式的约束”，以维护C#所要求的类型安全。  
2、“显式约束”由where子句表达，可以指定“基类约束”，“接口约束”，“构造器约束”，“值类型/引用类型约束”共四种约束。  
3、“显式约束”并非必须，如果没有指定“显式约束”，范型类型参数将只能访问System.Object类型中的公有方法。例如：在开始的例子中，定义的那个obj成员变量。比如我们在开始的那个例子中加入一个Test1类，在它当中定义两个公共方法Func1、Func2，如下图：

下面就开始分析这些约束：
 

###基类约束：

	    class A
	    {
	        public void Func1()
	        { }
	    }
	 
	    class B
	    {
	        public void Func2()
	        { }
	    }
	 
	    class C<S, T>
	        where S : A
	        where T : B
	    {
	        public C(S s,T t)
	        {
	            //S的变量可以调用Func1方法
	            s.Func1();
	            //T的变量可以调用Func2方法
	            t.Func2();
	        }
	    }

###接口约束：

    interface IA<T>
    {
        T Func1();
    }
 
    interface IB
    {
        void Func2();
    }
 
    interface IC<T>
    {
        T Func3();
    }
 
    class MyClass<T, V>
        where T : IA<T>
        where V : IB, IC<V>
    {
        public MyClass(T t,V v)
        {
            //T的对象可以调用Func1
            t.Func1();
            //V的对象可以调用Func2和Func3
            v.Func2();
            v.Func3();
        }
    }

###构造器约束：

    class A
        {
            public A()
            { }
        }
 
        class B
        {
            public B(int i)
            { }
        }
 
        class C<T> where T : new()
        {
            T t;
            public C()
            {
                t = new T();
            }
        }
 
        class D
        {
            public void Func()
            {
                C<A> c = new C<A>();
                C<B> d = new C<B>();
            }
        }

d对象在编译时报错：The type B must have a public parameterless constructor in order to use it as parameter 'T' in the generic type or method C<T>  
**注意：C#现在只支持无参的构造器约束**  
此时由于我们为B类型写入了一个有参构造器，使得系统不会再为B自动创建一个无参的构造器，但是如果我们将B类型中加一个无参构造器，那么对象d的实例化就不会报错了。B类型定义如下：

        class B
        {
            public B()
            { }
            public B(int i)
            { }
        }

###值类型/引用类型：
public struct A { }
        public class B { }
 
        public class C<T> where T : struct
        {
 
        }
 
        C<A> c1 = new C<A>();
        C<B> c2 = new C<B>();
    c2对象在编译时报错：The type 'B' must be a non-nullable value type in order to use it as parameter 'T' in the generic type or methor 'C<T>'
   
##总结：
1、C#的泛型能力由CLR在运行时支持，它既不同于C++在编译时所支持的静态模板，也不同于Java在编译器层面使用“擦拭法”支持的简单的泛型。  
2、C#的泛型支持包括类、结构、接口、委托四种泛型类型，以及方法成员。  
3、C#的泛型采用“基类，接口，构造器，值类型/引用类型”的约束方式来实现对类型参数的“显式约束”，它不支持C++模板那样的基于签名的隐式约束。