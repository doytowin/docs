# Subquery Field

对于一般的子查询条件，例如`score > (SELECT avg(score) FROM t_user WHERE removed = ?)`，在OQM中被分为三个部分分别进行映射：

* score >
* SELECT avg(score) FROM t_user
* WHERE clause

第一部分`score >`， 可以使用字段名`scoreGtXxx`来映射得到。
谓词后缀之后定义的字符串仅用于区分重复的字段名，在映射时会被忽略。

第二部分包含一个列名和一个表名，这些属于不变的静态，GoooQo提供了两种标签来保存这些信息：
一种是标签`subquery`，用于定义该子查询语句；
一种是标签`select`和`from`的组合，分别保存列名和表名。

第三部分是另一个WHERE子句，可以通过查询对象来映射。因此，我们将字段类型定义为对应的查询对象，并通过查询对象映射方法将字段的值映射为子查询中WHERE子句。

### 示例

```java
@SuperBuilder
@NoArgsConstructor
public class UserQuery extends PageQuery {
    // ...
    
    @Subquery(select = "avg(score)", from = UserEntity.class)
    private UserQuery scoreLtAvg;

    @Subquery(select = "score", from = UserEntity.class)
    private UserQuery scoreLtAny;

    @Subquery(select = "score", from = UserEntity.class)
    private UserQuery scoreLtAll;

    @Subquery(select = "avg(score)", from = UserEntity.class)
    private UserQuery scoreGtAvg;
}
```
