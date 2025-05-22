第1题 tag:基础

```sql
select * from user_profile
```

第2题 tag:基础

```sql
select device_id , gender , age , university from user_profile
```

第3题 tag:基础 distinct 去重 放在字段前面

```sql
select distinct  university from user_profile 
```

第4题 tag:基础 limit 限制返回几个

```sql
select device_id from user_profile limit 2
```

 第5题 tag:基础 as 别名 limit 限制返回几个

```sql
select device_id as user_infos_example from user_profile limit 2
```

第6题 tag:基础 where 限制条件 like 进行字段匹配  

```sql
select device_id , university from user_profile where university like '北京大学'
```

 第7题 tag:基础 where 限制条件 > 大于判断

```sql
select device_id , gender , age , university from user_profile where age > 24
```

第8题 tag:基础 where 限制条件 >= <= 大小判断 && 连接条件 和 and 一样

```sql
select device_id ,gender ,age from user_profile where age >= 20 && age <=23
```

 第9题 tag:基础 not in ('xx',('xx')...) 或者 not like 可以用 not 来判断 注意最好全用 ' '单引号

```sql
# select device_id , gender ,age ,university from user_profile where university not like "复旦大学"
select device_id , gender ,age ,university from user_profile where university not in ('复旦大学')
```

第10题 tag:基础 is not null 过滤空值

```sql
select device_id , gender , age ,university from user_profile where age is not null
```

 第11题 tag:基础 where > and like

```sql
select device_id , gender , age , university , gpa from user_profile where gpa > 3.5 and gender like 'male'
```

第12题 tag:基础 

```sql
select device_id , gender ,age ,university,gpa from user_profile where university like '北%大%' or gpa >3.7
```

第13题 tag:基础

```sql
select
    device_id,
    gender,
    age,
    university,
    gpa
from
    user_profile
where
    university in ('北京大学','山东大学','复旦大学')

```

 第14题 tag:基础 可以组合 先and 再 用or组合 asc 升序 dsc 降序

```sql
select
    device_id,
    gender,
    age,
    university,
    gpa
from
    user_profile
where
    university like '山东大学'
    and gpa > 3.5
    or university like '复旦大学'
    and gpa > 3.8
order by
    device_id asc

```

第15题 tag:基础 %模糊匹配  注意select最后一个不要有`,`

```sql
select
    device_id,
    age,
    university
from
    user_profile
where
    university like '北京%'
```

 第16题 tag:基础 用函数MAX() 注意题目要求

```sql
select
    MAX(gpa) as gpa
from
    user_profile
where 
    university like '复旦大学'
```

第17题 tag:基础 COUNT()函数记录有效次数 AVG()函数记录所有有效条数的均值 注意`,` 

```sql
select
    COUNT(gender) as male_num ,
    AVG(gpa) as avg_gpa
from
    user_profile
where 
    gender like 'male'
```

 第18题 tag:难 ROUND(X,1)保留X的1位小数 

GROUP BY
    gender, university

`GROUP BY gender, university` 的作用是： 它会遍历 `user_profile` 表中的每一行，并根据 `gender` 和 `university` 这两列的值的**唯一组合**来创建分组。例如，所有 `gender` 为 'male' 且 `university` 为 '北京大学' 的行会进入一个组，所有 `gender` 为 'female' 且 `university` 为 '北京大学' 的行会进入另一个组，以此类推。 它是基于表中实际存在的数据行进行分组。 之后，聚合函数（如 `COUNT()`, `AVG()`）会在这些形成的每个独立组内进行计算。

```sql
SELECT
    gender,
    university,
    COUNT(id) AS user_num,
    ROUND(AVG(active_days_within_30), 1) AS avg_active_day,
    ROUND(AVG(question_cnt), 1) AS avg_question_cnt
FROM
    user_profile
GROUP BY
    gender, university
ORDER BY
    gender ASC, university ASC;
```

第19题 tag:难

`WHERE` 子句用于在数据**分组前**筛选行。它直接作用于表中的原始数据。

`HAVING` 子句用于在数据**分组后**筛选组。它作用于由 `GROUP BY` 子句创建的聚合结果。 您的条件 `avg_question_cnt < 5` 或 `avg_answer_cnt < 20` 是基于聚合函数 `AVG()` 计算出来的值。因此，这些条件必须在数据分组并计算出平均值之后应用，所以应该使用 `HAVING` 子句，而不是 `WHERE` 子句。

