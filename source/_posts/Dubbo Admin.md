---
title: Dubbo Admin
date: 2019-10-05 21:26:10
update: 2019-10-05 21:26:10
categories: DUBBO
tags: [dubbo]
---

### 下载dubbo-admin 

```
git clone https://github.com/apache/dubbo-admin.git
```

<!-- more -->

### 配置zookepper地址

```powershell
cd F:\wk\tool\dubbo-admin\dubbo-admin-server\src\main\resources
```

```yml
admin.registry.address=zookeeper://192.168.25.11:2181
admin.config-center=zookeeper://192.168.25.11:2181
admin.metadata-report.address=zookeeper://192.168.25.11:2181
```

### build编译

#### 编译报错

```sh
[INFO] Running 'npm install' in F:\wk\tool\dubbo-admin\dubbo-admin-ui
[ERROR] internal/modules/cjs/loader.js:550
[ERROR]     throw err;
[ERROR]     ^
[ERROR]
[ERROR] Error: Cannot find module '../lib/utils/unsupported.js'
[ERROR]     at Function.Module._resolveFilename (internal/modules/cjs/loader.js:548:15)
[ERROR]     at Function.Module._load (internal/modules/cjs/loader.js:475:25)
[ERROR]     at Module.require (internal/modules/cjs/loader.js:598:17)
[ERROR]     at require (internal/modules/cjs/helpers.js:11:18)
[ERROR]     at F:\wk\tool\dubbo-admin\dubbo-admin-ui\node\node_modules\npm\bin\npm-cli.js:19:21
[ERROR]     at Object.<anonymous> (F:\wk\tool\dubbo-admin\dubbo-admin-ui\node\node_modules\npm\bin\npm-cli.js:92:3)
[ERROR]     at Module._compile (internal/modules/cjs/loader.js:654:30)
[ERROR]     at Object.Module._extensions..js (internal/modules/cjs/loader.js:665:10)
[ERROR]     at Module.load (internal/modules/cjs/loader.js:566:32)
[ERROR]     at tryModuleLoad (internal/modules/cjs/loader.js:506:12)
```

解决：

1. 下载安装`nodejs`，安装成功后将`E:\program\nodejs\node_modules`的`node_modules`文件夹复制替换`F:\wk\tool\dubbo-admin\dubbo-admin-ui\node`中的node_modules。
2. 将duboo-amdin项目导入到IDEA中，使用IDEA clean 和 package

### 启动

```powershell
cd dubbo-admin-distribution/target
nohup java -jar dubbo-admin-0.1.jar &
```

### Visit

`http://localhost:8080`

![dubbo-admin-a](https://volc1612.gitee.io/blog/images/dubbo-admin/dubbo-admin-a.png)


### 配置元数据

![dubbo-admin-b](https://volc1612.gitee.io/blog/images/dubbo-admin/dubbo-admin-b.png)

#### `provider`和`consumer`注入bean

```java
import org.apache.dubbo.config.MetadataReportConfig;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class DubboConfig {

    @Bean
    public MetadataReportConfig metadataReportConfig() {
        MetadataReportConfig metadataReportConfig = new MetadataReportConfig();
        metadataReportConfig.setAddress("zookeeper://192.168.25.11:2181");
        return metadataReportConfig;
    }
}
```

![dubbo-admin-c](https://volc1612.gitee.io/blog/images/dubbo-admin/dubbo-admin-c.png)


### Dubbo Admin的日常使用

**简介：dubbo admin的使用**

- 使用dubbo admin查看消费组和提供组节点
- 使用dubbo admin完成后台接口测试
- 使用dubbo admin设置请求权重
- 使用dubbo admin添加黑名单
- dubbo负载均衡策略
  - 轮询调度算法Round Robin Scheduling
    - 轮询调度算法的原理是每一次把来自用户的请求轮流分配给内部中的服务器，从1开始，直到N(内部服务器个数)，然后重新开始循环。算法的优点是其简洁性，它无需记录当前所有连接的状态，所以它是一种无状态调度。  
  - 最少活跃调用数 LeastActive LoadBalance
    - 相同活跃数的随机，活跃数指调用前后计数差。
    - 使慢的提供者收到更少请求，因为越慢的提供者的调用前后计数差会越大