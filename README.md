# MyLab.DBLocalization

## Purpose
Minimize table and column size for localization in database.


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