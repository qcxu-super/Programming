# 功能: 从外部传入参数

要形如下面的传入参数

```
java test.jar --x 1 --y hh ...
```

而不要下面这样

```
java test.jar 1 hh ...
```

- [argparse4j](https://argparse4j.github.io/)

## 1.添加pom文件依赖

```
<dependency>
  <groupId>net.sourceforge.argparse4j</groupId>
  <artifactId>argparse4j</artifactId>
  <version>0.9.0</version>
</dependency>
```

## 2.函数定义

```java
public class TestArgParse {
	public static void main(String[] args) {
		System.out.println("Starting up");
		ArgumentParser parser = ArgumentParsers.newFor("TestArgParse").build()
                .defaultHelp(true);
        parser.addArgument("--string")
                 .action(Arguments.store())
                 .type(String.class)
                 .setDefault("")
                 .nargs("?");
        parser.addArgument("--integers")
                 .type(Integer.class)
                 .setDefault(1)
                 .action(Arguments.store())
                 .nargs("?");
        
        Namespace result = null;
        for (int i = 0; i < args.length(); ++i) {
        	System.out.println("args[" + i + "] = " + args[i]);
        }


        try {
        	result = parse.parseArgs(args);
        	System.out.println("No exception");
        } catch (ArgumentParserException e) {
        	parse.handleError(e);
        	System.out.println("Exception: " + e.getMessage())''
        }

        System.out.println(result);
        System.out.println("string: " + result.getString("string"));
        System.out.println("integer: " + result.get("integers"));
	}
}

```


## 3.主代码

```
set -eu
spark-submit \
--master yarn \
--driver-memory 8g \
--class com.xxx...TestArgParse \
xxx.jar \
--string xxx --integers 777

```