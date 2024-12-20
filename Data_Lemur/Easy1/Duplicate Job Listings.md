## Question :
Assume you're given a table containing job postings from various companies on the LinkedIn platform. Write a query to retrieve 
the count of companies that have posted duplicate job listings.

**Definition:**
Duplicate job listings are defined as two job listings within the same company that share identical titles and descriptions.

<img width="118" alt="image" src="https://github.com/Sary332/SQL-Code-Practice/assets/110008177/c2e8f642-2d68-476e-a938-27f083d3b44f">
<img width="427" alt="image" src="https://github.com/Sary332/SQL-Code-Practice/assets/110008177/19610bf3-7a36-4697-8660-2d64ae5a8cab">

## Solution :
```sql
SELECT  COUNT(x.company_id) AS duplicate_companies
FROM  (
        SELECT company_id
              ,COUNT(company_id) AS hit
        FROM job_listings
        GROUP BY  company_id
     ) x
WHERE x.hit > 1 ;
```
<img width="139" alt="image" src="https://github.com/Sary332/SQL-Code-Practice/assets/110008177/5e2d6d59-bc4c-441a-b472-73e2aeba3174">
