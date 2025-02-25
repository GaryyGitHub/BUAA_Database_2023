# 数据库第七次作业

> #### 22373386 高铭
>
> #### WEEK 9

## 08 - 完整性约束 - 书P173

### 1、什么是数据库的完整性？

数据库的完整性是指**数据的正确性和相容性**。



### 3、什么是数据库的完整性约束条件？

完整性约束条件是数据模型的组成部分，是数据库中的数据应该满足的**语义约束条件**。



### 4、关系数据库管理系统的完整性控制机制应有哪三方面功能？

1. **完整性约束条件定义功能**：定义数据库完整性约束条件的机制
2. **完整性检查功能**：检查用户发出的操作请求是否违背了完整性约束条件
3. **违约处理功能**：如果发现用户的操作请求使数据违背了完整性约束条件，则采取一定的动作来保证数据的完整性



### 5、关系数据库管理系统在实现参照完整性时需要考虑哪些方面？

关系数据库管理系统在实现参照完整性时需要考虑可能破坏参照完整性的各种情况，以及用户违约后的处理策略。

对于外键，参照表与被参照表的操作：

|      被参照表      |       参照表       |         违约处理         |
| :----------------: | :----------------: | :----------------------: |
| 可能破坏参照完整性 |      插入元组      |           拒绝           |
| 可能破坏参照完整性 |     修改外键值     |           拒绝           |
|      删除元组      | 可能破坏参照完整性 | 拒绝/级联删除/设置为空值 |
|     修改主键值     | 可能破坏参照完整性 | 拒绝/级联修改/设置为空值 |



### 6、完成完整性约束条件的定义：主键、参照完整性、职工年龄不超过60岁

> 关系模式：职工（<u>职工号</u>，姓名，年龄，职务，工资，部门号）
>
> 部门（<u>部门号</u>，名称，经理名，电话）

代码如下：

```sql
# 部门表
create table department{
	Dno int primary key,	# 主键
	Dname varchar(255),
	Manager varchar(255),
	Phone char(15)
};

# 职工表
create table employee{
	Eno int primary key,	# 主键
	Ename varchar(255),
	Age int,
	Job varchar(255),
	Salary int,
	Dno int,
	constraint check_age check (Age <= 60),		# 年龄不超过60岁
	constraint fk_Dno foreign key(Dno) 			# 参照完整性
		references department(Dno)
};
```





### 7、在关系系统中，当操作违反实体完整性、参照完整性和用户定义的完整性约束条件时，一般如何分别进行处理？

- 操作违反实体完整性：拒绝执行
- 操作违反参照完整性：默认采用拒绝执行的方式，也可以选择级联操作（CASCADE）或设置为控制
- 操作违反用户定义的完整性约束条件：拒绝执行，有时要根据应用语义执行一些附加操作



### 8、联谊会，Male - 男宾信息，Female - 女宾信息。在Male和Female表上各建立一个触发器，将来宾人数限制在50人以内。提交触发器代码。

```sql
# male表
create trigger limit_male
    after insert on male
    for each row
    begin
        declare i int;
        select count(*) into i from male;
        select count(*) into i from female;
        if i>50 then
            delete from male where mid=NEW.mid;
        end if;
    end;
    
# female表
create trigger limit_female
    after insert on female
    for each row
    begin
        declare i int;
        select count(*) into i from male;
        select count(*) into i from female;
        if i>50 then
            delete from female where fmid=NEW.fmid;
        end if;
    end;
```





## 09 - 安全 - 书P154

### 1、什么是数据库的安全性？

数据库的安全性是指保护数据库以防止不合法的使用所造成的数据泄露、更改或破坏。



### 6、使用`GRANT`语句完成授权功能

> 学生（学号，姓名，年龄，性别，家庭住址，班级号）
>
> `Student(Sno, Sname, Age, Sex, Addr, Cno)`
>
> 班级（班级号，班级名，班主任，班长）
>
> `Class(Cno, Cname, Teacher, Leader)`

#### （1）授予用户U1拥有对两个表的所有权限，并可给其他用户授权

```sql
grant all privileges on table Student, Class to U1 with grant option
```



#### （2）授予用户U2对学生表具有查看权限，对家庭住址具有更新权限

```sql
grant select, update(Addr) on table Student to U2
```



#### （3）将对班级表查看权限授予所有用户

```sql
grant select on table class to public
```



#### （4）将对学生表的查询、更新权限授予角色R1

```sql
grant select, update on table student to R1
```



#### （5）将角色R1授予用户U1，并且U1可继续授权给其他角色

```sql
grant R1 to U1 with admin option
```

