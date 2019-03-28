---
title: springboot获取不到配置
---







### 问题现象

* 使用```@Value```注解不能获取到```application.yml```里面的配置
* 使用的代码从网上找的，大家都是这样写的，无任何特殊的配置

### 源码示例
```
es:
  ip: 172.16.33.29
  port: 9201
  username: admin
  password: 123
```

```
    @Value("${es.ip}")
    private String esIp;

    @Value("${es.port}")
    private String esHttpPort;

    @Value("${es.username}")
    private String esUserName;

    @Value("$(es.password)")
    private String esUserPw;

    /** jestClient 实例化对象 */
    private JestClient jestClient;

    public EsInstance() {
        initJestClient();
    }

    /**
     * 初始化ES
     */
    private void initJestClient() {
        String url = "http://" + esIp + ":" + esHttpPort;
        HttpClientConfig httpClientConfig = new HttpClientConfig.Builder(url)
                .defaultCredentials(esUserName, esUserPw)
                .build();
        CredentialsProvider credentialsProvider = httpClientConfig.getCredentialsProvider();
        HttpClientContext httpClientContextTemplate = HttpClientContext.create();
        httpClientContextTemplate.setCredentialsProvider(credentialsProvider);
        JestHttpClient clientWithMockedHttpClient;
        JestClientFactory factory = new JestClientFactory();
        factory.setHttpClientConfig(httpClientConfig);
        clientWithMockedHttpClient = (JestHttpClient) factory.getObject();
        jestClient = clientWithMockedHttpClient;
    }
```
* 此处获取的相关属性都是空的

### 问题原因
* Spring在扫描注解的时候，当扫描到会进行new对象的操作，因为此类里面有默认无参构造器，所以会直接初始化`initJestClient`方法，但是此时属性注解还是没有注入进去，所以获取的值就是空的！！！

### 解决方案
* 当执行完Spring对象初始化的时候，才走这个初始化的方法

### 问题反思
* 原来那样写，是因为使用的是工具类获取，工具类已经缓存到所有的属性，在bean初始化之前就已经缓存好了

### 生命周期
* Spring Bean的生命周期

 ![image](https://note.youdao.com/yws/public/resource/7fc235c525305c0a08973cbf52212bc5/xmlnote/7C755F1ACCFE4BBC9E511AB294ECBB92/6879)