---
title: SQL随机生成姓名、手机号、字符串等
date: '2023-11-15 17:45:47'
meta:
  - name: keywords
    content: MySQL
tags:
  - MySQL
categories:
  - 运维
permalink: /post/sql-randomly-generate-name-mobile-phone-number-string-etc-z3qcb3.html
author:
  name: Zhou
  link: https://github.com/Abner1zhou
---


<!-- more -->


# SQL随机生成姓名、手机号、字符串等

> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [www.jianshu.com](https://www.jianshu.com/p/8823710e64c6)

### 补充

关于 sql 中 rand() 函数的使用

```
select rand()#产生0-1之间的随机数
select rand()*3#产生0-3之间的随机数
select floor(rand()*3)#在0、1、2之间产生随机数
select ceiling(rand()*3)#在0、1、2、3之间生成随机数
select floor(1+rand()*3)#在1、2、3之间产生随机数
```

### 1、随机生成姓名 (函数的形式)

```
CREATE DEFINER=`root`@`%` FUNCTION `generateUserName`() RETURNS varchar(255) CHARSET utf8
DETERMINISTIC
BEGIN
    DECLARE xing varchar(2056) DEFAULT '赵钱孙李周郑王冯陈楮卫蒋沈韩杨朱秦尤许何吕施张孔曹严华金魏陶姜戚谢喻柏水窦章云苏潘葛奚范彭郎鲁韦昌马苗凤花方俞任袁柳酆鲍史唐费廉岑薛雷贺倪汤滕殷罗毕郝邬安常乐于时傅皮齐康伍余元卜顾孟平黄和穆萧尹姚邵湛汪祁毛禹狄米贝明臧计伏成戴谈宋茅庞熊纪舒屈项祝董梁杜阮蓝闽席季麻强贾路娄危江童颜郭梅盛林刁锺徐丘骆高夏蔡田樊胡凌霍虞万支柯昝管卢莫经裘缪干解应宗丁宣贲邓郁单杭洪包诸左石崔吉钮龚程嵇邢滑裴陆荣翁';

    DECLARE ming varchar(2056) DEFAULT '嘉懿煜城懿轩烨伟苑博伟泽熠彤鸿煊博涛烨霖烨华煜祺智宸正豪昊然明杰诚立轩立辉峻熙弘文熠彤鸿煊烨霖哲瀚鑫鹏致远俊驰雨泽烨磊晟睿天佑文昊修洁黎昕远航旭尧鸿涛伟祺轩越泽浩宇瑾瑜皓轩擎苍擎宇志泽睿渊楷瑞轩弘文哲瀚雨泽鑫磊梦琪忆之桃慕青问兰尔岚元香初夏沛菡傲珊曼文乐菱痴珊恨玉惜文香寒新柔语蓉海安夜蓉涵柏水桃醉蓝春儿语琴从彤傲晴语兰又菱碧彤元霜怜梦紫寒妙彤曼易南莲紫翠雨寒易烟如萱若南寻真晓亦向珊慕灵以蕊寻雁映易雪柳孤岚笑霜海云凝天沛珊寒云冰旋宛儿绿真盼儿晓霜碧凡夏菡曼香若烟半梦雅绿冰蓝灵槐平安书翠翠风香巧代云梦曼幼翠友巧听寒梦柏醉易访旋亦玉凌萱访卉怀亦笑蓝春翠靖柏夜蕾冰夏梦松书雪乐枫念薇靖雁寻春恨山从寒忆香觅波静曼凡旋以亦念露芷蕾千兰新波代真新蕾雁玉冷卉紫山千琴恨天傲芙盼山怀蝶冰兰山柏翠萱乐丹翠柔谷山之瑶冰露尔珍谷雪乐萱涵菡海莲傲蕾青槐冬儿易梦惜雪宛海之柔夏青亦瑶妙菡春竹修杰伟诚建辉晋鹏天磊绍辉泽洋明轩健柏煊昊强伟宸博超君浩子骞明辉鹏涛炎彬鹤轩越彬风华靖琪明诚高格光华国源宇晗昱涵润翰飞翰海昊乾浩博和安弘博鸿朗华奥华灿嘉慕坚秉建明金鑫锦程瑾瑜鹏经赋景同靖琪君昊俊明季同开济凯安康成乐语力勤良哲理群茂彦敏博明达朋义彭泽鹏举濮存溥心璞瑜浦泽奇邃祥荣轩';
  
    DECLARE I_xing int DEFAULT LENGTH(xing) / 3;
    DECLARE I_ming int DEFAULT LENGTH(ming) / 3;
    DECLARE return_str varchar(2056) DEFAULT '';
  
    SET return_str = CONCAT(return_str, substring(xing, floor(1 + RAND() * I_xing), 1));#substring(str,pos,len):由 <str> 中的第 <pos> 位置开始，选出接下去的 <len> 个字元。
    SET return_str = CONCAT(return_str, substring(ming, floor(1 + RAND() * I_ming), 1));
  
    IF RAND() > 0.400 THEN
        SET return_str = CONCAT(return_str, substring(ming, floor(1 + RAND() * I_ming), 1));
    END IF;
  
    RETURN return_str;
END
```

#### 调用函数

```
select `generateUserName`()
```

### 2、随机生成手机号

```
CREATE FUNCTION `generatePhone`() RETURNS char(11) CHARSET utf8
DETERMINISTIC
BEGIN
    DECLARE head VARCHAR(100) DEFAULT '000,156,136,176';
    DECLARE content CHAR(10) DEFAULT '0123456789';
    DECLARE phone CHAR(11) DEFAULT substring(head, 1+(FLOOR(1 + (RAND() * 3))*4), 3);#注意sql下标从1开始
    DECLARE i int DEFAULT 1;
    DECLARE len int DEFAULT LENGTH(content);
    WHILE i<9 DO
        SET i=i+1;
        SET phone = CONCAT(phone, substring(content, floor(1 + RAND() * len), 1));
    END WHILE;
  
    RETURN phone;
END
```

#### 调用函数

```
select `generatePhone`()
```

### 3、随机产生一个日期

```
#xxxx-xx-xx格式
select date(from_unixtime(unix_timestamp('1980-01-01') +floor(rand() * ( unix_timestamp('2000-01-01') -unix_timestamp('1980-01-01') + 1 ))));#产生1980-2000年之间的日期
```

### 4、随机产生包含大小写字母和数字的字符串

```
CREATE FUNCTION rand_string(n INT) RETURNS VARCHAR(255) CHARSET utf8
DETERMINISTIC
BEGIN
    DECLARE chars_str varchar(100) DEFAULT 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789';
    DECLARE return_str varchar(255) DEFAULT '';
    DECLARE i INT DEFAULT 0;
    WHILE i < n DO
        SET return_str = concat(return_str,substring(chars_str , FLOOR(1 + RAND()*62 ),1));
        SET i = i +1;
    END WHILE;
    RETURN return_str;
END
```

#### 调用函数

```
select rand_string(30);
```

### 5、批量插入数据（存储结构）

```
delimiter $$
CREATE PROCEDURE insertMember()
    begin
    declare i int;
    set i=100000000;
    while i<101000000 do 
        #在这里可以进行多张表的插入
        insert into vip_member(id,username,`password`,fullname,header_url,nickname,mobile,email,sex,birthday,enabled,
        deleted,create_time,create_date,create_user,modify_date,modify_user) 
        values(i,null,null,(select `generateUserName`()),'https://www.baidu.com',(select `generateUserName`()),(select `generatePhone`()),'123@163.com',null,'1997-08-06',1,0,'2019-09-11 16:22:18','2019-09-11 16:22:18',null,'2019-09-11 16:22:18',null);
    set i=i+1;
    end while;
end;
```

#### 调用和删除存储结构

```
call insertUser1();#调用
drop procedure insertMember; #删除
```

### 6、删除数据中的空格

```
update 表名 set 字段=trim(字段)
```

### 7、复制表结构

```
create table 表名 like 表名;
```

### 8、设置主键自动从 1 递增

```
#先把表清空
ALTER table 数据库.表名 AUTO_INCREMENT = 1;
```

### 9、将一张表中的数据对应导入到另一张表中

例如：

##### 表 qr_code_all：

<table><thead><tr><th>id</th><th>encrypted</th><th>signature</th></tr></thead><tbody><tr><td>1</td><td>123</td><td></td></tr><tr><td>2</td><td>234</td><td></td></tr><tr><td>3</td><td>456</td><td></td></tr></tbody></table>

##### 表 qr_code_test：

<table><thead><tr><th>id</th><th>encrypted</th><th>signature</th></tr></thead><tbody><tr><td>1</td><td></td><td>a</td></tr><tr><td>2</td><td></td><td>b</td></tr><tr><td>3</td><td></td><td>c</td></tr></tbody></table>

##### 表 qr_code_all：

<table><thead><tr><th>id</th><th>encrypted</th><th>signature</th></tr></thead><tbody><tr><td>1</td><td>123</td><td>a</td></tr><tr><td>2</td><td>234</td><td>b</td></tr><tr><td>3</td><td>456</td><td>c</td></tr></tbody></table>

将 qr_code_test 中的数据插入 qr_code_all 中

```
update qr_code_all a 
inner join 
(select id,signature from qr_code_test) b 
on a.id=b.id 
set a.signature=b.signature;
```
