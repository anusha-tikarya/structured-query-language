https://chatgpt.com/share/66ecb221-bad8-8003-b902-24d91f6bf560
```sql
with greater_avgSal as (
	select  employeeid ,EmployeeName, Department, Salary from emplo
	where salary > (select avg(salary) from Emplo))

select* from greater_avgSal;
```
https://chatgpt.com/share/66ef5855-0e44-8003-83ca-a3e1c882dcc1