```sql
SELECT
    university,
    ROUND(AVG(question_cnt), 3) AS avg_question_cnt,
    ROUND(AVG(answer_cnt), 3) AS avg_answer_cnt
FROM
    user_profile
GROUP BY
    university
HAVING
    AVG(question_cnt) < 5 OR AVG(answer_cnt) < 20;
```

第20题 tag:基础 ROUND(X,4) AVG() group by asc

```sql
select
    university,
    ROUND(AVG(question_cnt),4) as avg_question_cnt
from 
    user_profile
group by
    university
order by 
    ROUND(AVG(question_cnt),4) asc
```

第21题 tag:基础 连表 内连接join on 作为连表的判断 

```sql
select
    q.device_id,
    q.question_id,
    q.result
from
    question_practice_detail q 
    join
    user_profile u
on 
    q.device_id = u. device_id
where 
    university like '浙江大学'
```

第22题 tag:基础 这个要读题 以及 `COUNT(q.question_id) / COUNT(DISTINCT q.device_id)`这个东西可以这么运算

```sql
SELECT
    u.university,
    ROUND(
        COUNT(q.question_id) / COUNT(DISTINCT q.device_id), 
        4
    ) AS avg_answer_cnt
FROM
    user_profile u
JOIN
    question_practice_detail q ON u.device_id = q.device_id
GROUP BY
    u.university
ORDER BY
    u.university ASC;
```

第23题 tag:困难 要深度理解 COUNT() 和题目 才能作对 ` COUNT(a.result) / COUNT(distinct a.device_id)` 以及涉及到第二个表的创建和关联。

COUNT() 理论上除了条件内和groupby字段都可以

groupby相当于固定了前面几个列 并通过方法整合比如

​			age num

北京大学 hard 1 2

北京大学 hard 1 3

北京大学 hard 2 4

北京大学 hard 3 5

就可能被整合为

​			SUM(age)  ROUND(SUM(age) / AVG(num) , 2)

北京大学 hard  7 2.00 (AVG(num)=(2, 3, 4, 5)/4 = 3.5 COUNT(num) = 4) 

```sql
select
    u.university,
    a.difficult_level,
    ROUND(
        COUNT(a.result) / COUNT(distinct a.device_id)
    ,4) as avg_answer_cnt
from
    user_profile u
    join
    (
        select 
        q.*,
        qd.difficult_level
        from
        question_practice_detail q join question_detail qd
        on
        q.question_id = qd.question_id
    ) a
    on
    u.device_id = a.device_id
group by 
u.university , a.difficult_level
```

第24题 tag:难 一定要在group by 里加上u.university

MySQL数据库有一个叫做 `only_full_group_by` 的SQL模式，在较新的版本中这通常是默认开启的。这个模式要求：**所有出现在 `SELECT` 列表中，但没有被聚合函数包裹的列，都必须明确地出现在 `GROUP BY` 子句中。**

```sql
select
    u.university,
    a.difficult_level,
    ROUND(
        COUNT(a.result)  / COUNT(distinct u.device_id) ,4
    ) as avg_answer_cnt
from user_profile u
    join (
        select 
            q.*,
            qd.difficult_level
        from question_practice_detail q
        join
            question_detail qd
        on q.question_id =  qd.question_id
    ) a
    on
        u.device_id = a.device_id
where
    u.university like '山东大学'
group by
    a.difficult_level , u.university
# having
#     u.university like '山东大学'
```

第25题 tag:不熟悉 union all 和 union

在 SQL 中，`UNION` 和 `UNION ALL` 都是集合操作符，它们允许你将来自不同 `SELECT` 查询的结果纵向合并到一个结果集中。这意味着它们会把多个查询返回的行堆叠在一起。

| 特性           | `UNION`                                                      | `UNION ALL`                                            |
| -------------- | ------------------------------------------------------------ | ------------------------------------------------------ |
| **重复行处理** | **移除**重复行 (结果集中的行是唯一的)                        | **保留**所有行，包括重复行                             |
| **性能**       | 通常**较慢**，因为它需要额外的处理来查找和移除重复行 (通常涉及排序操作)。 | 通常**较快**，因为它只是简单地合并结果，不做去重处理。 |
| **结果行数**   | 小于或等于各个查询结果行数之和。                             | 等于各个查询结果行数之和。                             |

