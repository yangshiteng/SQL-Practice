# 1789. Primary Department for Each Employee

![image](https://user-images.githubusercontent.com/60442877/168522316-22f97a53-cfe8-42af-8061-5ba48a0d7d06.png)

![image](https://user-images.githubusercontent.com/60442877/168522332-0d594c26-7d4d-4be2-935d-bafa2f67f6a4.png)

Solution1: (Union)

    (select employee_id, department_id
    from employee
    where primary_flag = "Y")
    union
    (select employee_id, department_id
    from employee
    group by employee_id
    having count(employee_id) = 1)

Solution2: (Window Function) 

    select t.employee_id, t.department_id
    from (select *, count(employee_id) over(partition by employee_id) as C 
    from employee) t
    where t.C = 1 or t.primary_flag = "Y"

Solution3: (coalesce) (case when then else end) (if)

![image](https://user-images.githubusercontent.com/60442877/168523155-555b425e-64c0-4a8f-9bec-c47955ef35e7.png)

    select employee_id, coalesce(max(case when primary_flag = "Y" then department_id else null end), department_id) department_id
    from employee
    group by employee_id
    
    select employee_id,coalesce(max(if(primary_flag = "Y", department_id, null)), department_id) department_id
    from employee
    group by employee_id
