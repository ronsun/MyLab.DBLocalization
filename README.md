# MyLab.DBLocalization

## Purpose
Decrease table and column size and complexity for localization in database.

> This design involves trade-offs, including a higher level of abstraction, which may complicate ORM navigation properties and implicit joins.

## Query localization
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

## Explain
Use a `Localization.LocalizationMapId` instead a separate localization mapping table.