Dapper.SimpleCRUD  -  Dapper的简单CRUD助手
========================================
特征
--------
<img  align="right" src="https://raw.githubusercontent.com/ericdc1/Dapper.SimpleCRUD/master/images/SimpleCRUD-200x200.png" alt="SimpleCRUD">
Dapper.SimpleCRUD是一个[单个文件](https://github.com/ericdc1/Dapper.SimpleCRUD/blob/master/Dapper.SimpleCRUD/SimpleCRUD.cs)，您可以直接进入将扩展IDbConnection接口的项目。 (如果需要动态支持，则需要[附加文件](https://github.com/ericdc1/Dapper.SimpleCRUD/blob/master/Dapper.SimpleCRUD/SimpleCRUDAsync.cs)。)

谁想编写基本的read / insert / update / delete语句？

现有的Dapper扩展不符合我的理想模式。我希望简单的CRUD操作具有智能默认值而无需额外的任何操作。我还想让模型具有未直接映射到数据库的其他属性。例如 - 在其getter中组合FirstName和LastName的FullName属性 - 而不是将FullName添加到Insert和Update语句中。

在大多数情况下，我希望主键列为Id，但允许覆盖属性。

最后，我希望表名默认匹配类名，但允许覆盖属性。

此扩展添加以下8个帮助程序：

 -  Get(id) - 根据主键获取一条记录
 -  GetList<Type>() - 获取表中所有记录的记录列表
 -  GetList<Type>(where子句的匿名对象) - 获取与where选项匹配的所有记录的列表
 -  GetList<Type>(条件的字符串，带参数的匿名对象) - 获取与条件匹配的所有记录的列表
 -  GetListPaged<Type>(int pagenumber，int itemsperpage，条件字符串，订单字符串，带参数的匿名对象) - 获取与条件匹配的所有记录的分页列表
 -  Insert(entity) - 插入记录并返回新的主键
 -  Update(entity)- 更新记录
 -  Delete<Type>(id) - 删除基于主键的记录
 -  Delete(entity - 根据键入的实体删除记录
 -  DeleteList<Type>(where子句的匿名对象) - 删除与where选项匹配的所有记录
 -  DeleteList<Type>(条件的字符串，带参数的匿名对象) - 删除符合条件的所有记录的列表
 -  RecordCount<Type>(条件的字符串，带参数的匿名对象)-获取计算与条件匹配的所有记录


对于面向.NET 4.5或更高版本的项目，异步操作存在以下8个帮助程序：

 -  GetAsync(id) - 根据主键获取一条记录
 -  GetListAsync<Type>() - 获取表中所有记录的记录列表
 -  GetListAsync<Type>(where子句的匿名对象) - 获取与where选项匹配的所有记录的列表
 -  GetListAsync<Type>(条件的字符串，带参数的匿名对象) - 获取符合条件的所有记录的列表
 -  GetListPagedAsync<Type>(int pagenumber，int itemsperpage，条件字符串，订单字符串，带参数的匿名对象) - 获取与条件匹配的所有记录的分页列表
 -  InsertAsync(entity) - 插入记录并返回新的主键
 -  UpdateAsync(entity) - 更新记录
 -  DeleteAsync<Type>(id) - 删除基于主键的记录
 -  DeleteAsync(entity) - 根据键入的实体删除记录
 -  DeleteListAsync<Type>(where子句的匿名对象) - 删除与where选项匹配的所有记录
 -  DeleteListAsync<Type>(条件的字符串，带参数的匿名对象) - 删除符合条件的所有记录的列表
 -  RecordCountAsync<Type>(条件的字符串，带参数的匿名对象)-gets计算与条件匹配的所有记录

如果你需要更复杂的东西，可以使用Dapper的Query或Execute方法！

注意：所有扩展方法都假定连接已打开，如果连接已关闭，它们将失败。

通过NuGet安装 -  https://nuget.org/packages/Dapper.SimpleCRUD

查看模型生成器[T4模板](https://nuget.org/packages/Dapper.SimpleCRUD.ModelGenerator/)以生成您的POCO。文档位于https://github.com/ericdc1/Dapper.SimpleCRUD/wiki/T4-Template

获取映射到强类型对象的单个记录
-------------------------------------------------- ----------

```CSHARP
 public static T Get<T>(this IDbConnection connection, int id)
```

示例基本用法：

```CSHARP
public class User
{
    public int Id { get; set; }
    public string Name { get; set; }
    public int Age { get; set; }
}
      
var user = connection.Get<User>(1);  
```
执行此SQL的结果
```SQL
Select Id, Name, Age from [User] where Id = 1 
```

更复杂的例子：
```CSHARP
    [Table("Users")]
    public class User
    {
        [Key]
        public int UserId { get; set; }
        [Column("strFirstName")]
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public int Age { get; set; }
    }
    
    var user = connection.Get<User>(1);  
```

执行此SQL的结果
```SQL
Select UserId, strFirstName as FirstName, LastName, Age from [Users] where UserId = @UserID
```

笔记：

 - 可以从Dapper命名空间或System.ComponentModel.DataAnnotations中使用[Key]属性。
 - 可以从Dapper命名空间，System.ComponentModel.DataAnnotations.Schema或System.Data.Linq.Mapping中使用[Table]属性 - 默认情况下，数据库表名称将与模型名称匹配，但可以使用此名称覆盖它。
 -  [Column]属性可以在Dapper命名空间，System.ComponentModel.DataAnnotations.Schema或System.Data.Linq.Mapping中使用 - 默认情况下，列名称将与属性名称匹配，但可以使用此名称覆盖它。您甚至可以在where子句中使用模型属性名称匿名对象，SimpleCRUD将根据列属性生成适当的where子句以匹配数据库

 - 支持GUID(uniqueidentifier)主键(如果没有传入值，则自动填充)

执行查询并将结果映射到强类型列表
-------------------------------------------------- ----------

```CSHARP
public static IEnumerable<T> GetList<T>(this IDbConnection connection)
```

用法示例：

```CSHARP
public class User
{
    public int Id { get; set; }
    public string Name { get; set; }
    public int Age { get; set; }
}
```

```CSHARP
var user = connection.GetList<User>();  
```
结果是
```SQL
Select * from [User]
```

执行具有条件的查询，并将结果映射到强类型列表
-------------------------------------------------- ----------

```CSHARP
public static IEnumerable<T> GetList<T>(this IDbConnection connection, object whereConditions)
```

用法示例：

```CSHARP
public class User
{
    public int Id { get; set; }
    public string Name { get; set; }
    public int Age { get; set; }
}
  
var user = connection.GetList<User>(new { Age = 10 });  
```
结果是
```SQL
Select * from [User] where Age = @Age
```
笔记：
 - 要获取所有记录，请使用空的匿名对象 - new{}
 -  where选项被映射为“where [name] = [value]”
 - 如果你需要> <like等，只需使用manual where子句方法或Dapper的Query方法
 - 默认情况下，select语句将包括类中的所有属性 -  IgnoreSelect属性从select语句中删除项

 
使用where子句执行查询，并将结果映射到强类型列表
-------------------------------------------------- ----------

```CSHARP
public static IEnumerable<T> GetList<T>(this IDbConnection connection, string conditions, object parameters = null)
```

用法示例：

```CSHARP
public class User
{
    public int Id { get; set; }
    public string Name { get; set; }
    public int Age { get; set; }
}
  
var user = connection.GetList<User>("where age = 10 or Name like '%Smith%'");  
```

或带参数

```CSHARP
var encodeForLike = term => term.Replace("[", "[[]").Replace("%", "[%]");
string likename = "%" + encodeForLike("Smith") + "%";
var user = connection.GetList<User>("where age = @Age or Name like @Name", new {Age = 10, Name = likename});  
```
结果是
```SQL
Select * from [User] where age = 10 or Name like '%Smith%'
```

笔记：
 - 这将使用原生SQL语句，所以请注意SQL注入或使用参数
 - 没有什么可以阻止您使用此方法添加order by子句

使用where子句执行查询，并将结果映射到具有Paging的强类型List
-------------------------------------------------- ----------

```CSHARP
public static IEnumerable<T> GetListPaged<T>(this IDbConnection connection, int pageNumber, int rowsPerPage, string conditions, string orderby, object parameters = null)
```

用法示例：

```CSHARP
public class User
{
    public int Id { get; set; }
    public string Name { get; set; }
    public int Age { get; set; }
}
  
var user = connection.GetListPaged<User>(1,10,"where age = 10 or Name like '%Smith%'","Name desc");  
```
结果(SQL Server语法)
```SQL
SELECT * FROM (SELECT ROW_NUMBER() OVER(ORDER BY Name desc) AS PagedNumber, Id, Name, Age FROM [User] where age = 10 or Name like '%Smith%') AS u WHERE PagedNUMBER BETWEEN ((1 - 1) * 10 + 1) AND (1 * 10)
```
或带参数
```CSHARP
var user = connection.GetListPaged<User>(1,10,"where age = @Age","Name desc", new {Age = 10});  
```
结果(SQL Server语法)
```SQL
SELECT * FROM (SELECT ROW_NUMBER() OVER(ORDER BY Name desc) AS PagedNumber, Id, Name, Age FROM [User] where age = 10) AS u WHERE PagedNUMBER BETWEEN ((1 - 1) * 10 + 1) AND (1 * 10)
```

笔记：
 - 这使用原始SQL，因此请注意不要创建SQL注入漏洞或使用“参数”选项
 - 建议使用 https://github.com/martijnboland/MvcPaging 作为您的视图的分页助手
   -  @ Html.Pager(10,1,100) - 每页的项目，页码，总记录


插入记录
-------------------------------------------------- ----------

```CSHARP
public static int Insert(this IDbConnection connection, object entityToInsert)
```

用法示例：

```CSHARP
[表( “用户”)]
[Table("Users")]
public class User
{
   [Key]
   public int UserId { get; set; }
   public string FirstName { get; set; }
   public string LastName { get; set; }
   public int Age { get; set; }

   //Additional properties not in database
   [Editable(false)]
   public string FullName { get { return string.Format("{0} {1}", FirstName, LastName); } }
   public List<User> Friends { get; set; }
   [ReadOnly(true)]
   public DateTime CreatedDate { get; set; }
}

var newId = connection.Insert(new User { FirstName = "User", LastName = "Person",  Age = 10 });  
```
执行此SQL的结果
```SQL
Insert into [Users] (FirstName, LastName, Age) VALUES (@FirstName, @LastName, @Age)
```

笔记：
 - 默认表名称将与类名称匹配 - “表”属性将覆盖此名称
 - 默认主键为Id  -  Key属性会覆盖此项
 - 默认情况下，insert语句将包括类中的所有属性 - 可编辑(false)，只读(true)和IgnoreInsert属性从insert语句中删除项
 - 用只读(true)修饰的属性仅用于选择
 - 复杂类型不包含在insert语句中 - 即使没有Editable属性，这也会使List <User>不在插入中。如果使用Editable(true)装饰它们，则可以包含复杂类型。这对于枚举器很有用。

更新记录
-------------------------------------------------- ----------

```CSHARP
public static int Update(this IDbConnection connection, object entityToUpdate)
```

用法示例：

```CSHARP
[Table("Users")]
public class User
{
   [Key]
   public int UserId { get; set; }
   [Column("strFirstName")]
   public string FirstName { get; set; }
   public string LastName { get; set; }
   public int Age { get; set; }

   //Additional properties not in database
   [Editable(false)]
   public string FullName { get { return string.Format("{0} {1}", FirstName, LastName); } }
   public List<User> Friends { get; set; }
}
connection.Update(entity);

```
执行此SQL的结果
```SQL
Update [Users] Set (strFirstName=@FirstName, LastName=@LastName, Age=@Age) Where ID = @ID
```

笔记：
 - 默认情况下，update语句将包括类中的所有属性 - 可编辑(false)，ReadOnly(true)和IgnoreUpdate属性从update语句中删除项

删除记录
-------------------------------------------------- ----------

```CSHARP
public static int Delete<T>(this IDbConnection connection, int Id)
```

用法示例：

```CSHARP
public class User
{
   public int Id { get; set; }
   public string FirstName { get; set; }
   public string LastName { get; set; }
   public int Age { get; set; }
}
connection.Delete<User>(newid);

```
或者

```CSHARP
public static int Delete<T>(this IDbConnection connection, T entityToDelete)
```

用法示例：

```CSHARP
public class User
{
   public int Id { get; set; }
   public string FirstName { get; set; }
   public string LastName { get; set; }
   public int Age { get; set; }
}

connection.Delete(entity);
```

执行此SQL的结果
```SQL
Delete From [User] Where ID = @ID
```

删除多条记录的条件
-------------------------------------------------- ----------

```CSHARP
public static int DeleteList<T>(this IDbConnection connection, object whereConditions, IDbTransaction transaction = null, int? commandTimeout = null)
```

用法示例：

```CSHARP
connection.DeleteList<User>(new { Age = 10 });
```

使用where子句删除多个记录
-------------------------------------------------- ----------

```CSHARP
public static int DeleteList<T>(this IDbConnection connection, string conditions, object parameters = null, IDbTransaction transaction = null, int? commandTimeout = null)
```

用法示例：

```CSHARP
connection.DeleteList<User>("Where age > 20");
```
或带参数

```CSHARP
connection.DeleteList<User>("Where age > @Age", new {Age = 20});
```

获取记录数
-------------------------------------------------- ----------

```CSHARP
public static int RecordCount<T>(this IDbConnection connection, string conditions = "", object parameters = null)
```

用法示例：

```CSHARP
var count = connection.RecordCount<User>("Where age > 20");
```
或带参数

```CSHARP
var count = connection.RecordCount<User>("Where age > @Age", new {Age = 20});
```

自定义表和列名称解析器
---------------------
您还可以更改表名和列名的格式，首先创建一个实现ITableNameResolver和/或IColumnNameResolver接口的类
```CSHARP
public class CustomResolver : SimpleCRUD.ITableNameResolver, SimpleCRUD.IColumnNameResolver
{
    public string ResolveTableName(Type type)
    {
        return string.Format("tbl_{0}", type.Name);
    }

    public string ResolveColumnName(PropertyInfo propertyInfo)
    {
        return string.Format("{0}_{1}", propertyInfo.DeclaringType.Name, propertyInfo.Name);
    }
}
```
然后在初始化您的应用程序时应用解析器
```CSHARP
    var resolver = new CustomResolver();
    SimpleCRUD.SetTableNameResolver(resolver);
    SimpleCRUD.SetColumnNameResolver(resolver);
```

数据库支持
---------------------
*可以选择更改数据库语言。默认值为Microsoft SQL Server，但可以更改为PostgreSQL或MySQL。我们放弃了.Net Core版本的SQLite支持。
```CSHARP
   SimpleCRUD.SetDialect(SimpleCRUD.Dialect.PostgreSQL);
    
   SimpleCRUD.SetDialect(SimpleCRUD.Dialect.MySQL);
```

属性
---------------------
以下属性可应用于模型中的属性

[Table("YourTableName")]  - 默认情况下，数据库表名称将与模型名称匹配，但可以使用此名称覆盖它。
   
[Column("YourColumnName"]  - 默认情况下，列名称将与属性名称匹配，但可以使用此名称覆盖。您甚至可以在where子句中使用模型属性名称匿名对象，SimpleCRUD将生成适当的where子句以匹配基于列属性的数据库
   
[Key]  - 默认情况下，Id整数字段被视为主键，并从插入中排除。 [Key]属性允许您指定任何Int或Guid作为主键。
   
[Required]  - 默认情况下不插入[Key]属性，因为它应该是数据库自动增加的。如果要在插入期间自己指定值，可以将属性标记为[Key]和[Required]。

[Editable(false)]   - 默认情况下，select，insert和update语句包括类中的所有属性 - 可编辑(false)和属性不包括属性。一个很好的例子是FullName属性，它是从模型中的FirstName和Lastname组合而得的，但FullName字段实际上并不在数据库中。复杂类型不包含在insert语句中 - 即使没有Editable属性，这也会使List保持在插入之外。

[ReadOnly(true)]  - 使用ReadOnly(true)修饰的属性仅用于选择，并从插入和更新中排除。这对于像CreatedDate这样的字段很有用，其中数据库在插入时生成日期而您永远不想修改它。

[IgnoreSelect]  - 从选择中排除属性

[IgnoreInsert]  - 从插入中排除属性

[IgnoreUpdate]  - 从更新中排除属性

[NotMapped]  - 从所有操作中排除属性


你有一个完整的例子清单吗？
---------------------
Dapper.SimpleCRUD在[测试项目]中有一个基本测试套件(https://github.com/ericdc1/dapper.SimpleCRUD/blob/master/Dapper.SimpleCRUDTests/Tests.cs)

