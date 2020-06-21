### 数据库
* 数据库(DataBase,DB)是按照数据结构来组织、存储和管理数据的仓库。

### 数据库体系结构
* 三级模式结构，两级映像功能
	* 外模式：又称子模式或用户模式，对应用户级，它是从用户角度看到的数据库的数据视图，反应了数据库的用户观
	* 概念模式：又称逻辑模式，对应概念级，是对数据库中全部数据的逻辑结构和特征的总体描述
	* 内模式：又称存储模式，对应物理级，是数据库中全体数据的内部表示或底层描述，反应数据在存储介质上的存储方式和物理结构
* 数据库通常分为层次式数据库、网络式数据库和关系式数据库三种。其中层次式，网络式数据库又称为非关系型数据库。

### MySQL数据库
* 常用指令
	* mysql服务器的启动和停止
	
			1.计算机右击-管理-服务
			2.管理员身份运行cmd，
				net start mysql80
				net stop mysql80
	* mysql服务器的登录和退出
			
			登录：
				1.通过mysql自带的客户端(仅限于root用户)
				2.mysql【-h主机名 -P端口号 】-u用户名 -p密码
			退出：
				3.ctrl+c或者exit
	* 查看当前所有的数据库
			
			show databases;
	* 打开指定的库

			use 库名
	* 查看当前库的所有表

			show tables;
	* 查看其它库的所有表

			show tables from 库名;
	
	* 查看表结构
		
			desc 表名;
	* 查看服务器的版本
	
			1.登录到mysql服务端
				select version();
			2.没有登录到mysql服务端
				mysql --version 或 mysql --V

* DQL(Date Query Language)数据查询语言
	* 基础查询
	* 条件查询
	* 排序查询
	* 常见函数
	* 分组查询
	* 连接查询
	* 子查询
	* 分页查询
	* 联合查询
		
			SELECT 查询列表			7
			FROM 	表1				1
			连接类型 JOIN 表2			2
			ON 连接条件				3		根据连接条件形成一个虚拟表
			WHERE 筛选条件			4
			GROUP BY 分组条件			5
			HAVING 分组后的筛选		6		也会形成一个虚拟表
			ORDER BY 排序列表			8
			LIMIT 偏移，条目数			9

* DML(Data Manipulation Language)数据操纵语言
	* 插入语句[insert]
	* 修改语句[update]
	* 删除语句[delete]

* DDL(Date Definition Language)数据定义语言
	* 库和表的管理
		* 库的创建

				CREATE DATABASE IF NOT EXISTS 库名;
		* 库的删除

				DROP DATABASE IF EXISTS 库名;
		* 表的创建
			
				create table 表名(
					字段名 字段类型,
					字段名 字段类型,
					...			,	
					字段名 字段类型
				);
		* 表的修改
			
				alter table 表名 add|drop|modify|change|rename to| column 列名 【列的类型，约束】
		* 表的删除
			
				DROP TABLE IF EXISTS 表名;

* TCL(transaction control language)事务控制语言
	* 事务： 一个或者一组sql语句组成一个执行单元，这个执行单元要么全部执行，要么全部不执行
	* 特性(ACID)
		* A(原子性):一个事务不可再分割，要么都执行要么不执行
		* C(一致性):一个事务执行会使数据从一个一致状态切换到另外一个一致状态
		* I(隔离性):一个事务的执行不会受其它事务的干扰
		* D(持久性):一个事务一旦提交，则会永远的改变数据库的数据
	* 分类
		* 隐式事务：事务没有明显地开启和结束的标记
		* 显示事务：事务具有明显地开启和结束的标记【必须先设置自动提交功能为禁用】
	* 创建

			1.开启事务
				set autocommit=0;
				start transaction; 【可选的】
			2.编写事务中的sql语句(select insert update delete)
				语句1,
				语句2,
				...
			3.结束事务
				commit;提交事务
				rollback;回滚事务
	* 事务并发问题
		* 脏读：当一个事务读取到另外一个事务未提交的更新数据时称为脏读。例如当A和B两个事务并发执行，A事务如果读取到B事务没有提交的数据，这个时候如果B事务进行回滚，那么A事务得到的数据就不是数据库中的真实数据。
		* 不可重复读：当事务对同一行数据进行重复读取时，得到的数据不同时出现数据不一样的问题。例如当A和B两个事务进行并发执行，A要读取表中的一条记录，而B恰好要修改表中的这一条记录，当A读取时B恰好对这条记录进行了修改，那么当A再次对该条记录进行读取的时候内容已经发生了变化。
		* 幻读：一个事务读取到另一个事务已提交的新插入的数据。例如A和B并发执行，A中事务查询数据，B中事务插入或者删除数据，当A查询一个结果集时，B正好插入了一条记录，这时A再次查询时会出现以前没有的数据或者删除后的数据。
	* 事务的隔离级别
		
										脏读			幻读		不可重复读
			read uncommitted(未提交读):	会			会			会
			read committed(提交后读):		不会			会			会
			repeatable read(可重复读): 	不会			不会			会
			serializable(序列化)：		不会			不会			不会
	* 查看隔离级别
 
			select @@transaction_isolation;
			show variables like 'transaction_isolation';
	*设置隔离级别

			set session/global transaction isolation level 隔离级别
