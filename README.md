# MyLab.DBLocalization

## DBLocalization.db
### Purpose
Decrease table and column size and complexity for localization in database.

### Query localization
``` sql
select
    p.Name,
    l.code,
    local.Data
from
    Product p
    join Localization local on  p.TitleLocalizationMapId = local.LocalizationMapId
    join Language l on local.LanguageId = l.Id
where
    p.Name = 'Book'
    and l.Code = 'zh-TW'
```

### Explain
Use a `Localization.LocalizationMapId` instead a separate localization mapping table.

### Trade-offs
This design involves trade-offs, including a higher level of abstraction, which may complicate ORM navigation properties and implicit joins.

## PivotDBLocalization.db
### Purpose
Support flexible localization across multiple product properties without a fixed schema.

### Query localization
``` sql
select
    p.Name,
    l.Code,
    local.Data
from
    Product p
    join ProductProperty pp on pp.ProductId = p.Id
    join Localization local on pp.LocalizationMapId = local.LocalizationMapId
    join Language l on local.LanguageId = l.Id
where
    p.Name = 'Book'
    and l.Code = 'zh-TW'
    and pp.PropertyName in ('Title', 'Description');
```

### Explain
Use a pivot table `ProductProperty` with `PropertyName` to store properties (e.g., Title, Description) as rows, so different products can have different property sets without schema changes.

### Trade-offs
It needs extra joins, which makes queries more complex. `PropertyName` has no IDE intellisense like column names, so you must maintain values carefully; typos will break query results. Strongly-typed ORM navigation is also harder.
