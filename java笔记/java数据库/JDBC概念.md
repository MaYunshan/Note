##JDBC(Java Data Base Connectivity)java数据库连接
###一.相关概念
* JDBC
	*  JDBC是一种用于执行SQL语句的Java API，可以为多种数据库关系数据库提供统一访问，它由一组用Java语言编写的类和接口组成。jDBC为数据库开发人员提供了一种标准的API,使他们能够用纯Java API来编写数据库应用程序。JDBC是进行数据库连接的抽象层。
* 数据库驱动
	* 我们安装好数据库之后，我们的应用程序是不能直接使用数据库的，必须要通过相应的数据库驱动程序，通过驱动程序去和数据库打交道。其实也就是数据库厂商的JDBC接口实现，即对Connection等接口的实现类的jar文件。
* JDBC驱动的种类
	* JDBC-ODBC桥加ODBC驱动程序
		*  JDBC API---------jdbc-odbc桥-------odbc------厂商DB代码---------数据库Server
		*  这种类型的驱动实际是把所有 jdbc的调用传递给odbc ,再由odbc调用本地数据库驱动代码.( 本地数据库驱动代码是指 由数据库厂商提供的数据库操作二进制代码库)
		*  要求客户端必须装有odbc驱动，不适合基于Internent的开发。
		*  由于jdbc-odbc先调用 odbc再由odbc去调用本地数据库接口访问数据库.所以,执行效率比较低,对于那些大数据量存取的应用是不适合的
	* 本地api驱动
		* JDBC API------JDBC驱动-------厂商提供的本地API---------------数据库Server
		* 本地api驱动直接把jdbc调用转变为数据库的标准调用再去访问数据库
		* 仍然需要在客户端加载数据库厂商 提供的代码库，这样就不适合基于internet的应用。
	* JDBC网络纯Java驱动程序
		*  JDBC API-------- JDBC网络协议驱动---------中间件服务器-------厂商提供的本地API------------数据库Server
		*  使用一种与数据库无关的协议将数据库请求发送给服务器构件，然后该构件再将数据库请求翻译成为特定数据库协议。
		*  由于这种驱动是基于server的.所以,它不需要在客户端加载数据库厂商提供的代码库。
		*  适合那种需要同时连接多个不同种类的数据库并且对并发连接要求高的应用。
		*  这种驱动在中间件层仍然需要配置其它数据库驱动程序,并且由于多了一个中间层传递数据,它的执行效率还不是最好。
	*  本地协议纯Java驱动程序
		*  JDBC API--------JDBC驱动---------数据库Server
		*  这种驱动直接把JDBC调用转换为符合相关数据库系统规范的请求。这种类型的驱动完全由java实现，实现了平台独立性。
		*  不需要在客户端或服务器端装载任何的软件或驱动. 这种驱动程序可以动态的被下载.但是对于不同的数据库需要下载不同的驱动程序。
* JDK规范的接口
		
				java.sql.Driver

				java.sql.Connection

				java.sql.Statement

				java.sql.PreparedStatement

				java.sql.ResultSet

				java.sql.CallableStatement

	* JDK仅仅只是提供了连接数据库的接口而已，并没有提供具体的实现了，接口实现类是由数据库厂商或者第三方提供的，
	* 这样的好处是：我们只需要学会上面几个接口的基本方法即可，那么我们就可以连接不同的数据库了，
	* 它屏蔽了连接不同数据库的差异性，但是需要在连接不同的数据库时导入相关的实现类，这个实现类，我们一般称为数据库的驱动包。
