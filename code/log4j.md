## 最简单的配置
```
log4j.rootLogger = error,stdout

### 输出信息到控制抬 ###
log4j.appender.stdout = org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target = System.out
log4j.appender.stdout.layout = org.apache.log4j.PatternLayout
#log4j.appender.stdout.layout.ConversionPattern = [%-5p]method:%l%n%m%n
log4j.appender.stdout.layout.ConversionPattern = %l[%-5p]:%m%n


log4j.logger.com = debug,cfs
log4j.additivity.com=false
### 输出信息到控制抬 ###
log4j.appender.cfs = org.apache.log4j.ConsoleAppender
log4j.appender.cfs.Target = System.out
log4j.appender.cfs.layout = org.apache.log4j.PatternLayout
#log4j.appender.stdout.layout.ConversionPattern = [%-5p]method:%l%n%m%n
log4j.appender.cfs.layout.ConversionPattern = %c%t[%-5p]:%m%n

```