* 视图
	* 含义：视图是对若干张基本表的引用，一张虚表，查询语句执行的结果集，不存储具体的数据。
	* 作用
		* 方便操作，特别是查询操作，减少复杂的SQL语句，增强可读性；
		* 更加安全，数据库授权命令不能限定到特定行和特定列，但是通过合理创建视图，可以把权限限定到行列级别；
	* 视图的创建

			CREATE VIEW myv1
			AS 
			SELECT 查询列表
			FROM 	表1
			连接类型 JOIN 表2	
			ON 连接条件
			WHERE 筛选条件
			GROUP BY 分组条件
			HAVING 分组后的筛选
			ORDER BY 排序列表
			LIMIT 偏移，条目数
	* 视图的修改
		
			create or replace view 视图名
			as
			查询语句
		
			alter view 视图名
			as
			查询语句
	*视图的删除
			
			drop view 视图名1，视图名2,....;

* 变量
	* 系统变量
		* 全局变量
			* 服务器每次启动将为所有的全局变量赋初识值，针对于所有的会话(连接)有效，但不能跨重启
		* 会话变量
			* 仅仅针对于当前的会话有效
		* 使用方法：

				1.查看所有的系统的变量
					show global|session variables;
				2.查看满足条件的部分系统变量
					show global|session variables like '';
				3.查看指定的某个系统变量的值
					select @@系统变量名;|select @@golbal.系统变量名;
				4.为某个指定的系统变量赋值
					set global|session 系统变量名=值;
					set @@global|session.系统变量名=值;
		* 注意：
			如果是全局变量，则需要加global，如果是会话级别，需要加session，如果不加，默认是session

	* 自定义变量
		* 用户变量
			* 针对于当前会话有效，等同于会话变量的作用域	
			* 使用

					1.声明并初始化：【=或者：=】
						set @用户变量名=值;
						set @用户变量名：=值;
						select @用户变量名：=值;
					
					2.赋值（更新用户变量名的值）
						a.通过set或者select
							set 用户变量名=值;
							set 用户变量名：=值;
							select @用户变量名：=值;
						
						b.通过select查询语句
							select 字段 into 变量名 from 表;
					
					3.使用
						select @用户变量名;
		* 局部变量
			* 仅仅在定义它的begin end中有效

					1.声明
						declare 变量名 类型
						declare 变量名 类型 default 值;
					2.赋值
						a.通过set或者select
							set 用户变量名=值;
							set 用户变量名：=值;
							select @用户变量名：=值;
						
						b.通过select查询语句
							select 字段 into 变量名 from 表;
							
					3.使用

* 存储过程
	* 概念：
		* 一组预先编译好的为了完成特定功能的sql语句的集合，类似于批处理语句
	* 优点：
		* 增强SQL语言的功能和灵活性：
			* 存储过程可以用控制语句编写，有很强的灵活性，可以完成复杂的判断和较复杂的运算。
		* 标准组件式编程：
			* 存储过程被创建后，可以在程序中被多次调用，而不必重新编写该存储过程的SQL语句。
		* 较快的执行速度：
			* 如果某一操作包含大量的Transaction-SQL代码或分别被多次执行，那么存储过程要比批处理的执行速度快很多。因为存储过程是预编译的。
	* 创建
		
			DELIMITER 符号     //设置结束符号
			CREATE PROCEDURE 存储过程名(参数列表)
			BEGIN
				存储过程语句
			END
	
			参数列表包含三部分
				参数模式
					in：该参数可以作为输入，即该参数需要调用方传入值
					out：该参数可以作为输出，该参数可以作为返回值
					inout：即可以作为输入又可以作为输出，既需要传入值，又可以返回值
				参数名
				参数类型
	* 删除

			drop procedure 存储过程名;

* 流程控制语句
	* case语句
		* 1.类似于switch语句，一般用于实现等值判断

				case 变量|表达式|字段
				when 要判断的值 then 返回的值1或者语句1
				when 要判断的值 then 返回的值2或者语句2
				...
				else 要返回的值n或者语句n
				end case; 
		* 2.类似于多重if语句，一般用于实现区间判断
	
				CASE
				WHEN 要判断的条件1 THEN 返回的值1或者语句1
				WHEN 要判断的条件1 THEN 返回的值1或者语句1
				...
				ELSE 要返回的值n或者语句n;
				END CASE;
		* 特点
			* 可以作为表达式，嵌套在其它语句中使用，可以放在任何地方，begin end 中间或者begin end 外面
			* 如果when中的值满足推荐条件，则执行对应的then后面的语句，并且结束case
	* if结构
		* 基本用法

				IF(expr1, expr2, expr3) 
		* 实现多重分支

				IF 条件1 THEN 语句1;
				ELSEIF 条件2 THEN 语句2;
				...
				ELSE 语句n;
				END IF;
	* 循环结构
		* while循环

				label:WHILE 循环条件 DO
					循环体
				END WHILE label;
		* loop循环

				label:LOOP
					循环体;
				END LOOP label;
		* repeat

				label:REPEAT
					循环体;
				UNTIL 循环结束条件
				END REPEAT label;
		
		* iterate 
			* 类似于continue，结束本次循环，继续下一次循环
		* leave 	
			* 类似于break，跳出循环体，
				
* 数据库连接池	
	* 