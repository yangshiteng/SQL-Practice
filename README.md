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


# 1795. Rearrange Products Table

![image](https://user-images.githubusercontent.com/60442877/169728687-cd93a763-51a1-49f4-80c4-d9a664f201fc.png)

![image](https://user-images.githubusercontent.com/60442877/169728697-5d52d4b6-cb75-4647-9d0d-136404072a6d.png)

Solution 1: (UNION) 

    (select product_id, "store1" as store, store1 as price
    from Products
    where store1 is not null)
    union
    (select product_id, "store2" as store, store2 as price
    from Products
    where store2 is not null)
    union
    (select product_id, "store3" as store, store3 as price
    from Products
    where store3 is not null)

Solution 2: (UNPIVOT) (MS SQL)

    select product_id,store,price 
    from Products
    UNPIVOT
    (
    price
    for store in (store1,store2,store3)
    ) as T


# 1421. NPV Queries

![image](https://user-images.githubusercontent.com/60442877/169732053-a6f44b42-3e07-494c-a547-29f4c1f97bc3.png)

![image](https://user-images.githubusercontent.com/60442877/169732071-e25d68a2-5e92-47cd-ab40-5c99ef5ff47e.png)

![image](https://user-images.githubusercontent.com/60442877/169732080-5e4cecb1-0ac2-4689-9097-5f1f77a33e12.png)

Solution 1: (left join) (ifnull)

    select Q.id as id, Q.year as year, ifnull(N.npv,0) as npv
    from Queries as Q left join NPV as N on (Q.id = N.id and Q.year = N.year) 

Solution 2: (left join) (coalesce)

    select Q.id as id, Q.year as year, coalesce(N.npv,0) as npv
    from Queries as Q left join NPV as N on (Q.id = N.id and Q.year = N.year) 
    
    
    
# 1809. Ad-Free Sessions

![image](https://user-images.githubusercontent.com/60442877/171065459-ede93e96-2dd7-4b3a-8539-c10700ab65ef.png)

![image](https://user-images.githubusercontent.com/60442877/171065472-a89b4ab6-9c2a-445b-89f5-3fd77e278598.png)

    SELECT distinct session_id
    FROM playback pb LEFT JOIN ads ad
    ON pb.customer_id = ad.customer_id
    AND ad.timestamp BETWEEN start_time and end_time
    WHERE ad.customer_id IS NULL




