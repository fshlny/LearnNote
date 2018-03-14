# log4j配置详解

先放上自己demo的配置

```
log4j.rootLogger=ERROR,LOG1
log4j.Threshold=ALL

log4j.logger.LOG2=DEBUG,LOG2
log4j.additivity.LOG2=false
  
#LOG1----------  
log4j.appender.LOG1=org.apache.log4j.DailyRollingFileAppender
log4j.appender.LOG1.Append=true
log4j.appender.LOG1.DatePattern='_'yyyy-MM-dd-HH-mm
log4j.appender.LOG1.File=logs/LOG1.log
#log4j.appender.LOG1.Threshold=DEBUG
log4j.appender.LOG1.Encoding=UTF-8
log4j.appender.LOG1.layout=org.apache.log4j.PatternLayout
log4j.appender.LOG1.layout.ConversionPattern=%d{HH:mm:ss} [%-5p] %m%n
#LOG2----------  
log4j.appender.LOG2=org.apache.log4j.DailyRollingFileAppender
log4j.appender.LOG2.Append=true
log4j.appender.LOG2.DatePattern='_'yyyy-MM-dd-HH-mm
log4j.appender.LOG2.File=logs/LOG2.log
log4j.appender.LOG2.Threshold=INFO 
log4j.appender.LOG2.Encoding=UTF-8
log4j.appender.LOG2.layout=org.apache.log4j.PatternLayout
log4j.appender.LOG2.layout.ConversionPattern=%d{HH:mm:ss} [%-5p] %m%n
```

## log4j属性

#### 1. log4j.rootLogger=INFO,LOG1 

rootLogger是log4j的根logger，根日志就是所有的日志的父类，如果在代码中写入了没有配置的日志，这个时候就会找到根日志，如果输
出的级别大于根日志的级别就会输出到根日志中，相当于根日志是默认的日志。可以通过`Logger.getRootLogger()`方法来获取到`rootLogger`对象，
这里获取到的也就是`LOG1`日志对象。上面配置了rootLogger的日志等级为INFO，日志对象为LOG1。

#### 2. Threshold

用于指定`Appender`的输出日志等级，等级分为；`OFF` ,`FATAL` ,`ERROR` ,`WARN` ,`INFO` ,`DEBUG` ,`ALL`。`Threshold`是个全局的过滤器，它将把低于所设置的level的信息过滤不显示出来。

如上面`LOG2`的`Threshold`为,当代码中打印日志的等级小于`INFO`，则会被过滤。

#### 3. log4j.additivity 

它是`子Logger` 是否继承`父Logger`的输出源（`appender`）的标志位。具体说，默认情况下`子Logger`会继承`父Logger`的`appender`，也就是说`子Logger`会在`父Logger`的`appender`里输出。若是`additivity`设为`false`，则`子Logger`只会在自己的`appender`里输出，而不会在`父Logger`的`appender`里输出。也就是`LOG2`输出的日志内容会输出到`LOG2`的日志文件中去.

#### 4. Appender

log4j提供了一下`Appender`实现。

```
ConsoleAppender
DailyRollingFileAppender
RollingFileAppender
ExternallyRolledFileAppender

```

> ConsoleAppender

控制台输出，ConsoleAppender使用用户指定的布局将日志事件附加到System.out或System.err。 默认目标是System.out。

> DailyRollingFileAppender

文件输出，每日产生一个文件。

> RollingFileAppender

RollingFileAppender扩展FileAppender以在日志文件达到特定大小时进行备份。

> ExternallyRolledFileAppender

由`RollingFileAppender`扩展而来，它将日志通过`Socket`连接，把日志发送到其他机器上保存

#### 5. File

指定输出的文件目录和名称

#### 6. Encoding

输出文件编码格式

#### 7. layout  输出格式

org.apache.log4j.HTMLLayout（以HTML表格形式布局）

org.apache.log4j.PatternLayout（可以灵活地指定布局模式）

org.apache.log4j.SimpleLayout（包含日志信息的级别和信息字符串）

org.apache.log4j.TTCCLayout（包含日志产生的时间、线程、类别等等信息）

#### 8. ConversionPattern 内容格式 

> %d: 输出日志时间点的日期或时间，默认格式为ISO8601，也可以在其后指定格式，比如：%d{yyyy-MM-dd HH:mm:ss,SSS}，输出类似：2011-10-18 22:10:28,921 

> %r: 输出自应用启动到输出该log信息耗费的毫秒数 

> %c: 输出日志信息所属的类目，通常就是所在类的全名 

> %t: 输出产生该日志事件的线程名 

> %l: 输出日志事件的发生位置，相当于%C.%M(%F:%L)的组合,包括类目名、发生的线程，以及在代码中的行数。 

> %x: 输出和当前线程相关联的NDC(嵌套诊断环境),尤其用到像java servlets这样的多客户多线程的应用中。 

> %%: 输出一个"%"字符 

> %F: 输出日志消息产生时所在的文件名称 

> %L: 输出代码中的行号 

> %m: 输出代码中指定的消息,产生的日志具体信息 

> %n: 输出一个回车换行符，Windows平台为"\r\n"，Unix平台为"\n"输出日志信息换行 


可以在%与模式字符之间加上修饰符来控制其最小宽度、最大宽度、和文本的对齐方式。如： 

1) %20c：指定输出category的名称，最小的宽度是20，如果category的名称小于20的话，默认的情况下右对齐。 

2) %-20c:指定输出category的名称，最小的宽度是20，如果category的名称小于20的话，"-"号指定左对齐。 

3) %.30c:指定输出category的名称，最大的宽度是30，如果category的名称大于30的话，就会将左边多出的字符截掉，但小于30的话也不会有空格。 

4) %20.30c:如果category的名称小于20就补空格，并且右对齐，如果其名称长于30字符，就从左边交远销出的字符截掉

#### 9. DatePattern

日期格式化后的文件名后缀，如：

```
# 每分钟产生一个文件
log4j.appender.LOG2.DatePattern='_'yyyy-MM-dd-HH-mm

#每一天产生一个文件
log4j.appender.LOG2.DatePattern='_'yyyy-MM-dd

#每一个小时产生一个文件
log4j.appender.LOG2.DatePattern='_'yyyy-MM-dd-HH

```

## 参考资料

1. [深入Log4J源码之Log4J Core](http://www.blogjava.net/DLevin/archive/2012/06/28/381667.html)

2. [log4j中Pattern布局ConversionPattern详解](http://blog.csdn.net/jethai/article/details/52345140)
