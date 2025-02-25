# 数据库第五次作业

#### 22373386 高铭

#### WEEK 5 & 6

## 书P130

### 5. 完成查询操作

#### （1）找出所有供应商的姓名和所在城市

```sql
SELECT SNAME, CITY
FROM S;
```



#### （2）找出所有零件的名称、颜色、重量

```sql
SELECT PNAME, COLOR, WEIGHT
FROM P;
```



#### （3）找出使用供应商S1所供应零件的工程号码

```sql
SELECT JNO
FROM SPJ
WHERE SNO = 'S1';
```



#### （4）找出工程项目J2使用的各种零件的名称及其数量

```sql
SELECT P.PNAME, SPJ.QTY
FROM P, SPJ
WHERE P.PNO = SPJ.PNO AND SPJ.JNO = 'J2';
```



#### （5）找出上海厂商供应的所有零件号码

```sql
# 写法1
SELECT DISTINCT PNO
FROM SPJ
WHERE SNO IN
	(SELECT SNO
     FROM S
     WHERE CITY = '上海');
```

```sql
# 写法2
SELECT DISTINCT SPJ.PNO
FROM S, SPJ
WHERE S.SNO = SPJ.SNO AND S.CITY = '上海';
```



#### （6）找出使用上海产的零件的工程名称

```sql
SELECT JNAME
FROM S, J, SPJ
WHERE S.CITY = '上海' AND S.SNO = SPJ.SNO AND SPJ.JNO = J.JNO;
```



#### （7）找出没有使用天津产的零件的工程号码

```sql
SELECT JNO
FROM J
WHERE NOT EXISTS
	(SELECT *
     FROM SPJ, S
     WHERE S.CITY = '天津' AND S.SNO = SPJ.SNO AND SPJ.JNO = J.JNO);
```



## WEEK 5 - SQL 测验题

### 一

现有关系模式如下： 

学生（<u>学号</u>，姓名，性别，年龄）；课程（<u>课程号</u>，课程名，教师姓名）；选课表（课程号，学号，成绩）

`S(SID, SNAME, SEX, AGE); C(CID, CNAME, TEACHER); SC(CID, SID, SCORE)`

#### 1. 检索年龄大于20岁的男生的学号和姓名。

```SQL
SELECT SID,SNAME
FROM S
WHERE AGE>20 AND SEX='MALE';
```



#### 2. 检索选修了姓刘的老师所教授的课程的女学生的姓名。

```SQL
SELECT SNAME
FROM S,C,SC
WHERE C.TEACHER LIKE '刘%' AND C.CID=SC.CID AND SC.SID=S.SID AND S.SEX='FEMALE';
```



#### 3. 检索李想同学不学的课程的课程号和课程名。

```SQL
SELECT CID,CNAME
FROM C
WHERE NOT EXISTS
	(SELECT *
	 FROM S,SC
	 WHERE S.SNAME='李想' AND S.SID=SC.SID AND SC.CID=C.CID);
```



#### 4. 检索至少选修了两门课程的学生的学号。

```SQL
SELECT SID
FROM SC
GROUP BY SID
HAVING COUNT(*)>=2;
```



#### 5. 求刘老师所教授课程的每门课的平均成绩。

```SQL
SELECT AVG(SC.SCORE)
FROM C,SC
WHERE C.TEACHER LIKE '刘%' AND C.CID=SC.CID
GROUP BY C.CID;
```



#### 6. 假设不存在重修的情况，请统计每门课的选修人数(选课人数超过两人的课程才统计)。要求显示课程号和人数，查询结果按人数降序排列，若人数相同，按课程号升序排列。

```SQL
SELECT CID,COUNT(SID)
FROM SC
GROUP BY CID
HAVING COUNT(SID)>2
ORDER BY COUNT(SID) DESC, CID;
```



#### 7. 求年龄大于所有女生年龄的男生的姓名和年龄。

```SQL
SELECT SNAME,AGE
FROM S
WHERE SEX='MALE' AND AGE>ALL
	(SELECT AGE
  	 FROM S
	 WHERE SEX='FEMALE');
```



#### 8. 假定不存在重修的情况，求选修了所有课程的学生的学号姓名。(可以不用相关子查询做)

