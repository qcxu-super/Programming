# 功能: 读取yaml文件

要读取如下的yaml文件

```
- input: table_name1
- output: table_name2
- schema:
  - item[$].xxx
  - user.xxx
```

- [jackson-yaml](https://www.baeldung.com/jackson-yaml)

## 1.添加pom文件依赖

```
<dependency>
    <groupId>com.fasterxml.jackson.dataformat</groupId>
    <artifactId>jackson-dataformat-yaml</artifactId>
    <version>2.13.3</version>
</dependency>
```

## 2.主代码

```java
public class TestYaml {
    public static void main(String[] args) throw IOException {
        String filePath = ".../test.yaml";
        Path path = Paths.get(filePath);
        byte[] data = Files.readAllBytes(path);

        ObjectMapper mapper = new ObjectMapper(new YAMLFactory());
        JsonNode jsonNode = mapper.readTree(data);
        JsonNode input = jsonNode.findValue("input");
        JsonNode output = jsonNode.findValue("output");
        JsonNode schema = jsonNode.findValue("schema");
        System.out.println(input);
        System.out.println(output);
        System.out.println(schema);
        ArrayList<String> featureList = new ArrayList<String>();
        for (int i = 0; i < schema.size(); ++i) {
            String s = schema.get(i).toString();
            featureList.add(s);
        }
        System.out.println(featureList);
    }
}
```

jsonNode是一个结构体。如果yaml文件长下面这个样子，还可以用迭代器的方式遍历

```
- input: xxx
- input: yyy
- input: kkk
```

```java
JsonNode jsonNode = mapper.readTree(data);
Iterator<JsonNode> elements = jsonNode.elements();
while (elements.hasNext()) {
    JsonNode child = elements.next();
    System.out.println(child.get("input"));
}

```