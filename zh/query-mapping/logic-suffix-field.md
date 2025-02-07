# 逻辑后缀字段

默认情况下，查询对象各个字段对应的查询条件之间是通过AND连接起来的。

## Or后缀

如果需要使用逻辑运算符OR连接查询条件，则需要在查询对象中定义一个后缀为Or的结构体或数组。

GoooQo支持以下三种定义方式：

```java
type UserQuery struct {
	PageQuerygo
	//...
	NameStartOr *[]string
	UserOr      *UserQuery
	UsersOr     *[]UserQuery
}
```

### List\<String\> nameStartOr;

```java
UserQuery userQuery = UserQuery.builder().nameStartOr(Arrays.asList("Bob","John","Tim")).build();
List<UserEntity> users = userDataAccess.Query(userQuery);
// SQL="SELECT id, name, score, memo, deleted FROM t_user 
// WHERE (name LIKE ? OR name LIKE ? OR name LIKE ?)" args="[Bob% John% Tim%]"
```

### UserQuery userOr;

```java
UserQuery userQuery = UserQuery.builder().nameStartOr(Arrays.asList(1, 4, 12)).deleted(trur).build();
List<UserEntity> users = userDataAccess.Query(userQuery);
// SQL="SELECT id, name, score, memo, deleted FROM t_user 
// WHERE (id IN (?, ?, ?) OR deleted = ?)" args="[1 4 12 true]"
```

### List\<UserQuery> UsersOr;

```java
userQuery := UserQuery{UsersOr: &[]UserQuery{
	{IdIn: &[]int64{1, 4, 12}, Deleted: P(true)},
	{IdGt: P(int64(10)), Deleted: P(false)},
}}
List<UserEntity> users = userDataAccess.Query(userQuery);
// SQL="SELECT id, name, score, memo, deleted FROM t_user
// WHERE (id IN (?, ?, ?) AND deleted = ? OR id > ? AND deleted = ?)"
// args="[1 4 12 true 10 false]"
```

## And后缀

当字段的名称以`And`结尾时，连接多个查询条件的逻辑运算符为AND。

```java
type UserQuery struct {
	PageQuerygo
	//...
	UserOr      *UserQuery
	UserAnd     *UserQuery
}
```

### UserAnd \*UserQuery

```java
userQuery := UserQuery{ScoreLt: P(80), UserOr: &UserQuery{Deleted: P(true),
    UserAnd: &UserQuery{IdIn: &[]int64{1, 4, 12}, Deleted: P(false)}}}
List<UserEntity> users = userDataAccess.Query(userQuery)
// SQL="SELECT id, name, score, memo, deleted FROM t_user
// WHERE score < ? AND (deleted = ? OR id IN (?, ?, ?) AND deleted = ?)"
// args="[80 true 1 4 12 false]"
```

### 相关文章

[在GoooQo中怎么表达select * from user where id = ? or (name = ? and age = ?)](https://blog.doyto.win/post/goooqo-or-clause/)
