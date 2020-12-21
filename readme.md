# dubbo

dubbo 测试，基于 springboot

## 环境

- docker
- java8 (jdk15 测试失败)
- maven
- springboot
- vscode

1. vscode java8 配置，需修改为自己机器的 jdk home：

```
"java.configuration.runtimes": [
    {
      "name": "JavaSE-8",
      "path": "/usr/lib/jvm/java-1.8.0-openjdk-amd64",
      "default": true
    },
]
```

2. maven settings.xml 配置：

```
<profile>
		<id>jdk-14</id>
	    <activation>
	        <jdk>14</jdk>
	    </activation>
	    <properties>
	        <maven.compiler.source>14</maven.compiler.source>
	        <maven.compiler.target>14</maven.compiler.target>
	        <maven.compiler.compilerVersion>14</maven.compiler.compilerVersion>
	    </properties>
	</profile>
	<profile>
		<id>jdk-11</id>
	    <activation>
	        <jdk>11</jdk>
	    </activation>
	    <properties>
	        <maven.compiler.source>11</maven.compiler.source>
	        <maven.compiler.target>11</maven.compiler.target>
	        <maven.compiler.compilerVersion>11</maven.compiler.compilerVersion>
	    </properties>
	</profile>
	<profile>
		<id>jdk-8</id>
	    <activation>
	        <activeByDefault>true</activeByDefault>
	        <jdk>8</jdk>
	    </activation>
	    <properties>
	        <maven.compiler.source>8</maven.compiler.source>
	        <maven.compiler.target>8</maven.compiler.target>
	        <maven.compiler.compilerVersion>8</maven.compiler.compilerVersion>
	    </properties>
	</profile>
<profile>
```

> 设置 java8 compile level 为默认编译级别： <activeByDefault>true</activeByDefault>

3. 全局 java 环境改为 1.8

否则打包时会出现如下错误：
https://github.com/google/error-prone/issues/359

## 概念

生产者、消费者、注册中心、监控

## 配置注册中心

> ** 注意： 用于本地 zk 测试，此配置可忽略 **

采用 zookeeper：https://hub.docker.com/_/zookeeper

0. docker-compose.yml 中注释：volumes
1. docker-compose up
2. 复制关键文件到宿主机：

```
   dubbo docker cp f99098d79109:/data ./zoo1/
   dubbo docker cp f99098d79109:/conf ./zoo1/conf
   dubbo docker cp f99098d79109:/datalog ./zoo1/
```

3. docker-compose down
4. 关闭 volumes 的注释
5. docker-compose up
6. 配置 zookeeper

> 分单机，多机注册中心两个配置版本。

## hello world 例子

1.  启动容器：
    docker-compose up
    lzd #检查日志，看 zk 集群配置是否正确
2.  浏览器登录 dubbo admin：
    http://localhost:8080
3.  测试
    目录： dubbo-spring-boot-project/dubbo-spring-boot-samples/registry-samples/zookeeper-samples
    - 配置和启动 provider
    - 配置和启动 consumer
    - pring “dubbo-registry-zookeeper-provider-sample] : Hello, mercyblitz”
