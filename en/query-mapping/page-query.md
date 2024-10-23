# Page Query

## Definition

`PageQuery` implements the `Query` interface. `PageQuery` has three fields, `PageNumber` and `PageSize` is used to build the paging clause, and `Sort` is used to build the sorting clause.

```go
package core

type PageQuery struct {
	PageNumber *int    `json:"page,omitempty"`
	PageSize   *int    `json:"size,omitempty"`
	Sort       *string `json:"sort,omitempty"`
}
```

## Paging

```go
userQuery := UserQuery{PageQuery: PageQuery{}}
users, err := userDataAccess.Query(ctx, userQuery)
//SELECT id, score, memo FROM User

userQuery := UserQuery{PageQuery: PageQuery{PageSize: P(20)}}
users, err := userDataAccess.Query(ctx, userQuery)
//SELECT id, score, memo FROM User LIMIT 20 OFFSET 0

// When only PageNumber is set, PageSize will be set to 10
userQuery := UserQuery{PageQuery: PageQuery{PageNumber: P(5)}}
users, err := userDataAccess.Query(ctx, userQuery)
//SELECT id, score, memo FROM User LIMIT 10 OFFSET 40

userQuery := UserQuery{PageQuery: PageQuery{PageSize: P(50), PageNumber: P(3)}}
users, err := userDataAccess.Query(ctx, userQuery)
//SELECT id, score, memo FROM User LIMIT 10 OFFSET 100	
```

## Sorting

The `Sort` string should follow `regexp.MustCompile("(?i)(\w+)(,(asC|dEsc))?;?")`

```go
userQuery := UserQuery{PageQuery: PageQuery{Sort: P("id,desc;score,asc;memo")}}
users, err := userDataAccess.Query(ctx, userQuery)
//SELECT id, score, memo FROM User ORDER BY id DESC, score ASC, memo
```
