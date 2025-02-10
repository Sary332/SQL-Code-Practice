## QUESTION :
You are given a table, BST, containing two columns: N and P, where N represents the value of a node in Binary Tree, 
and P is the parent of N.

<img width="283" alt="image" src="https://github.com/user-attachments/assets/aafbc6e9-33fc-49a9-bb57-313e150554c0" />

Write a query to find the node type of Binary Tree ordered by the value of the node. Output one of the following for each node:

Root: If node is root node.
Leaf: If node is leaf node.
Inner: If node is neither root nor leaf node.

## SOLUTION
``SQL
SELECT N, 
       CASE WHEN P IS NULL THEN 'Root'
            WHEN N IN (SELECT  P FROM BST WHERE P IS NOT NULL) THEN 'Inner'
            ELSE 'Leaf'
       END AS BINARY_TREE
FROM  BST
ORDER BY N
```

