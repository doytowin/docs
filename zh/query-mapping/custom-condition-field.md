# 自定义查询字段

## 使用场景

当现有的的字段后缀和注解都无法映射成需要的SQL语句时，可以考虑使用@QueryField注解。

## 注解定义

{% code title="QueryField.java" %}
```java
@Target(FIELD)
@Retention(RUNTIME)
public @interface QueryField {
    String and();
}
```
{% endcode %}

{% hint style="info" %}
当被注解字段的值满足过滤条件时，and变量里定义的条件语句将会被拼接到SQL中。
{% endhint %}

## 代码示例

### 业务代码

```text
public class TestQuery extends PageQuery {
    @QueryField(and = "(username = ? OR email = ? OR mobile = ?)")
    private String account;
}
```

### 单元测试

```text
@Test
void testQueryField() {
    TestQuery testQuery = TestQuery.builder().account("test").build();
    ArrayList<Object> argList = new ArrayList<>();

    String sql = BuildHelper.buildWhere(testQuery, argList);

    assertThat(sql).isEqualTo(" WHERE (username = ? OR email = ? OR mobile = ?)");
    assertThat(argList).containsExactly("test", "test", "test");
}
// passed
```

可以看到，TestQuery的account字段通过@QueryField注解定义的查询条件被原样拼接到where语句，并且因为查询条件里有3个占位符，account的值“test”也被添加了三次到argList。
