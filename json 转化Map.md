---
title: json 转化Map
---

### json 转化Map
* 正常的fastjson和sf.json是无法的几行代码之内将json转化成Map
* 当时Gson就可以满足当前的需求

```
@Data
public class Test {
    
    private String id;
    
    private String name;
    
}

public class GsonTest {
    
    public static void main(String[] args) {
        ...
        Map<String, List<Test>> testMap = new HashMap<>();
        // 装Map
        String testMapJsonStr = "";
        
        // 利用Gson 转化Map
        Gson gson = new Gson();
        Map<String, List<Test>> newTestMap = gson.fromJson(dzdmJson, new TypeToken<Map<String, List<Test>>>(){}.getType());
    }
    
}

```
```
public static final <T> T toObject(String jsonString, TypeReference<T> type) {
    return JSON.parseObject(jsonString, type);
}
```

### 2 2018 0127 补充
* 在使用Gson的时候，发现不能格式化带有int的json字符串。报错如下：
![image](https://note.youdao.com/yws/public/resource/7fc235c525305c0a08973cbf52212bc5/xmlnote/B2FA8FC518424E2AAAEAFF0E730B5B26/6692)

* 示例json
```
  3: {
    "id": 3,
    "name": "背叛国家罪",
    "open": false,
    "pId": 2,
    "title": "背叛国家罪"
  },
  4: {
    "id": 4,
    "name": "分裂国家罪",
    "open": false,
    "pId": 2,
    "title": "分裂国家罪"
  },
```
* 解决方案
    > 使用fastjson的类库进行格式化
```
JSON.parseObject(str, new TypeReference<Map<Integer, Bean>>(){});
```