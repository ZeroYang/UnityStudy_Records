#C# 替换文件中的字符    

	static void ReplaceChar(string name)
	{
	   string path = string.Format("{0}/UIXML/{1}.xml", Application.streamingAssetsPath, name);
	   string text = string.Empty;
	   using (FileStream fs = new FileStream(path, FileMode.Open, FileAccess.Read))
	   {
	       using (StreamReader sr = new StreamReader(fs))
	       {
	           text = sr.ReadToEnd();
	           text = text.Replace("“", "\"");
	           text = text.Replace("”", "\"");
	           text = text.Replace("《", "<");
	           text = text.Replace("》", ">");
	       }
	   }
	
	   using (FileStream fs = new FileStream(path, FileMode.Create, FileAccess.Write))
	   {
	       using (StreamWriter sw = new StreamWriter(fs))
	       {
	           sw.Write(text);
	       }
	   }
	}