---
title: 不同语言 json 与 字符串 的互相转换
---
使用过json的同学应该都能体会到它的方便和简洁，它便于人的阅读理解，对于计算机来说也很容易解析和生成，所以在数据传输和交换过程中被广泛使用，与字符串之间转换是个常见场景，不同语言略有不同，此处介绍一下。

<!--more-->

>每种语言都有很多实现方法来做转换，这里只挑选其中的一种方法介绍，避免混杂方便大家记忆。
>JSON在编程中真正重要的是它通过键值对实现的简单清晰的层次结构，所以这里将 python 中的 dict 也当json进行介绍，除了名称不同，其它优势特点均相同。

## JavaScript中 json 与string 的互相转换

json全称 JavaScript Object Notation, 也就是JS对象标记，和JS的契合度很高，所以转换方法也很简单。

```javascript
JSON.parse(jsonstr); //可以将json字符串转换成json对象   
JSON.stringify(jsonobj); //可以将json对象转换成json对符串
```
## Java中 json 与 String 的互相转换 

这里用到org.json包  
该部分参考http://zhangfan822.iteye.com/blog/1880830

#### json转字符串

直接使用toString方法即可
myString = myjson.toString()

示例：

```java
public String jsonTest() throws JSONException{  
    JSONObject json=new JSONObject();  
    JSONArray jsonMembers = new JSONArray();  
    JSONObject member1 = new JSONObject();  
    member1.put("loginname", "zhangfan");  
    member1.put("password", "userpass");  
    member1.put("email","10371443@qq.com");  
    member1.put("sign_date", "2007-06-12");  
    jsonMembers.put(member1);  
  
    JSONObject member2 = new JSONObject();  
    member2.put("loginname", "zf");  
    member2.put("password", "userpass");  
    member2.put("email","8223939@qq.com");  
    member2.put("sign_date", "2008-07-16");  
    jsonMembers.put(member2);  
    json.put("users", jsonMembers);  
  
    return json.toString();  
}  
```
#### 字符串转json

直接以String作为json初始化参数即可
myjson = new JSONObject(myString)

示例：

```java
public String jsonTest2() throws JSONException{  
    String jsonString="{\"users\":[{\"loginname\":\"zhangfan\",\"password\":\"userpass\",\"email\":\"10371443@qq.com\"},{\"loginname\":\"zf\",\"password\":\"userpass\",\"email\":\"822393@qq.com\"}]}";  
    JSONObject json= new JSONObject(jsonString);  
    JSONArray jsonArray=json.getJSONArray("users");  
    String loginNames="loginname list:";  
    for(int i=0;i<jsonArray.length();i++){  
        JSONObject user=(JSONObject) jsonArray.get(i);  
        String userName=(String) user.get("loginname");  
        if(i==jsonArray.length()-1){  
            loginNames+=userName;  
        }else{  
            loginNames+=userName+",";  
        }  
    }  
    return loginNames;  
}  
```

## Python 中 json 与 string 的互相转换 

Python中json形式的对应类型是dict  
```python
t={'a':1,'b':2}
print type(t)

# out
<type 'dict'>
```
我们这里把dict当作json对象进行使用和转换处理。  

引入模块 ``import json``
```python
json.dumps(json) #json转字符串
json.loads(json_str) #字符串转json
```

## END

这里总结了json、java、python中json和字符串的转换，日后会根据使用经验补充其他语言。

