# **数据库第二次上机**

> #### 22373386 高铭

## TASK 1 约束

### Q1 

```sql
create table Department(
    D_id char(30) NOT NULL PRIMARY KEY,
    D_name varchar(255) NOT NULL,
    D_floor smallint
);

create table Staff(
    S_id char(30) NOT NULL PRIMARY KEY,
    S_name varchar(255) NOT NULL,
    S_salary int,
    S_absence int default 0,
    S_birth date,
    S_marriage bool,
    S_Did char(30) NOT NULL,
    constraint fk_Did foreign key (S_Did)
        references Department(D_id),
    check (S_salary >= 2000 and S_name LIKE '^[A-Z][a-z]*([- ]?[A-Z][a-z]*)*$')
);
```

### Q2

#### Department:

<img src="./img/Q2-1.png" alt="1" style="zoom:67%;" />

#### Stuff:

<img src="./img/Q2-2.PNG" alt="2" style="zoom:80%;" />

#### 报错1：

<img src="./img/Q2-3.png" alt="3" style="zoom:67%;" />

触发报错，因为插入数据S_salary为1980不合规，而要求该值不小于2000。

#### 报错2：

![4](./img/Q2-4.png)

触发报错，因为插入数据S_Did为0009不合规。S_Did是外键，要求和Stuff的主键D_id保持对应。但是0009并不在表Stuff中的D_id的域中，故触发报错。

#### 删去还有员工的部门

![5](./img/Q2-5.png)

报错，因为想要删去的0001部门还与stuff中的数据存在外键约束，无法删除。

## TASK 2

### Q3 查询员工表的员工薪资、缺勤天数两列信息

<img src="./img/Q3.PNG" alt="Q3" style="zoom:50%;" />

### Q4 统计员工人数 

<img src="./img/Q4.PNG" alt="Q4" style="zoom:60%;" />

### Q5 查询全体员工的平均薪资

<img src="./img/Q5.PNG" alt="Q3" style="zoom: 67%;" />

### Q6 某个部门的最高工资、最低工资。我们指定“某个”部门为0002

<img src="./img/Q6.PNG" alt="Q6" style="zoom:67%;" />

### Q7 统计缺勤天数超过3天的所有员工名

<img src="./img/Q7.PNG" alt="image-20240328193508473" style="zoom:67%;" />

### Q8 查询平均工资最高的部门的名称和所在的楼层

<img src="./img/Q8.PNG" alt="Q8" style="zoom:58%;" />

### Q9 查询所有员工名，英文名称均大写表示

<img src="./img/Q9.PNG" alt="Q9" style="zoom:60%;" />

### Q10 将**Date**格式的出生日期列以”year/month/day”和“yearmonthday”格式展示

<img src="./img/Q10.PNG" alt="Q10" style="zoom:60%;" />

### Q11 选择某个员工，将其的姓和名分别单独查询出来

<img src="./img/Q11.PNG" alt="Q11" style="zoom:67%;" />

### Q12 选择两个员工，查询出他俩生日差几天

<img src="./img/Q12.PNG" alt="Q12" style="zoom: 67%;" />

### Q13 查询生日比2000年1月1日晚的员工

<img src="./img/Q13.PNG" alt="Q13" style="zoom:67%;" />