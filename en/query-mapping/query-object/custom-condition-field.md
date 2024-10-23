# Custom Condition Field

For types of query conditions that are not currently supported, GoooQo uses the tag `condition` to directly write native SQL conditions:&#x20;

```go
type UserQuery struct {
    PageQuery
    Account    *string `condition:"(username = ? OR email = ?)"`
    //...
}
```

Here is a sample:&#x20;

```go
userQuery := UserQuery{Account: P("John")}
users, err := userDataAccess.Query(ctx, userQuery)
// SQL="SELECT id, name, score, memo, deleted FROM t_user
//   WHERE (username = ? OR email = ?)" args="[John John]"
```