* JDBC编程步骤
	* 1.装载相应的数据库的JDBC驱动并进行初始化
		* 导入相应数据库的包：项目路径下建立lib库文件夹，导入访问mysql的第三方的类库

				mysql-connector-java-8.0.15.jar
		* 初始化驱动：Class.forName是把这个类加载到JVM中，加载的时候，就会执行其中的静态初始化块，完成驱动的初始化的相关工作。
				
				String driverClass=com.mysql.cj.jdbc.Driver；
				Class.forName(driverClass);
		 
	* 2.建立JDBC和数据库之间的Connection连接
		* 通过DriverManage的getConnection()方法获取数据库连接
		
				String jdbcUrl="jdbc:mysql://localhost:3306/girls?serverTimezone=UTC&useSSL=false";
				String user="root";
				String password="mysql";
				Connection connection = DriverManager.getConnection(jdbcUrl, user, password);
	* 3.创建Statement或者PreparedStatement接口，执行SQL语句
		* JDBC中的Statement和PrepareStatement是用来和数据库进行交互的接口，一旦和数据库建立连接，就可以使用它们的方法发送SQL语句到数据库并且从数据库获得返回数据
			* Statement接口：Statement接口创建之后，可以执行SQL语句，完成对数据库的增删改查。其中 ，增删改只需要改变SQL语句的内容就能完成，然而查询略显复杂。在Statement中使用字符串拼接的方式，该方式存在句法复杂，容易犯错等缺点。
					
					//2.准备要执行的sql语句

					//向表中插入数据
					String sql1="INSERT INTO BOYS(id,boyname,userCP) VALUES('5','杨过','50');";
					//更新表中的数据
					String sql2="UPDATE BOYS SET boyName='关羽' WHERE id=2;";
					//删除表中的数据
					String sql3="DELETE FROM BOYS WHERE boyName='杨过';";
					
					//3.执行插入

					//1).获取操作sql语句的statement对象：调用connection的createStatement()方法获取
					statement=connection.createStatement();
					
					//2).调用statement对象的executeUpdate(sql)执行sql语句进行插入
					statement.executeUpdate(sql);
			* PrepareStatement接口：继承自Statement接口，可以传入带占位符的sql语句，并且提供了补偿占位符变量的方法。
				* 优点：
					* PrepareStatement能防止sql注入。
					* prepareStatement会先初始化SQL，先把这个SQL提交到数据库中进行预处理，多次使用可提高效率。createStatement不会初始化，没有预处理，每次都是从0开始执行SQL

								//准备sql语句
								String sql="insert into exam_student values(?,?,?);";
								
								//从连接对象处获得PrepareStatement对象，然后分别设置占位符处的数据
								PreparedStatement ps=conn.PrepareStatement(sql);
								ps.setInt(1, 6);
								ps.setString(2, "郭靖");
								ps.setInt(3, 89);
								
								//调用ps对象的executeUpdate()执行sql语句进行插入,这里无需传递sql语句
								ps.executeUpdate();
	* 4.处理和显示结果(处理结果集ResultSet;)
		* ResultSet接口：结果集，封装了使用JDBC进行查询的结果。
			* 调用statement对象的executeQuery(sql)可以得到要查询结果集。
			* ResultSet返回的实际上就是一张数据表，有一个指针指向数据表的第一行的前面，可以调用next()方法方法检测下一行是否有效，如果有效该方法返回true，且指针下移。相当于Iterator的hasNext()和next()方法的结合体。当指针移动某一行时。
			* 可以通过getXXx(index)和getXxx(columnName)获取每一列的值。

					//2.获取statement对象
					statement=conn.createStatement();
					
					//3.准备sql语句
					String sql="SELECT id,boyName,userCP from boys;";
					
					//4.执行sql语句
					rs = statement.executeQuery(sql);
					
					//5.处理ResultSet
					while(rs.next()) {
						int id=rs.getInt(1);
						String name=rs.getString("boyName");
						int cp=rs.getInt("userCP");
						System.out.println(id+"\t"+name+"\t"+cp);
					}
		
	* 5.释放资源(按顺序关闭ResultSet、statement、Connection)
		* Connection、Statement、ResultSet都是应用程序和数据库的连接资源，使用后一定要关闭，需要在finally中关闭连接资源。
		* 关闭顺序 ：从里到外，ResultSe、statement，然后关闭先获取的connection

				/**
				 * 	关闭数据库连接
				 */
				public static void release(ResultSet rs,Statement statement,Connection connection) {
					//释放ResultSet资源
					if(rs!=null) {
						try {
							rs.close();
						} catch (Exception e) {
							e.printStackTrace();
						}
					}

					//释放Statement资源
					if(statement!=null) {
						try {
							statement.close();
						} catch (Exception e) {
							e.printStackTrace();
						}
					}

					//释放数据库连接资源
					if(connection!=null) {
						try {
							connection.close();
						} catch (Exception e) {
							e.printStackTrace();
						}
					}
				}
	* 上面的连接过程是针对于MySQL数据库的，如果要连接其它的数据库，需要在源程序中修改参数信息，比较麻烦。采用配置文件可以避免修改源程序。
		* 把数据库驱动Driver实现类的全类名、url、user、password放入一个配置文件jdbc.properties,当需要改变连接的数据库时，只需要修改该配置文件中的信息就可以实现和具体数据库的解耦，jdbc.properties中的信息如下
	
				driver=com.mysql.cj.jdbc.Driver
				jdbcUrl=jdbc:mysql://localhost:3306/girls?serverTimezone=UTC&useSSL=false
				user=root
				password=mysql 
		* 通过反射的方式加载数据库连接信息
			
				/**
				 * 	获取连接的方法
				 * 	通过读取数据库配置文件从数据库服务器获取一个连接
				 * @throws Exception
				 */
				public static Connection testDriverManger() throws Exception {
					//准备连接数据库的四个字符串
					String driverClass=null;		//驱动的全类名
					String jdbcUrl=null;			//JDBC url
					String user=null;				//用户名
					String password=null;			//用户密码
					
					//读取类路径下的jdbc.properties文件
					InputStream is = JDBCTools.class.getClassLoader().getResourceAsStream("jdbc.properties");
					Properties properties=new Properties();
					properties.load(is);
					
					driverClass=properties.getProperty("driver");
					jdbcUrl=properties.getProperty("jdbcUrl");
					user=properties.getProperty("user");
					password=properties.getProperty("password");
			
					//加载数据库驱动程序【注册驱动】
					//DriverManager.registerDriver(Class.forName(driverClass).newInstance());
					Class.forName(driverClass);
					
					//通过DriverManage的getConnection()方法获取数据库连接
					Connection connection = DriverManager.getConnection(jdbcUrl, user, password);
					return connection;

* 利用反射及JDBC元数据编写通用的查询方法				}
	* ResultSetMetaData 类
		* getColumnName(int column)：获取指定列的名称，索引从1开始
		* getColumnCount()：返回当前 ResultSet 对象中的列数
		* getColumnLabel(int column):返回指定列的别名，索引从1开始
	* 1.先利用sql语句进行查询，得到结果集
	* 2.利用反射创建实体类的对象
	* 3.获取结果集的列的别名，别名与java类中的字段名称一样
	* 4.获取每一列的值，将列名和对应的值装进map
	* 5.遍历map，再次利用反射 为2中的对象的各个属性赋值，属性为map中的key，值为map中对应的value

			/**
			 * 利用反射读取数据
			 * @param clazz：描述的对象类型
			 * @param sql：要执行的sql语句
			 * @param args：填充占位符的可变参数数组
			 * @return
			 */
			public static<T>  T getInfo(Class<T> clazz,String sql,Object ...args) {
				T object=null;
				Connection conn=null;
				PreparedStatement ps=null;
				ResultSet rs=null;
				try {
					//获取数据库连接
					conn=testDriverManger();
					
					//获取ps对象
					ps=conn.prepareStatement(sql);
					
					//设置占位符
					for (int i = 0; i < args.length; i++) {
						ps.setObject(i+1, args[i]);
					}
					
					//1.与数据库进行交换并获取查询结果
					rs=ps.executeQuery();
					
					//1.1得到 ResultSetMetaData对象
					ResultSetMetaData rsmd=rs.getMetaData();
					
					//1.2创建map对象，key用来保存列的别名，value用来保存对应的属性
					Map<String, Object> map=new HashMap<String, Object>();
					
					//处理结果集
					while(rs.next()) {
						
						//3.遍历rsmd对象，取出所有的列别名和对应的值添加到map中
						for (int i = 0; i < rsmd.getColumnCount(); i++) {
							//获取列名
							String columnName=rsmd.getColumnLabel(i+1);
							//获取列名对应的值
							Object columnValue=rs.getObject(columnName);
							//将列名和值添加到map中
							map.put(columnName, columnValue);
						}
						
						if(!map.isEmpty()) {
							//2.如果map不为空，利用反射创建clazz对应的对象
							object=clazz.newInstance();
							
							//
							for (String fieldName : map.keySet()) {
								Object fieldValue=map.get(fieldName);
								//5. 利用反射为对象添加属性

								//获取字段名
								Field nameField=clazz.getDeclaredField(fieldName);
								
								//如果是private变量，设置可访问权限
								if(!Modifier.isPublic(nameField.getModifiers())){
									nameField.setAccessible(true);
								}
								nameField.set(object, fieldValue);
							}
						}
					}
				}catch (Exception e) {
						e.printStackTrace();
				}finally {
					release(rs, ps, conn);
				}
				return object;
			}