```sql
select 
    device_id, gender, age, gpa
from user_profile
where university='山东大学'

union all

select 
    device_id, gender, age, gpa
from user_profile
where gender='male'
```

 第26题 tag:不了解 CASE WHEN THEN END

```sql
SELECT
    CASE
        WHEN age >= 25 THEN '25岁及以上'
        ELSE '25岁以下'
    END AS age_cut,  
    COUNT(device_id) AS number 
FROM
    user_profile
GROUP BY
    age_cut;  
```

第27题 tag:不熟悉  多个WHEN THEN 不用加`,`要急着 CASE 与 END

```sql
select
    device_id,
    gender,
    CASE
        WHEN age >= 25 THEN '25岁及以上'
        WHEN age < 25 AND age >= 20 THEN '20-24岁'
        WHEN age IS NULL THEN '其他'
        ELSE '20岁以下'
    END AS age_cut
from
    user_profile;
```

 第28题 tag:不了解 year() month() day() 函数  日期函数

```sql
select
    day(date) as day,
    count(question_id) as question_cnt
from question_practice_detail
where month(date)=8 and year(date)=2021
group by date
```

第29题 tag:又不会又难

只有 COUNT(a) COUNT(distict a) 和 COUNT(DISTINCT a,b)写法

 COUNT(DISTINCT a,b)：不重复的ab组合数：它统计 `expression1, expression2, ...` (这里是列 `a` 和 `b`) 形成的**不重复组合**的数量。如果所有表达式在某行都为 `NULL`，这个 `(NULL, NULL, ...)` 组合通常也会被算作一个不同的值（如果它确实出现了）。

left join 

会左表优先，根据on条件来进行组合

比如 q:a3 a1 b3 

q1 left join q2 on q1. id = q2.id

就会组合成 a3a3 a3a1 具体的是 q1.a3q2.a3 和q1.a3q2.a1 

他又在on条件里加了一个and 所以可以找出DATEDIFF (q2.date, q1.date) = 1相差一天的进行组合

并且**成功匹配的行（`LEFT JOIN` 的关键）：**

对于 `q1` 中的每一行，如果在 `q2` 中**没有**找到任何满足 `ON` 条件的行（例如，某个用户在某天活跃了 (`q1`)，但第二天没有活跃 (`q2`)），那么 `q1` 的这一行数据**仍然会**出现在中间结果集中。

如果是join就只剩组合成功的例子了也就是连续两天活跃用户

最后会出现类似于 a3a4 b4 c2c3 d1 这样的虚拟数据

如果 a1 a2 a3 b4 c3 c4  left join 自身 on id=id

最后会出现 a1a2,a1a3,a2a3,a3,b4,c3c4,c4

如果加上 and  datediff =1

最后会出现 a1a2,a2a3,a3,b4,c3c4,c4

接着COUNT(distinct q2.device_id, q2.date) a2a3c4是3

count(DISTINCT q1.device_id, q1.date) a1 a2 a3 b4 c3 c4 是6

```sql
SELECT
    COUNT(distinct q2.device_id, q2.date) / count(DISTINCT q1.device_id, q1.date) as avg_ret
from
    question_practice_detail as q1
    left join question_practice_detail as q2 on q1.device_id = q2.device_id
    and DATEDIFF (q2.date, q1.date) = 1

```

 第30题 tag: 还行 子表

```sql
select a.gender,
COUNT(a.device_id) as number
from
    (
        select
        device_id,
        CASE
        WHEN profile like '%female' THEN 'female'
        ELSE  'male'
        END as gender
        from user_submit 
    ) a
group by
a.gender
```

第31题 tag:不熟悉 SUBSTRING_INDEX(A,B,C)

**语法**: `SUBSTRING_INDEX(str, delim, count)`

`str`: 要处理的原始字符串，这里是 `blog_url` 列的值。

`delim`: 分隔符，这里是 `'/'` (斜杠)。

`count`:

- 如果 `count` 是正数，则返回第 `count` 个分隔符出现之前的所有字符。
- 如果 `count` 是负数，则返回倒数第 `count` 个分隔符之后的所有字符。

在这个例子中，`SUBSTRING_INDEX(blog_url, '/', -1)` 的作用是提取 `blog_url` 字符串中最后一个 `'/'` 之后的部分。这通常用于从 URL 中提取末尾的路径、文件名或标识符，这里被用来提取用户名。 

还有一个是SUBSTRING(str,begin,length)

begin：sql里第一个索引是1

