# Jersey 2.x throws NegativeArraySizeException when try to logging request

由于Jersey 2.x在最新的release中使用LoggingFeature代替LoggingFilter记录reqeust和response的log，在使用POST请求是，抛出NegativeArraySizeException。
```java
java.lang.NegativeArraySizeException
    at org.glassfish.jersey.logging.LoggingInterceptor.logInboundEntity(LoggingInterceptor.java:210) ~[jersey-common-2.23.2.jar:?]
    at org.glassfish.jersey.logging.ServerLoggingFilter.filter(ServerLoggingFilter.java:108) ~[jersey-common-2.23.2.jar:?]
    at org.glassfish.jersey.server.ContainerFilteringStage.apply(ContainerFilteringStage.java:132) ~[jersey-server-2.23.2.jar:?]
    at org.glassfish.jersey.server.ContainerFilteringStage.apply(ContainerFilteringStage.java:68) ~[jersey-server-2.23.2.jar:?]
```

其根本原因是使用不正确,代码如下
```Java
jerseyConfig.register(
        new LoggingFeature(java.util.logging.Logger.getLogger(LoggingFeature.DEFAULT_LOGGER_NAME),
          java.util.logging.Level.SEVERE, 
          LoggingFeature.Verbosity.PAYLOAD_ANY, 
          Integer.MAX_VALUE)
       );
```
在上面代码中，第四个参数maxEntitySize为Integer.MAX_VALUE。当创建logging buffer时，Jersey会初始化数组大小为 maxEntity + 1，所以引起了那个exception。