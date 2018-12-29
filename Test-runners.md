
### IDE支持的图形化runners

NetBeans，Eclipse和IntelliJ IDEA都内置图形化test runners

###  基于控制台的Test runner

JUnit提供了定义要运行的suite（套件）并显示其结果的工具。若要运行测试并在控制台上查看结果，请从Java程序中运行以下命令（译者注：程序返回org.junit.runner.Result对象）
```java
    org.junit.runner.JUnitCore.runClasses(TestClass1.class, ...);
```

或者在命令行中运行，保持 test class 和 junit 在classpath中

```java
    java org.junit.runner.JUnitCore TestClass1 [...other test classes...]
```
这个用法在这里有进一步的记录: http://junit.org/javadoc/latest/org/junit/runner/JUnitCore.html

### 使用适配器运行老的runners

`JUnit4TestAdapter` 可以运行 Junit-4风格的测试 使用 Junit-3风格的test runner。若要使用它，请将以下内容添加到测试类中:
```java
      public static Test suite() {
            return new JUnit4TestAdapter('YourJUnit4TestClass'.class);
      }
```
Caveat: See [#1189](https://github.com/junit-team/junit/issues/1189) for issues with including a JUnit-4-style suite that contains a JUnit-3-style suite.

## @RunWith 注解

当一个类使用了 `@RunWith` 注解或者继承了一个使用了 `@RunWith` 注解的类，JUnit将调用它引用的类(@RunWith注解中的runner)来运行该类中的测试，而不是JUnit内置的测试运行程序。
- JavaDoc for @RunWith http://junit.org/javadoc/latest/org/junit/runner/RunWith.html
- 默认runner是  `BlockJUnit4ClassRunner` ,取代了旧的 `JUnit4ClassRunner` 
- 一个使用了 `@RunWith(JUnit4.class)` 注解的类会调用JUnit当前版本的默认JUnit4 runner，这个类(JUnit4.class)是当前默认JUnit 4 class runner

## 特定 Runners ##
### Suite套件 ###
- `Suite` 是一个允许你手动的建立含多个类的测试套件的标准runner。
 - More information at [[Aggregating tests in Suites]] page.
 - JavaDoc: http://junit.org/javadoc/latest/org/junit/runners/Suite.html

### Parameterized参数化 ###
-  `Parameterized` 是一个执行参数化测试的标准测runner。当运行一个参数化测试类，每个实例都在测试方法与测试数据元素间交叉创建。 
 - More information at [[Parameterized Tests]] page.
 - JavaDoc: http://junit.org/javadoc/latest/org/junit/runners/Parameterized.html

### Categories ###
### Categories分类 ###

你可以指定 测试组执行 或者 通过 `Categories` runner 分组。一旦方法上使用 `@Category(MyCategory.class)` 注解，你可以使用 `--filter` 选项去约束那些测试将会运行。
```
    java org.junit.runner.JUnitCore --filter=org.junit.experimental.categories.IncludeCategories=MyCat1,MyCat2 --filter=org.junit.experimental.categories.ExcludeCategories=MyCat3,MyCat4
```
你或许通过 `FilterFactory` 的实例来过滤测试。 `--filter` 选项使用方式如下

    java [Runner] --filter=[FilterFactory]=[Categories,]

- `Categories` 是一个标准的runner，它使带有特定类别标记的测试子集能够在给定的测试运行中执行/被排除在外。

 - More information at [[Categories]] page.

## 实验性 Runners ##
### Enclosed封闭的 ###
- 如果你测试在内部类中，例如，Ant 不会发现他们。通过执行带有Enclosed的外部类，内部类的测试才会执行。你如果你将测试放入内部类去组合他们为了方便或者共享常数。
- JavaDoc: http://junit.org/javadoc/latest/org/junit/experimental/runners/Enclosed.html
- Working Example of use on the [['Enclosed'-test-runner-example]] page

## Third Party Runners ##
## 第三方 Runners ##
Some popular third party implementations of runners for use with `@RunWith` include:
- [SpringJUnit4ClassRunner](http://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/test/context/junit4/SpringJUnit4ClassRunner.html)
- [MockitoJUnitRunner](http://site.mockito.org/mockito/docs/current/org/mockito/runners/MockitoJUnitRunner.html)
- [HierarchicalContextRunner](https://github.com/bechte/junit-hierarchicalcontextrunner/wiki)
- [Avh4's Nested](https://github.com/avh4/junit-nested)
- [NitorCreation's NestedRunner](https://github.com/NitorCreations/CoreComponents/tree/master/junit-runners)
- [[Others...|Custom-runners]]
