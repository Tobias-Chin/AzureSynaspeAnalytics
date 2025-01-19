# SQL Code and Output

## SQL Query

Here is the SQL query I used:

```sql
With subrank as (
SELECT subcategory,count(subcategory)as incidents, dense_rank()over(order by count(subcategory)desc)as safetyrank
FROM
    OPENROWSET(
        BULK     'https://azureopendatastorage.blob.core.windows.net/citydatacontainer/Safety/Release/city=Boston/*.parquet',
        FORMAT = 'parquet'
    ) AS [result]
    group by subcategory
    )
select subcategory, incidents, safetyrank from subrank where safetyrank<=10;
```
![Image](https://github.com/user-attachments/assets/9b1fc799-bb65-4b49-a5fb-261796bb691e)


