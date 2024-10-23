# Query Object

### Example

```go
type UserQuery struct {
    PageQuery
    IdGt     *int64
    IdIn     *[]int64
    ScoreLt  *int
    MemoNull *bool
    MemoLike *string
    Deleted  *bool
    UserOr   *[]UserQuery
    
    ScoreGtAvg *UserQuery `subquery:"select:avg(score),from:UserEntity"`
    ScoreLtAny *UserQuery `subquery:"select:score,from:UserEntity"`
    ScoreLtAll *UserQuery `subquery:"select:score,from:UserEntity"`
}
```

### Query Interface

The query object needs to implement the `Query` interface for the construction of paging clause and sorting clause:&#x20;

```go
package core

type Query interface {
    GetPageNumber() int
    GetPageSize() int
    CalcOffset() int
    GetSort() *string
    NeedPaging() bool
}
```

The `PageQuery` struct provides a standard implementation for the query structs.&#x20;

{% content-ref url="../page-query.md" %}
[page-query.md](../page-query.md)
{% endcontent-ref %}

### Fields Definition

The query object is used to map the dynamic part of the SQL statements, such as filter conditions, pagination, and sorting.

Each field in the query object is used to map a query condition.

Check the following docs for how to define the fields in GoooQo:

{% content-ref url="predicate-suffix-field.md" %}
[predicate-suffix-field.md](predicate-suffix-field.md)
{% endcontent-ref %}

{% content-ref url="logic-suffix-field.md" %}
[logic-suffix-field.md](logic-suffix-field.md)
{% endcontent-ref %}

{% content-ref url="subquery-field.md" %}
[subquery-field.md](subquery-field.md)
{% endcontent-ref %}

{% content-ref url="e-r-query-field.md" %}
[e-r-query-field.md](e-r-query-field.md)
{% endcontent-ref %}

{% content-ref url="custom-condition-field.md" %}
[custom-condition-field.md](custom-condition-field.md)
{% endcontent-ref %}

