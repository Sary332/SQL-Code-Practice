## Question :

Assume you're given two tables containing data about Facebook Pages and their respective likes (as in "Like a Facebook Page").
Write a query to return the IDs of the Facebook pages that have zero likes. The output should be sorted in ascending order based on
the page IDs.

<img width="157" alt="image" src="https://github.com/Sary332/SQL-Code-Practice/assets/110008177/d63e3733-4a76-487a-af7d-64f73284f035">
<img width="191" alt="image" src="https://github.com/Sary332/SQL-Code-Practice/assets/110008177/01926144-bb11-4ec9-ad55-ff87407b92b0">

## Solution :
```sql
SELECT    p.page_id
FROM      pages p
LEFT JOIN page_likes pl
ON        p.page_id = pl.page_id
WHERE     pl.liked_date is NULL
ORDER BY  1 ASC
```
<img width="386" alt="image" src="https://github.com/Sary332/SQL-Code-Practice/assets/110008177/6b6ad24a-5ad4-492b-b5d8-7905af8390d6">

