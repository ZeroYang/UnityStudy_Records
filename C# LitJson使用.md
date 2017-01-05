#C# LitJson使用

    using LitJson;

json对象嵌套json对象的情况
```
        string result = "{\"state_code\":1101,\"message\":\"网关获取成功 网关获取成功 \",\"data\":{\"host\":\"127.0.0.1\",\"port\":\"8080\",\"desp\":\"\",\"status\":1,\"resource_url\":\"http://www.baidu.com\",\"serverlist_url\":\"http://cdn.com/serverlist_common.json; http://cdn1.com/serverlist_common.json\"}}";
```


##解析

    class Host
    {
        public string host;//地址
        public string port;//端口
        public string desp;//描述
        public int status;//1 返回正常地址端口 -1不返回地址端口、资源等只返回描述 
        public string resource_url;//资源下载地址
        public string serverlist_url;//serverlist获取地址
    }

    JsonData jsondata = JsonMapper.ToObject(result);
    JsonData data = jsondata["data"];  
    Debug.Log(data.ToJson());  
    hostData = JsonMapper.ToObject<Host>(data.ToJson());
    //hostData = new Host();
    //hostData.host = (string)data["host"];
    //hostData.port = (string)data["port"];
    //hostData.desp = (string)data["desp"];
    //hostData.status = (int)data["status"];
    //hostData.resource_url = (string)data["resource_url"];
    //hostData.serverlist_url = (string)data["serverlist_url"];
    Debug.Log(hostData.serverlist_url);

##json生成

对象中嵌套对象：`{“name”:”peiandsky”,”info”:{“sex”:”male”,”age”:28}}`

     JsonData data2 = new JsonData();
    
     data2["name"] = "peiandsky";
    
     data2["info"] = new JsonData();
    
     data2["info"]["sex"] = "male";
    
     data2["info"]["age"] = 28;
    
     string  json2 = data2.ToJson();