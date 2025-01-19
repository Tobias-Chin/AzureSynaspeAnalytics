# SQL Code and Output

## Query to find the top 10 most common safety incidents happening in Boston:

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


## sample of results:
![Image](https://github.com/user-attachments/assets/9b1fc799-bb65-4b49-a5fb-261796bb691e)


## graph of results:
![Image](https://github.com/user-attachments/assets/6043595b-5684-4351-aba8-d81f16336256)


As we can see it is clear parking enforcment and street cleaning remain the top issues surrounding Boston Safety.


## To go further in detail I wanted to find the top 3 subcategory incidents occuring per categroy:



```sql
With subrank as (
SELECT category,subcategory,count(subcategory)as incidents, dense_rank()over(partition by category order by count(subcategory)desc)as safetyrank
FROM
    OPENROWSET(
        BULK     'https://azureopendatastorage.blob.core.windows.net/citydatacontainer/Safety/Release/city=Boston/*.parquet',
        FORMAT = 'parquet'
    ) AS [result]
    group by category,subcategory
    )
select category,subcategory, incidents, safetyrank from subrank where safetyrank<=3
```


## sample of results:![Image](https://github.com/user-attachments/assets/5209c9a2-9e2a-4ea9-9e95-8542f2475978)



## graph of results:
![Image](https://github.com/user-attachments/assets/1b39f112-aac8-44d8-9918-7835cbc44cf3)
