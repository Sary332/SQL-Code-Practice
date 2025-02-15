##  QUESTION :
Pivot the Occupation column in OCCUPATIONS so that each Name is sorted alphabetically and displayed underneath its corresponding
Occupation. The output should consist of four columns (Doctor, Professor, Singer, and Actor) in that specific order, with their 
respective names listed alphabetically under each column.

Note: Print NULL when there are no more names corresponding to an occupation.

**Input Format**

The OCCUPATIONS table is described as follows:

<img width="313" alt="image" src="https://github.com/user-attachments/assets/9eb65725-4e36-4902-b325-7db3bc1d685a" />

Occupation will only contain one of the following values: Doctor, Professor, Singer or Actor.

**Sample Input**

<img width="227" alt="image" src="https://github.com/user-attachments/assets/2f34304a-78c3-4685-bde2-05435ecc5198" />

<img width="258" alt="image" src="https://github.com/user-attachments/assets/6f8a7f82-60bb-4b52-85e4-fd37078b0ff4" />

## SOLUTION 
```SQL
WITH PIVOT_OCCUPATIONS AS (
SELECT NAME,
       OCCUPATION,
       ROW_NUMBER () OVER (PARTITION BY OCCUPATION ORDER BY NAME) AS RN
FROM OCCUPATIONS
)

SELECT MAX(CASE WHEN OCCUPATION = 'DOCTOR' THEN NAME  END) AS DOCTOR,
       MAX(CASE WHEN OCCUPATION = 'PROFESSOR' THEN NAME END) AS PROFESSOR,
       MAX(CASE WHEN OCCUPATION = 'SINGER' THEN NAME END) AS SINGER,
       MAX(CASE WHEN OCCUPATION = 'ACTOR' THEN NAME END) AS ACTOR
FROM PIVOT_OCCUPATIONS
GROUP BY RN
ORDER BY RN
```
<img width="404" alt="image" src="https://github.com/user-attachments/assets/feffa282-cd4e-4872-aa14-9f23fdcbd0bb" />


## NOTES:
Your current query attempts to pivot the `Occupation` column using `CASE` statements, but it has a few issues:

1. **Incorrect Sorting**: The `ORDER BY` clause is applied to the entire result set, but it doesn't properly sort the names under each occupation.
2. **No Row Numbering**: Without assigning row numbers to each occupation, the names won't align correctly in the pivoted format.
3. **NULL Handling**: The `CASE` statements in the `SELECT` clause don't handle cases where there are no more names for an occupation.

To achieve the desired output, you need to:
1. Assign a row number to each name within its occupation.
2. Use these row numbers to align the names under their respective occupations.
3. Pivot the data using conditional aggregation.

---

### **Explanation**

1. **Common Table Expression (CTE)**:
   - The `OccupationRowNumbers` CTE assigns a row number (`RowNum`) to each name within its occupation, sorted alphabetically.
   - Example:
     | Name       | Occupation | RowNum |
     |------------|------------|--------|
     | Samantha   | Actor      | 1      |
     | Julia      | Actor      | 2      |
     | Maria      | Professor  | 1      |
     | Ashley     | Professor  | 2      |

2. **Pivoting with Conditional Aggregation**:
   - The `SELECT` statement uses `MAX(CASE ...)` to pivot the data.
   - For each `RowNum`, it selects the name corresponding to each occupation.
   - Example:
     - For `RowNum = 1`, it selects the first `Doctor`, first `Professor`, first `Singer`, and first `Actor`.
     - For `RowNum = 2`, it selects the second `Doctor`, second `Professor`, etc.

3. **Grouping and Ordering**:
   - The `GROUP BY RowNum` ensures that each row in the result corresponds to a specific `RowNum`.
   - The `ORDER BY RowNum` ensures that the rows are displayed in the correct order.

4. **NULL Handling**:
   - If there are no more names for a specific occupation at a given `RowNum`, the `MAX(CASE ...)` will return `NULL`.

---

### **Example Input and Output**

#### Input Table: `OCCUPATIONS`
| Name      | Occupation |
|-----------|------------|
| Ashley    | Professor  |
| Samantha  | Actor      |
| Julia     | Doctor     |
| Britney   | Professor  |
| Maria     | Professor  |
| Meera     | Professor  |
| Priya     | Doctor     |
| Priyanka  | Professor  |
| Jennifer  | Actor      |
| Ketty     | Professor  |
| Belvet    | Professor  |
| Naomi     | Professor  |
| Jane      | Singer     |
| Jenny     | Singer     |
| Kristeen  | Singer     |
| Christeen | Singer     |
| Eve       | Actor      |
| Aamina    | Doctor     |

#### Output:
| Doctor  | Professor | Singer   | Actor     |
|---------|-----------|----------|-----------|
| Julia   | Ashley    | Jane     | Samantha  |
| Priya   | Britney   | Jenny    | Jennifer  |
| Aamina  | Maria     | Kristeen | Eve       |
| NULL    | Meera     | Christeen| NULL      |
| NULL    | Priyanka  | NULL     | NULL      |
| NULL    | Ketty     | NULL     | NULL      |
| NULL    | Belvet    | NULL     | NULL      |
| NULL    | Naomi     | NULL     | NULL      |

---

### **Key Points**
1. **Row Numbering**: The `ROW_NUMBER()` function ensures that names are assigned a unique number within their occupation, which is crucial for aligning them in the pivoted format.
2. **Pivoting**: The `MAX(CASE ...)` technique is used to pivot the data into separate columns for each occupation.
3. **NULL Handling**: If there are no more names for an occupation at a specific `RowNum`, the query automatically returns `NULL`.




