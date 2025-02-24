## QUESTION
Amber's conglomerate corporation just acquired some new companies. Each of the companies follows this hierarchy: 

<img width="112" alt="image" src="https://github.com/user-attachments/assets/a0adbf3e-f2cd-4dc6-80b8-ad8bf6334ee8" />

Given the table schemas below, write a query to print the company_code, founder name, total number of lead managers, total 
number of senior managers, total number of managers, and total number of employees. Order your output by ascending company_code.

Note:

The tables may contain duplicate records.
The company_code is string, so the sorting should not be numeric. For example, if the company_codes are C_1, C_2, and C_10, then the ascending company_codes will be C_1, C_10, and C_2.
Input Format

The following tables contain company data:

- Company: The company_code is the code of the company and founder is the founder of the company.

  <img width="173" alt="image" src="https://github.com/user-attachments/assets/1fa76b8a-b66a-4b79-afcd-e6df75b29a3b" />  <img width="202" alt="image" src="https://github.com/user-attachments/assets/1a984eff-9b39-423a-b0ed-db612dbaa2a5" />


- Lead_Manager: The lead_manager_code is the code of the lead manager, and the company_code is the code of the working company.

  <img width="200" alt="image" src="https://github.com/user-attachments/assets/31220e2f-c799-4eb1-ba12-3ab2d2873eef" />  <img width="249" alt="image" src="https://github.com/user-attachments/assets/fd23fcfd-3cd9-43db-8e7b-7951b3688de1" />


- Senior_Manager: The senior_manager_code is the code of the senior manager, the lead_manager_code is the code of its lead 
  manager, and the company_code is the code of the working company.

  <img width="209" alt="image" src="https://github.com/user-attachments/assets/1e954ae7-3a10-4559-9f26-616ebc6d0b64" />  <img width="385" alt="image" src="https://github.com/user-attachments/assets/689d695a-5f6a-45bd-a0c8-d16d4695275c" />


- Manager: The manager_code is the code of the manager, the senior_manager_code is the code of its senior manager, the
  lead_manager_code is the code of its lead manager, and the company_code is the code of the working company.

  <img width="209" alt="image" src="https://github.com/user-attachments/assets/d3c5999d-b94e-40c8-b383-9c05744f5463" />  <img width="482" alt="image" src="https://github.com/user-attachments/assets/ee7ca2e2-24c5-49d0-9176-bd59559a5320" />


- Employee: The employee_code is the code of the employee, the manager_code is the code of its manager, the senior_manager_code
  is the code of its senior manager, the lead_manager_code is the code of its lead manager, and the company_code is the code of
  the working company.

  <img width="209" alt="image" src="https://github.com/user-attachments/assets/5f833a5e-0b55-4d97-92fa-bca88d7ba3fe" />  <img width="580" alt="image" src="https://github.com/user-attachments/assets/9a5b7637-b113-4d96-8b9d-aac6a69e83f7" />


<img width="149" alt="image" src="https://github.com/user-attachments/assets/05f71150-2c73-495f-8582-8090308a6bbb" />


**Explanation :**

In company C1, the only lead manager is LM1. There are two senior managers, SM1 and SM2, under LM1. There is one manager,
M1, under senior manager SM1. There are two employees, E1 and E2, under manager M1.

In company C2, the only lead manager is LM2. There is one senior manager, SM3, under LM2. There are two managers, M2 and M3, 
under senior manager SM3. There is one employee, E3, under manager M2, and another employee, E4, under manager, M3.


## SOLUTION :
```SQL
SELECT 
    C.COMPANY_CODE,
    C.FOUNDER,
    COUNT(DISTINCT E.LEAD_MANAGER_CODE),
    COUNT(DISTINCT E.SENIOR_MANAGER_CODE),
    COUNT(DISTINCT E.MANAGER_CODE),
    COUNT(DISTINCT E.EMPLOYEE_CODE)
FROM COMPANY C
JOIN EMPLOYEE E
ON C.COMPANY_CODE = E.COMPANY_CODE
GROUP BY
    C.COMPANY_CODE,
    C.FOUNDER
ORDER BY 
    C.COMPANY_CODE ASC;
```
<img width="365" alt="image" src="https://github.com/user-attachments/assets/71254fcf-e082-4d4a-9bd8-c03cfc11c665" />



