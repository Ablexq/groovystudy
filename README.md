

推荐阅读：

[Android Gradle配置快速入门](http://www.paincker.com/android-gradle-basics)

[Gradle开发快速入门——DSL语法原理与常用API介绍](http://www.paincker.com/gradle-develop-basics)

[Gradle依赖项学习总结，dependencies、transitive、force、exclude的使用与依赖冲突解决](http://www.paincker.com/gradle-dependencies)

[Gradle插件开发与执行原理浅析](http://www.paincker.com/gradle-plugin-and-principle#_Closure)

![](imgs/01.png)

![](imgs/02.png)

![](imgs/03.png)

Android Studio 的编译构建，是基于Gradle的， 而Gradle又是基于Groovy， Groovy又是基于Java的。

Gradle是一个基于JVM的新一代构建工具，可以用于自动化自定义有序的步骤来完成代码的编译、
测试和打包等工作，让重复的步骤变得简单，用于实现项目自动化，是一种可编程的工具，
你可以用代码来控制构建流程最终生成可交付的软件。构建工具可以帮助你创建一个重复的、
可靠的、无需手动介入的、不依赖于特定操作系统和IDE的构建.

# Groovy基本语法理论介绍

Gradle是一种基于Groovy语言(DSL)来声明的，所以我们很有必要先了解下Groovy 语言。Groovy 语言是用于Java虚拟机的敏捷语言，是可以用于面对对象编程，又可以用作纯粹的脚本语言，同时有具有闭包和动态语言的其他特性。

基本特性：
1. Groovy 完全兼容java语法
2. 末尾的分号是可选的
3. 类、方法默认是public的
4. 编译器会自动添加getter/setter方法
5. 属性可以用.号获取
6. 最后一个表达式的值会作为返回值的，return关键字可省略
7. ==等同于equals(),不会有NullPointerExceptions
8. 在Groovy中使用def关键字定义的变量类型都是Object，
定义方法时，通常可以省略def关键字，尽量别def和类型混用，避免这种写法def String name = "Guillaume"。def类似js中的var

def 定义变量和方法， task 定义可执行的任务
``` 
task method <<{
	add (1,2)  //语句后面的分号可以省略
	minus 1,2  //方法调用时，方法的括号可以省略
}
def add(int a,int b) {  //默认修饰符为public
 println a+b 
}  
def minus(a,b) {//参数类型可以省略
 println a-b
 a-b   //如果不使用return ，方法的返回值为最后一行代码的执行结果。
}   
```


高效特性
1. assert 断言语句 可以写在任何地方
2. 可选类型定义，相当于若类型语言 ，类型会自动推导出来的
3. 可选的括号 ，方法如过有有参数，（）可以不写
4. 字符串 有三种 分别为 ''  、 ""  、  '''  '''   ，引用${}
5. 闭包
闭包是一个变量，又是一个函数，类似C语言中的函数指针，或者Java中只有一个方法的接口（Runnable等）。
反编译class文件可以看出，Groovy闭包都会转化为继承groovy.lang.Closure的类。
闭包方法的参数用箭头定义，如果不特殊指定，则默认有一个it参数。
闭包方法的返回值可以用return显示指定，如果不指定则使用最后一条语句的值。

Groovy中定义方法既可以用Groovy闭包风格，也可以用Java风格，参数/返回值类型也是可选的。
``` 
def f1 = { text ->
    println text
}
def f2(text) {
    println text
}
void f3(String text) {
    println text
}
```

# 

Gradle生命周期分为三个部分，如下：

1 初始化阶段: 

解析整个工程中所有Project,构建所有的Project对应的project对象，这个阶段主要就是解析setting.gradle文件

2 配置阶段：

解析所有的projects对象中的task，构建好所有的task拓扑图，主要解析各个project下的build.gradle文件

3 执行阶段：

执行具体的的task及其依赖task





# 参考：
[apache groovy官方文档](http://www.groovy-lang.org/documentation.html)

[Gradle DSL语法官方文档](https://docs.gradle.org/current/javadoc/)

[使用Groovy开发之新特性](https://www.jianshu.com/p/ba55dc163dfd)

[任玉刚：Gradle从入门到实战 - Groovy基础](https://blog.csdn.net/singwhatiwanna/article/details/76084580)

[刘望舒：Gradle核心思想（三）Groovy快速入门指南](https://blog.csdn.net/itachi85/article/details/84311537)

[Android构建02-Groovy基础](https://www.jianshu.com/p/158222e8035e/)

[Gradle系列之Groovy基础篇](https://juejin.im/post/5d14efc2e51d4510835e02d7?utm_source=gold_browser_extension)

[Android Gradle Groovy自动化构建入门篇](https://www.jianshu.com/p/20cdcb1bce1b)

# 报错：

```
* What went wrong:
Task 'hello' not found in root project 'MyTest'. 
Some candidates are: 'help'.

* Try:
Run gradle tasks to get a list of available tasks. 
Run with --stacktrace option to get the stack trace. 
Run with --info or --debug option to get more log output. 
Run with --scan to get full insights.
```

# Gradle DSL语法

Groovy是一种开源的脚本语言，在Java基础上进行了扩展，支持闭包、动态类型、元编程等特性，
几乎兼容所有Java语法。因此很容易用Groovy实现领域特定语言(DSL, Domain-Specific Language)。

每一个Gradle构建脚本中都会包含三个基本的构建块，project、task和property。

gradle -q taskName

``` 
//查看依赖树
gradle dependencies 
//查看所有tasks
gradle tasks
//查看project
gradle project


//编译并打包
gradle assemble
gradle assembleDebug
gradle assembleRelease

gradle check
gradle build
gradle clean
```
build任务：构建项目

clean任务：删除build目录及构建生成的文件

assemble任务：不执行单元测试，只编译和打包

check任务：只执行单元测试

javadoc任务：生成Java格式的doc api文档


Gradle日志级别:
``` 
级别      用于
ERROR   错误消息
QUIET     重要的信息消息
WARNING 警告消息
LIFECYCLE   进度信息消息
INFO    信息性消息
DEBUG   调试消息
```
gradle -q +任务名称来运行一个指定的task，这个q是命令行开关选项，
通过开关选项可以控制输出的日志级别。

```
开关选项    输出日志级别
无日志选项   LIFECYCLE及更高级别
-q或者 --quiet    QUIET及更高级别
-i或者 --info INFO及更高级别
-d或者 --debug    DEBUG及更高级别
``` 
# 看懂Gradle脚本
 
[看懂Gradle脚本（1）- Groovy语言的Map语法糖](https://blog.csdn.net/zxhoo/article/details/29570685)

[看懂Gradle脚本（2）- Groovy语言的闭包语法](https://blog.csdn.net/zxhoo/article/details/29601675)

[看懂Gradle脚本（3）- Groovy AST转换](https://blog.csdn.net/zxhoo/article/details/29830529)


``` 
apply plugin: 'com.android.application'
//apply方法，参数是个map
//void apply(Map<String, ?> var1);
//如果传入方法的参数是Map，那么Map字面量可以省略方括号
//方法调用省略了小括号
apply ([plugin: 'com.android.application'])

testCompile group: 'junit', name: 'junit', version: '4.12'
//等价于：
testCompile ([group: 'junit', name: 'junit', version: '4.12'])


//以下三种方式完全等价：
//task   myTask     {...}
//task  'myTask'    {...}
//task ('myTask',   {...})


//外部依赖 完整写法
compile group:'com.squareup.okhttp3',name:'okhttp', version:'3.0.1' 
//外部依赖 简单写法
compile 'com.squareup.okhttp3:okhttp:3.0.1' 
```

# 自定义属性 ext ：

Project、Task和SourceSet都允许用户添加额外的自定义属性、并对自定义属性进行读取和设置

方式：通过ext属性，添加多个通过ext代码块

优点：相比局部变量有广泛的作用域，可以跨Project、跨Task访问，只要能访问这些属性所属的对象即可

[要点提炼| Gradle指南](https://www.jianshu.com/p/1274c1f1b6a4)








