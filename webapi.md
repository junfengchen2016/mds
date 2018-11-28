# webapi

## c#
```csharp
var url = "http://api.m.taobao.com/rest/api3.do?api=mtop.common.getTimestamp";
WebClient client = new WebClient();
client.Headers["Type"] = "GET";
client.Headers["Accept"] = "application/json";
client.Encoding = Encoding.UTF8;
string json = client.DownloadString(url);
JObject jo = (JObject)JsonConvert.DeserializeObject(json);
```

## 时间
* [淘宝](http://api.m.taobao.com/rest/api3.do?api=mtop.common.getTimestamp)
* [苏宁](http://quan.suning.com/getSysTime.do)

