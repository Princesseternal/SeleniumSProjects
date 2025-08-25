ROW_NUMBER

Assigns a unique sequential number to each row starting from 1.
No duplicates — even if rows have the same values, they get different row numbers.

Example:
Values: [100, 100, 200] → ROW_NUMBER: [1, 2, 3]

RANK

Assigns the same rank to rows with duplicate values.
Skips the next ranks after duplicates.

Example:
Values: [100, 100, 200] → RANK: [1, 1, 3]
(Rank 2 is skipped)

DENSE_RANK

Does not skip any ranks after duplicates.
Example:
Values: [100, 100, 200] → DENSE_RANK: [1, 1, 2]

Before assigning ROW_NUMBER, RANK, or DENSE_RANK, SQL sorts the data based on the ORDER BY clause specified in the OVER () function.
The ranking is then applied on the sorted result.

SQL QUERY:

SELECT *,
ROW_NUMBER() OVER (PARTITION BY DeptId ORDER BY Salary) AS RowNumber,
RANK() OVER (PARTITION BY DeptId ORDER BY Salary) AS [Rank],
DENSE_RANK() OVER (PARTITION BY DeptId ORDER BY Salary) AS [DenseRank] FROM Table

Query selects all columns from Table and adds three ranking columns:

RowNumber
Rank
DenseRank
All of them are calculated within each department (PARTITION BY DeptId) and ordered by salary in ascending order.

PARTITION BY DeptId

This divides the data into groups based on DeptId (department ID).The ranking functions are applied separately within each group (i.e., for each department).
ORDER BY Salary

Within each department, rows are sorted by salary (lowest to highest).Ranking is assigned based on this ordering.
Lets see another example:

<img width="1286" height="732" alt="image" src="https://github.com/user-attachments/assets/9d94233e-0586-4ba5-91e3-139801d339ce" />


Here ROW_NUMBER, RANK, and DENSE_RANK are calculated by partitioned by DeptID and ordered by Salary

DeptID = 1

No duplicate salaries.
All three functions assign consecutive numbers: 1, 2, 3.
DeptID = 2

20000 appears twice → both get Rank = 1
Rank skips 2 → jumps to 3 for 50000
Dense_Rank continues without skipping → goes to 2
DeptID = 3

No duplicate salaries → all functions increment by 1

DeptID = 4

10000 appears twice → both get Rank = 1
Rank skips 2 → next is 3 and then 4
Dense_Rank continues with 2, 3, 4 (no skipping)
Summary:

Use ROW_NUMBER() when you need unique row IDs.
Use RANK() when you want to reflect actual positions (with gaps for ties).
Use DENSE_RANK() when you want to rank tied rows but avoid gaps.
