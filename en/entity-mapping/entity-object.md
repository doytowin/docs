# Entity Object

### Example

```go
import (
	. "github.com/doytowin/goooqo"
)

type UserEntity struct {
	Int64Id
	Name  *string `json:"name,omitempty"`
	Score *int    `json:"score,omitempty"`
	Memo  *string `json:"memo,omitempty"`
}

func (u UserEntity) GetTableName() string {
	return "t_user"
}
```

### Definition

An entity object is used to provide the table name and column names for CRUD statements construction in GoooQo.

The entity struct needs to implement the following interface:

```go
package core

type Entity interface {
	GetId() any

	// SetId set id to self.
	// self: the pointer points to the current entity.
	// id: type could be int, int64, or string so far.
	SetId(self any, id any) error
}


package rdb

import "github.com/doytowin/goooqo/core"

type RdbEntity interface {
	core.Entity
	GetTableName() string
}
```

* `GetId` is used to build a `Update` statement.
* `SetId` is used to set the generated ID to an entity.
* `GetTableName` is used to provide the table name corresponding to the entity.
* Each field in the entity needs to correspond to a column in the table.

GoooQo provides two `Entity` implementations, `IntId` and `Int64Id`, to simplify entity definition.

The CRUD statements of the `UserEntity` in the example are:

```sql
SELECT id, name, score, memo FROM t_user；
INSERT INTO t_user (name, score, memo) VALUES (?, ?, ?)
UPDATE t_user SET name = ?, score = ?, memo = ? WHERE id = ?;
DELETE FROM t_user WHERE id = ?;
```

