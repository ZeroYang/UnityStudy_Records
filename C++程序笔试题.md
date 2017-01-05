# C/C++ 程序员笔试 #



## 1.	局部变量能否和全局变量重名？ ##


## 2.	如何引用一个已经定义过的全局变量？ ##


## 3.	全局变量可不可以定义在可被多个.C文件包含的头文件中？ ##


## 4.	static全局变量和普通的全局变量有什么区别？ ##



## 5.	分析一下程序的运行结果: ##

```

    #include<iostream.h>
    class CBase
    {
		public:
		CBase(){cout<< “constructing CBase class”<<endl;}
		~CBase(){cout<< “destructing CBase class”<<endl;}
    };

    class CSub : public CBase
    {
	    public:
	    CSub(){cout<< “constructing CSub class”<<endl;}
	    ~CSub(){cout<<“destructing CSub class”<<endl;}
    };

    void main()
    {
    	CSub obj;
    }
```

## 6.	计算结果 ##

```

    int a = 1, b = 2;
    int c = b++;  
    int d = (++c) + (c++) + (a++);  
    	a = 
    	b = 
    	c = 
    	d = 
```
	
## 7.	在32bit系统下，已知代码： ##

```

    char* a = 1,  b =2; 
    char c[] = “abcdefg”; 
    char d[10] = “abcdefg”;
    string e(“abc\0defg”);
    typedef char* A;
    A  f, g ;
```

请下计算以下表达式的值:	

    sizeof(a) = 
    sizeof(b) = 
    sizeof(c) = 
    strlen(c) = 
    sizeof(d) = 
    e.length() = 
    sizeof(f) = 
    sizeof(g) = 

## 8.	以下三条输出语句分别输出什么？ ##


```

    char str1[] = "abc";
    char str2[] = "abc";
    const char str3[] = "abc"; 
    const char str4[] = "abc"; 
    const char* str5 = "abc";
    const char* str6 = "abc";
    std::cout << std::boolalpha << ( str1==str2 ) << std::endl; // 输出什么？
    std::cout << std::boolalpha << ( str3==str4 ) << std::endl; // 输出什么？
    std::cout << std::boolalpha << ( str5==str6 ) << std::endl; // 输出什么？
```


	
## 9.	以下代码中的两个sizeof用法有问题吗？ ##



```

    void UpperCase( char str[] ) 　// 将 str 中的小写字母转换成大写字母
    {
	    for( size_t i=0; i<sizeof(str)/sizeof(str[0]); ++i )
	    	if( 'a'<=str[i] && str[i]<='z' )
	     		str[i] -= ('a'-'A' );
    }
    
    char str[] = "aBcDe";
    cout << "str字符长度为: " << sizeof(str)/sizeof(str[0]) << endl;
    UpperCase( str );
    cout << str << endl;
```

	
## 10.	以下代码能够编译通过吗，为什么？ ##


```

    unsigned int const size1 = 2;
    char str1[ size1 ];
    unsigned int temp = 0;
    std::cin >> temp;
    unsigned int const size2 = temp;
    char str2[ size2 ];
```

	
## 11.	下面代码有什么问题，如何修改？ ##


```

    #include <iostream>
    using namespace std;
    class Test
    {
	    public:
	    explicit Test (int InitValue = 0)
	    {
	    	value = new int(InitValue);
	    }

	    int read()
	    {
	    	return *value;
	    }

	    void write(int x)
	    {
	    	*value = x;
	    }

	    private:
	    	int *value;
    };
```

## 12.	以下代码有什么错误？ ##


```

    std::vector array;
    array.push_back( 1 );
    array.push_back( 2 );
    array.push_back( 3 );
    for(size_t i=array.size()-1; i>=0; --i ) // 反向遍历array数组
    cout << array[i] << endl;
```

	写一个“标准”宏MIN，这个宏输入两个参数并返回较小的一个。另外，当你写下面的代码时会发生什么事？
`least = MIN(*p++,  a);`


## 13.	写一个函数，要求输入是一个int类型的数组，输出是该数组中最大的两个值。 ##
	`void  GetMaxTwo(std::vector<int>& a, int& max1, int& max2);`
	





	









## 14.	简述tcp和udp的区别: ##
	
	
	
	
	
	
	
	
	
	
	
## 15.	在众多设计模式中，列出你最常用的几种模式并简述（至少三种）。 ##
	
	
	
