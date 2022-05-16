# 1789. Primary Department for Each Employee

![image](https://user-images.githubusercontent.com/60442877/168522316-22f97a53-cfe8-42af-8061-5ba48a0d7d06.png)

![image](https://user-images.githubusercontent.com/60442877/168522332-0d594c26-7d4d-4be2-935d-bafa2f67f6a4.png)

    (select employee_id, department_id
    from employee
    where primary_flag = "Y")
    union
    (select employee_id, department_id
    from employee
    group by employee_id
    having count(employee_id) = 1)
