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