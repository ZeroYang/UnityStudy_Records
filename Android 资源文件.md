Android assets目录下文件读取

properties配置文件读取

	import java.io.InputStream;
	import java.util.HashMap;
	import java.util.Properties;

	...

	private String getAdChannelChildId()
	{
		Properties properties = new Properties();  
		try {  
			InputStream s = null;
			AssetManager am = this.getAssets();  
			s = am.open("sgzswljqb.properties");
			properties.load(s);  
		} catch (Exception e) {  
			e.printStackTrace();
			return "";
		}  
		String id = properties.getProperty("adChannelChildId", "");
		Log.d(TAG, "id="+id);
		return id;
	}