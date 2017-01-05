#Unity3D与iOS交互

在游戏开发中，会调用一些iOS的库功能，或者第三方sdk接入，需要获取返回值，除unity提供了一个方法（UnitySendMessage）来把iOS中的数据回调到unity的游戏内部。
当然还可以在unity内部通过调用具有返回值iOS的方法把iOS中的数据传入unity中.

一、首先在IOS中我们需要一个NString类型处理函数。

	char* _MakeStringCopy( const char* string)
	{
		if (NULL == string) {
			return NULL;
		}
		char* res = (char*)malloc(strlen(string)+1);
		strcpy(res, string);//必须copy一份 避免il2cpp中自动释放内存错误
		return res;
	} 


二、然后就可以返回一切的NString类型到unity中了。下面是在unity中调用的函数。

	const char* getUDID()
	{
		return _MakeStringCopy([[APService registrationID] UTF8String]);
	} 


三、在unity中调用示例

	[DllImport("__Internal")]
	private static extern string getUDID();  
	
	
	public static string GetUdid()
	{
		return getUDID();
	}


 参考：

C# 中调用 C++ 生成的dll

1：C# 调用 返回 字符串 C++ native dll 函数 的注意事项：

a：C++ DLL的返回值，安全的做法是分配一个全局 char 数组，把要返回的 char * 复制到这个 char 数组中，

	char   buff[255];    
	
	const char* __stdcall ReturnString()
	{
	  strcpy(buff,"xxxxxxxxxxxxxxx");
	  return buff;
	 }
b：C# 收到 字符串后，需要 Marshal

	[DllImport("VC.dll", EntryPoint = "ReturnString", CharSet = CharSet.Ansi, CallingConvention = CallingConvention.Winapi)]
	public static extern IntPtr ReturnString();

//调用VCDLL的代码...

	IntPtr intPtr = ReturnString();
	
	string str = Marshal.PtrToStringAnsi(intPtr);
	
	...

因为 C++ 返回的是 char* ，是个指针，所以c# 要用 IntPtr 来接回。

Marshal.PtrToStringAnsi MSDN上的解释：将非托管 ANSI 字符串中第一个空值（空值就是\0）之前的所有字符复制到托管 String。将每个 ANSI 字符扩展为 Unicode 字符。