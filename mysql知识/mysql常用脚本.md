## mysql常用基本整理

---



#### 在日常开发中处理问题常用的脚本以及一些常用参数的查看和解释

#### 1.查询表容量

> SELECT
>
>   table_schema AS '数据库',
>
>   table_name AS '表名',
>
>   table_rows AS '记录数',
>
>   TRUNCATE ( data_length / 1024 / 1024, 2 ) AS '数据容量(MB)',
>
>   TRUNCATE ( index_length / 1024 / 1024, 2 ) AS '索引容量(MB)',
>
>   TRUNCATE (
>
> ​    ( index_length + data_length ) / 1024 / 1024,
>
> ​    2
>
>   ) AS '总容量          (MB)'
>
> FROM
>
>   information_schema.TABLES
>
> WHERE
>
>   table_schema = 'OP'
>
> ORDER BY
>
>   index_length + data_length DESC

#### 2.查询当前用户下正在执行的sql进程

>show processlist 使用root用户

#### 3.查看数据库连接超时时间

>show global variables like 'wait_timeout';  -- 非交互式超时时间， 如 JDBC 程序
>show global variables like 'interactive_timeout';  -- 交互式超时时间， 如数据库工具

#### 4.查看线程数

> Threads_cached：缓存中的线程连接数。
> Threads_connected：当前打开的连接数。
> Threads_created：为处理连接创建的线程数。
> Threads_running：非睡眠状态的连接数，通常指并发连接数。
>
> 每产生一个连接或者一个会话，在服务端就会创建一个线程来处理。反过来，如果要
> 杀死会话，就是 Kill 线程。

#### 5.数据库允许最大连接数

>show variables like 'max_connections'; 
>
>set global max_connections = 1000;
>
>show 的参数说明：
>1、级别：会话 session 级别（默认）；全局 global 级别
>2、动态修改：set，重启后失效；永久生效，修改配置文件/etc/my.cnf









