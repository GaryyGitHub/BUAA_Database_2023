# 数据库第四次作业

#### 22373386 高铭

## 书P70

### 6. 用关系代数完成查询

（1）求供应工程J1零件的供应商号码SNO
$$
\Pi_{SNO}(\sigma_{JNO='J1'}(SPJ))
$$
（2）求供应工程J1零件P1的供应商号码SNO
$$
\Pi_{SNO}(\sigma_{JNO='J1'\and PNO='P1'}(SPJ))
$$
（3）求供应工程J1零件为红色的供应商号码SNO
$$
\Pi_{SNO}(\Pi_{SNO,PNO}(\sigma_{JNO='J1'}(SPJ))\bowtie\Pi_{PNO}(\sigma_{COLOR='红'}(P)))
$$
（4）求没有使用天津供应商生产的红色零件的工程号JNO
$$
\Pi_{JNO}(J)-\Pi_{JNO}(\Pi_{SNO,PNO,JNO}(SPJ)\bowtie\Pi_{SNO}(\sigma_{CITY='天津'}(S))\bowtie\Pi_{PNO}(\sigma_{COLOR='红'}(P)))
$$
（5）求至少用了供应商S1所供应的全部零件的工程号JNO
$$
\Pi_{PNO,JNO}(SPJ)\div\Pi_{PNO}(\sigma_{SNO='S1'}(SPJ))
$$

### 7. 试述等值连接与自然连接的区别和联系

- 等值连接是$\theta$为“=”的连接运算。

- 自然连接是一种特殊的等值连接，它要求两个关系中进行比较的分量必须是相同的属性组，并且**在结果中把重复属性列去掉**。即等值连接去掉重复属性就是自然连接。

### 8. 关系代数的基本运算有哪些？如何用这些基本运算来表示其他运算？

- 并、差、笛卡尔积、投影和选择是基本运算。

- 交运算：
  $$
  R\cap S=\{t\,|\,t\in R\and t\in S\}
  $$

- 连接运算：
  $$
  R\mathop{\bowtie}\limits_{A\theta B} S=\sigma_{A\theta B}(R\times S)
  $$

- 除运算：
  $$
  R(X,Y)\div S(Y,Z)=\Pi_X(R)-\Pi_X(\Pi_X(R)\times \Pi_Y(S)-R)
  $$

## PPT作业

C A P不能进行自然连接！！！只能进行笛卡尔积操作！！！

1. 找出所有顾客、代理商和商品都在同一个城市的三元组(cid,aid,pid)

$$
\Pi_{cid,aid,pid}(\sigma_{C.city=A.city\and A.city=P.city}(Customer\bowtie Agents\bowtie Products))
$$

2. 找出所有顾客、代理商和商品两两不在同一个城市的三元组(cid,aid,pid)。

$$
\Pi_{cid,aid,pid}(\sigma_{C.city\neq A.city\and A.city\neq P.city\and P.city\neq C.city}(Customer\bowtie Agents\bowtie Products))
$$

3. 取出至少被一个在杭州的顾客通过位于上海的代理商定购的商品的名字。

$$
\Pi_{Pname}(Orders\bowtie\sigma_{city='杭州'}(Customer)\bowtie\sigma_{city='上海'}(Agents)\bowtie Products)
$$

​	列出所有在同一个城市的代理商的aid对。
$$
\Pi_{A1.aid,A2.aid}(\sigma_{A1.city=A2.city\and A1.aid\ne A2.aid}(A1\bowtie A2)),A1=A2=Agents
$$

4. 取出销售过所有曾被顾客c002定购过的商品的代理商的名字。

$$
\Pi_{Aname}(Agents\bowtie\Pi_{aid}(\sigma_{cid='c002'}(Orders)))
$$

5. 取出所有的三元组(cid,aid,pid)，要求对应的顾客，代理商和商品中至少有两者是位于同一座城市。

$$
\Pi_{cid,aid,pid}(\sigma_{C.city=A.city\or A.city=P.city\or P.city=C.city}(Customer\bowtie Agents\bowtie Products))
$$

6. 取出接受过上海的顾客一笔总额超过￥500的订单的代理商的aid值。

$$
\Pi_{aid}(\sigma_{Qty>500}(Orders)\bowtie\sigma_{city='上海'}(Customer))
$$

7. 取出只从一家代理商处定购过商品的顾客的cid值。

$$
\Pi_{cid}(Order)-\Pi_{cid}(\sigma_{O1.aid\ne O2.aid}(O1\mathop{\bowtie}\limits_{O1.cid=O2.cid}O2)),O1=O2=Order
$$

其中，减数为从两家及以上代理商处订购过商品的顾客的cid值。