```sql
SELECT SID,SNAME
FROM S
WHERE NOT EXISTS
	(SELECT *
	FROM C
	WHERE NOT EXISTS
		(SELECT *
		 FROM SC
		 WHERE SC.SID=S.SID AND SC.CID=C.CID));
```



#### 9. 查询重修次数在2次以上的学生学号，课程号，重修次数

```sql
SELECT SID,CID,COUNT(*)
FROM SC
GROUP BY SID,CID
HAVING COUNT(*)>2;
```



#### 10. 查询重修学生人数最多的课程号，课程名，教师姓名

```sql
SELECT C.CID,C.CNAME,C.TEACHER
FROM C,SC
WHERE C.CID=SC.CID
GROUP BY SC.SID,SC.CID
HAVING COUNT(*) >= ALL
	(SELECT COUNT(*)
 	 FROM SC
	 GROUP BY SC.SID,SC.CID);
```



### 二

关系模式：学生（学号，姓名，年龄，性别，班级）

课程（课程号，课程名，先修课程号，学分）注意：此表的主键是(课程号)

选课（学号，课程号，教师号，成绩）

教师（教师号，教师名称）

```sql
S(SID, SNAME, AGE, SEX, CLASS)
C(CID, CNAME, CPID, CREDIT)
SC(SID, CID, TID, SCORE)
T(TID, TNAME)
```

#### 1. 查找李力的所有不及格的课程名称和成绩，按成绩降序排列

```SQL
SELECT C.CNAME,SC.SCORE
FROM S,C,SC
WHERE S.SMAME='李力' AND S.SID=SC.SID AND C.CID=SC.CID AND SC.SCORE<60
ORDER BY SC.SCORE DESC;
```



#### 2. 列出每门课的学分，选修的学生人数，及学生成绩的平均分

```sql
SELECT CREDIT,CNT,AVGSCORE
FROM C
JOIN
	(SELECT CID,COUNT(SID) AS CNT,AVG(SCORE) AS AVGSCORE
     FROM SC
     GROUP BY SC.CID) AS NEW
ON NEW.CID=C.CID;
```



#### 3. 选出所修课程总学分在10分以下的学生（注：不及格的课程没有学分）。

```sql
SELECT S.*
FROM SC,C,S
WHERE C.CID=SC.CID AND SC.SCORE>=60 AND SC.SID=S.SID
GROUP BY SC.SID
HAVING SUM(C.CREDIT)<10;
```



#### 4. 选出选课门数最多的学生学号及选课数量

```sql
SELECT SID,COUNT(CID)
FROM SC
GROUP BY SID
HAVING COUNT(CID)>ALL
	(SELECT COUNT(CID)
     FROM SC
     GROUP BY SID);
```



#### 5. 列出每门课的最高分及获得该分数的学生

```sql
SELECT CID,SCORE,S.*
FROM SC,S
WHERE SC.SID=S.SID AND SCORE=
	(SELECT MAX(SCORE)
     FROM SC AS SC2
     WHERE SC.CID=SC2.CID
     GROUP BY CID);
```



#### 6. 选出物理课得分比所有男学生的物理课平均分高的学生姓名

```sql
SELECT S.SNAME
FROM S,SC,C
WHERE S.SID=SC.SID AND C.CID=SC.CID AND C.CNAME='物理' AND SCORE >
	(SELECT AVG(SCORE)
     FROM S,SC,C
     WHERE S.SID=SC.SID AND C.CID=SC.CID AND C.CNAME='物理' AND S.SEX='MALE');
```



#### 7. 选出修习过物理课的直接先修课的学生

```sql
SELECT S.*
FROM SC,C
WHERE SC.CID=C.CPID AND S.SID=SC.SID AND C.CPID=
	(SELECT CID
	 FROM C
	 WHERE C.CNAME='物理');
```



#### 8. 选出有两门以上先修课的课程（包括直接先修课、间接先修课）(用课程表)

```sql
SELECT *
FROM C
WHERE EXISTS
	(SELECT *
     FROM C C1, C C2
     WHERE C.CPID=C1.CID AND C1.CPID=C2.CID);
```



## WEEK 6 - SQL练习