```sql
SELECT
    device_id,
    SUBSTRING_INDEX (blog_url, '/', -1) AS user_name 
FROM
    user_submit
```

```sql 
select 
-- 替换法 replace(string, '被替换部分','替换后的结果')
-- device_id, replace(blog_url,'http:/url/','') as user_name

-- 截取法 substr(string, start_point, length*可选参数*)
-- device_id, substr(blog_url,11,length(blog_url)-10) as user_nam

-- 删除法 trim('被删除字段' from 列名)
-- device_id, trim('http:/url/' from blog_url) as user_name

-- 字段切割法 substring_index(string, '切割标志', 位置数（负号：从后面开始）)
device_id, substring_index(blog_url,'/',-1) as user_name

from user_submit;
```

第32题 tag:还行 错了两次

substring(profile,12,2) 列名不用加`''`

聚合函数外要加入 group by

```SQL
select
    substring(profile,12,2) as age,
    COUNT(device_id) as number
from    
    user_submit
group by
    substring(profile,12,2)
```
 第33题 tag: 秒 连表 

```sql
select
    u.device_id,
    a.*
from
    (
        select
            university,
            MIN(gpa) as gpa
        from
            user_profile
        group by
            university
    ) a
    join user_profile u on u.university = a.university
    and u.gpa = a.gpa
order by
    a.university

```

第34题 tag:难 但是妙 sum(if(qpd.result='right', 1, 0)) 不能是count ！

```sql
select up.device_id, '复旦大学' as university,
    count(question_id) as question_cnt,
    sum(if(qpd.result='right', 1, 0)) as right_question_cnt
from user_profile as up

left join question_practice_detail as qpd
  on qpd.device_id = up.device_id and month(qpd.date) = 8

where up.university = '复旦大学'
group by up.device_id
```

 第35题 tag: 还是 SUM(if (a.result = 'right', 1, 0))这个东西

```SQL
select
    a.difficult_level,
    ROUND(
        SUM(if (a.result = 'right', 1, 0)) / COUNT(a.result),
        4
    ) as correct_rate
from
    user_profile u
    join (
        select
            q.*,
            qd.difficult_level
        from
            question_practice_detail q
            left join question_detail qd on q.question_id = qd.question_id
    ) a on a.device_id = u.device_id
where
    university like '浙江大学'
group by
    a.difficult_level
order by
    ROUND(
        SUM(if (a.result = 'right', 1, 0)) / COUNT(a.result),
        4
    )

```
 第36题 tag:简单

```sql
select
    device_id,
    age
from
    user_profile
order by
    age

```

第37题 tag:基础  orderby 的先后也是排序的先后

```sql
select
    device_id,
    gpa,
    age
from 
    user_profile
order by 
    gpa,age
```

 第38题 tag:基础

```SQL
select
    device_id,
    gpa,
    age
from 
    user_profile
order by 
    gpa desc,age desc
```
 第39题 tag:简单

```sql
select
    COUNT(distinct device_id) as did_cnt,
    COUNT(result) as question_cnt
from
    question_practice_detail
where
    year (date) = 2021
    and month (date) = 8
```

第40题 tag:基础  REGEXP'xxxxx' 正则表达

```sql
select
    id,
    name,
    phone_number
from
    contacts
where
    phone_number REGEXP '^[1-9][0-9]{2}-?[0-9]{3}-?[0-9]{4}$';
```

 第41题 tag:不熟悉 窗口函数 sum(xx) over( order by xxx )	

- SUM(profit): 表示对 profit 列的值进行求和操作。
- OVER (): 是窗口函数的关键字，用于定义一个窗口，以便对一组行进行操作，而不是对整个表进行聚合。
- ORDER BY profit_date: 指定窗口中数据按照 profit_date（利润日期）进行排序，这是一个窗口排序操作。
- AS cumulative_profit: 将计算的结果命名为 cumulative_profit，即累积利润。

```SQL
select
    *,
    sum(profit) over (
        order by
            profit_date
    ) as cumulative_profit
from
    daily_profits
order by
    profit_date

```
  第42题 tag: abs() 绝对值 ceil() 向上取整 floor()向下取整 round(x,z)保留n位小数

```sql
select *,
    abs(value) as absolute_value,
    ceil(value) as ceiling_value, #向上取整
    floor(value) as floor_value, #向下取整
    round(value,1) as rounded_value
from numbers
order by id asc
```