```sql
S(SID, SNAME, AGE, SEX, CLASS)
C(CID, CNAME, CPID, CREDIT)
SC(SID, CID, TID, SCORE, TIME)
T(TID, TNAME)
```

### 关系代数

#### 1. 查找选修了物理课的学生姓名

$$
\Pi_\text{SNAME}(\sigma_\text{CNAME='物理'}(C\bowtie S))
$$



#### 2. 查找教的学生的成绩都大于60分的教师（给出教师号即可）

$$
\Pi_{\text{TID}}(T)-\Pi_{\text{TID}}(\sigma_{\text{SCORE}\le 60}(T\bowtie SC\bowtie S))
$$



#### 3. 查找没有选修张三老师教的所有课的学生

$$
\Pi_{\text{SID}}(S)-\Pi_{\text{SID}}(\sigma_\text{TNAME='张三'}(T\bowtie SC))
$$



### SQL

#### 1. 创建一个表，查询每个学生选修的课程数量，将结果插入表中

```sql
CREATE TABLE NEWTAB
	(SID CHAR(20) NOT NULL PRIMARY KEY,
     SNAME CHAR(20) NOT NULL,
     CNUM INT);
INSERT
	INTO NEWTAB(SID,SNAME,CNUM)
		SELECT S.SID,S.SNAME,COUNT(SC.CID)
		FROM S
		WHERE S.SID=
			(SELECT SC.SID
             FROM SC
             GROUP BY SC.SID
            );
```



#### 2. 找出所有姓诸的学生姓名（排除姓‘诸葛’的学生）

```sql
SELECT SNAME
FROM S
WHERE SNAME LIKE '诸%' AND SNAME NOT LIKE '诸葛%';
```



#### 3. 检索至少得过一次课程最高分的学生学号姓名（不考虑重修的情况）

```SQL
SELECT DISTINCT SID,SNAME
FROM S
WHERE SID =
	(SELECT SID
     FROM SC SC1
     WHERE SC1.SCORE =
    	(SELECT MAX(SC2.SCORE)
         FROM SC SC2
         GROUP BY SC2.CID));
```



#### 4. 查询如下内容（学生ID，课程ID，时间），列出**每个**学生第一次选某课程的时间（即非重修的选课时间）

```sql
SELECT SC.SID, SC.CID, MIN(SC.TIME)
FROM SC
GROUP BY SC.SID, SC.CID;
```

改正后：

```sql
select sid, cid, sc.time
from s
left join sc on s.sid = sc.sid
where sc.time >= (
	select max(sc1.time)
    from sc sc1
    where sc.sid = sc1.sid and sc.cid = sc1.cid
    group by sc1.sid, sc1.cid
)
```



#### 5. 将学生的重修课程成绩都改成60分

```SQL
# 理解1：首修仍是原来分数，重修那次的选课改成60分
UPDATE SC
SET SCORE = 60
WHERE (SID, CID, TIME) IN (
    SELECT SID, CID, MAXTIME
    FROM(
    	SELECT SID, CID, MAX(TIME) AS MAXTIME, COUNT(*)
    	FROM SC
    	GROUP BY SID,CID
    	HAVING COUNT(*) > 1
    )
);
```

```sql
# 理解2：对于有过重修的课，成绩记录都改成60分
UPDATE SC
SET SCORE = 60
WHERE (SID, CID) IN (
    SELECT SID, CID
    FROM (
        SELECT SID, CID
        FROM SC
        GROUP BY SID, CID
        HAVING COUNT(*) > 1
    )
);
```



#### 6.查找每个学生当前可选修的课程列表（即该学生没有选该课程，且该学生已经修完了该课程的先修课）

```sql
SELECT S.SID, C.CID, C.CNAME
FROM S
CROSS JOIN C				# 创建所有学生和课程的组合
LEFT JOIN SC ON S.SID=SC.SID AND C.CID=SC.CID
WHERE SC.CID IS NULL		# 该学生没有选该课程
AND EXISTS (
	SELECT *
    FROM C AS C2
    JOIN SC AS SC2 ON C2.CID=SC2.CID
    WHERE C2.CPID=C.CID AND SC2.SID=S.SID
);							# 该学生已经修完了该课程的先修课
```

