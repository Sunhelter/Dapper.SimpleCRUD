Dapper.SimpleCRUD  -  Dapper�ļ�CRUD����
========================================
����
--------
<img  align="right" src="https://raw.githubusercontent.com/ericdc1/Dapper.SimpleCRUD/master/images/SimpleCRUD-200x200.png" alt="SimpleCRUD">
Dapper.SimpleCRUD��һ��[�����ļ�](https://github.com/ericdc1/Dapper.SimpleCRUD/blob/master/Dapper.SimpleCRUD/SimpleCRUD.cs)��������ֱ�ӽ��뽫��չIDbConnection�ӿڵ���Ŀ�� (�����Ҫ��̬֧�֣�����Ҫ[�����ļ�](https://github.com/ericdc1/Dapper.SimpleCRUD/blob/master/Dapper.SimpleCRUD/SimpleCRUDAsync.cs)��)

˭���д������read / insert / update / delete��䣿

���е�Dapper��չ�������ҵ�����ģʽ����ϣ���򵥵�CRUD������������Ĭ��ֵ�����������κβ������һ�����ģ�;���δֱ��ӳ�䵽���ݿ���������ԡ����� - ����getter�����FirstName��LastName��FullName���� - �����ǽ�FullName��ӵ�Insert��Update����С�

�ڴ��������£���ϣ��������ΪId�������������ԡ�

�����ϣ������Ĭ��ƥ�������������������ԡ�

����չ�������8����������

 -  Get(id) - ����������ȡһ����¼
 -  GetList<Type>() - ��ȡ�������м�¼�ļ�¼�б�
 -  GetList<Type>(where�Ӿ����������) - ��ȡ��whereѡ��ƥ������м�¼���б�
 -  GetList<Type>(�������ַ���������������������) - ��ȡ������ƥ������м�¼���б�
 -  GetListPaged<Type>(int pagenumber��int itemsperpage�������ַ����������ַ���������������������) - ��ȡ������ƥ������м�¼�ķ�ҳ�б�
 -  Insert(entity) - �����¼�������µ�����
 -  Update(entity)- ���¼�¼
 -  Delete<Type>(id) - ɾ�����������ļ�¼
 -  Delete(entity - ���ݼ����ʵ��ɾ����¼
 -  DeleteList<Type>(where�Ӿ����������) - ɾ����whereѡ��ƥ������м�¼
 -  DeleteList<Type>(�������ַ���������������������) - ɾ���������������м�¼���б�
 -  RecordCount<Type>(�������ַ���������������������)-��ȡ����������ƥ������м�¼


��������.NET 4.5����߰汾����Ŀ���첽������������8����������

 -  GetAsync(id) - ����������ȡһ����¼
 -  GetListAsync<Type>() - ��ȡ�������м�¼�ļ�¼�б�
 -  GetListAsync<Type>(where�Ӿ����������) - ��ȡ��whereѡ��ƥ������м�¼���б�
 -  GetListAsync<Type>(�������ַ���������������������) - ��ȡ�������������м�¼���б�
 -  GetListPagedAsync<Type>(int pagenumber��int itemsperpage�������ַ����������ַ���������������������) - ��ȡ������ƥ������м�¼�ķ�ҳ�б�
 -  InsertAsync(entity) - �����¼�������µ�����
 -  UpdateAsync(entity) - ���¼�¼
 -  DeleteAsync<Type>(id) - ɾ�����������ļ�¼
 -  DeleteAsync(entity) - ���ݼ����ʵ��ɾ����¼
 -  DeleteListAsync<Type>(where�Ӿ����������) - ɾ����whereѡ��ƥ������м�¼
 -  DeleteListAsync<Type>(�������ַ���������������������) - ɾ���������������м�¼���б�
 -  RecordCountAsync<Type>(�������ַ���������������������)-gets����������ƥ������м�¼

�������Ҫ�����ӵĶ���������ʹ��Dapper��Query��Execute������

ע�⣺������չ�������ٶ������Ѵ򿪣���������ѹرգ����ǽ�ʧ�ܡ�

ͨ��NuGet��װ -  https://nuget.org/packages/Dapper.SimpleCRUD

�鿴ģ��������[T4ģ��](https://nuget.org/packages/Dapper.SimpleCRUD.ModelGenerator/)����������POCO���ĵ�λ��https://github.com/ericdc1/Dapper.SimpleCRUD/wiki/T4-Template

��ȡӳ�䵽ǿ���Ͷ���ĵ�����¼
-------------------------------------------------- ----------

```CSHARP
 public static T Get<T>(this IDbConnection connection, int id)
```

ʾ�������÷���

```CSHARP
public class User
{
    public int Id { get; set; }
    public string Name { get; set; }
    public int Age { get; set; }
}
      
var user = connection.Get<User>(1);  
```
ִ�д�SQL�Ľ��
```SQL
Select Id, Name, Age from [User] where Id = 1 
```

�����ӵ����ӣ�
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

ִ�д�SQL�Ľ��
```SQL
Select UserId, strFirstName as FirstName, LastName, Age from [Users] where UserId = @UserID
```

�ʼǣ�

 - ���Դ�Dapper�����ռ��System.ComponentModel.DataAnnotations��ʹ��[Key]���ԡ�
 - ���Դ�Dapper�����ռ䣬System.ComponentModel.DataAnnotations.Schema��System.Data.Linq.Mapping��ʹ��[Table]���� - Ĭ������£����ݿ�����ƽ���ģ������ƥ�䣬������ʹ�ô����Ƹ�������
 -  [Column]���Կ�����Dapper�����ռ䣬System.ComponentModel.DataAnnotations.Schema��System.Data.Linq.Mapping��ʹ�� - Ĭ������£������ƽ�����������ƥ�䣬������ʹ�ô����Ƹ�������������������where�Ӿ���ʹ��ģ������������������SimpleCRUD�����������������ʵ���where�Ӿ���ƥ�����ݿ�

 - ֧��GUID(uniqueidentifier)����(���û�д���ֵ�����Զ����)

ִ�в�ѯ�������ӳ�䵽ǿ�����б�
-------------------------------------------------- ----------

```CSHARP
public static IEnumerable<T> GetList<T>(this IDbConnection connection)
```

�÷�ʾ����

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
�����
```SQL
Select * from [User]
```

ִ�о��������Ĳ�ѯ���������ӳ�䵽ǿ�����б�
-------------------------------------------------- ----------

```CSHARP
public static IEnumerable<T> GetList<T>(this IDbConnection connection, object whereConditions)
```

�÷�ʾ����

```CSHARP
public class User
{
    public int Id { get; set; }
    public string Name { get; set; }
    public int Age { get; set; }
}
  
var user = connection.GetList<User>(new { Age = 10 });  
```
�����
```SQL
Select * from [User] where Age = @Age
```
�ʼǣ�
 - Ҫ��ȡ���м�¼����ʹ�ÿյ��������� - new{}
 -  whereѡ�ӳ��Ϊ��where [name] = [value]��
 - �������Ҫ> <like�ȣ�ֻ��ʹ��manual where�Ӿ䷽����Dapper��Query����
 - Ĭ������£�select��佫�������е��������� -  IgnoreSelect���Դ�select�����ɾ����

 
ʹ��where�Ӿ�ִ�в�ѯ���������ӳ�䵽ǿ�����б�
-------------------------------------------------- ----------

```CSHARP
public static IEnumerable<T> GetList<T>(this IDbConnection connection, string conditions, object parameters = null)
```

�÷�ʾ����

```CSHARP
public class User
{
    public int Id { get; set; }
    public string Name { get; set; }
    public int Age { get; set; }
}
  
var user = connection.GetList<User>("where age = 10 or Name like '%Smith%'");  
```

�������

```CSHARP
var encodeForLike = term => term.Replace("[", "[[]").Replace("%", "[%]");
string likename = "%" + encodeForLike("Smith") + "%";
var user = connection.GetList<User>("where age = @Age or Name like @Name", new {Age = 10, Name = likename});  
```
�����
```SQL
Select * from [User] where age = 10 or Name like '%Smith%'
```

�ʼǣ�
 - �⽫ʹ��ԭ��SQL��䣬������ע��SQLע���ʹ�ò���
 - û��ʲô������ֹ��ʹ�ô˷������order by�Ӿ�

ʹ��where�Ӿ�ִ�в�ѯ���������ӳ�䵽����Paging��ǿ����List
-------------------------------------------------- ----------

```CSHARP
public static IEnumerable<T> GetListPaged<T>(this IDbConnection connection, int pageNumber, int rowsPerPage, string conditions, string orderby, object parameters = null)
```

�÷�ʾ����

```CSHARP
public class User
{
    public int Id { get; set; }
    public string Name { get; set; }
    public int Age { get; set; }
}
  
var user = connection.GetListPaged<User>(1,10,"where age = 10 or Name like '%Smith%'","Name desc");  
```
���(SQL Server�﷨)
```SQL
SELECT * FROM (SELECT ROW_NUMBER() OVER(ORDER BY Name desc) AS PagedNumber, Id, Name, Age FROM [User] where age = 10 or Name like '%Smith%') AS u WHERE PagedNUMBER BETWEEN ((1 - 1) * 10 + 1) AND (1 * 10)
```
�������
```CSHARP
var user = connection.GetListPaged<User>(1,10,"where age = @Age","Name desc", new {Age = 10});  
```
���(SQL Server�﷨)
```SQL
SELECT * FROM (SELECT ROW_NUMBER() OVER(ORDER BY Name desc) AS PagedNumber, Id, Name, Age FROM [User] where age = 10) AS u WHERE PagedNUMBER BETWEEN ((1 - 1) * 10 + 1) AND (1 * 10)
```

�ʼǣ�
 - ��ʹ��ԭʼSQL�������ע�ⲻҪ����SQLע��©����ʹ�á�������ѡ��
 - ����ʹ�� https://github.com/martijnboland/MvcPaging ��Ϊ������ͼ�ķ�ҳ����
   -  @ Html.Pager(10,1,100) - ÿҳ����Ŀ��ҳ�룬�ܼ�¼


�����¼
-------------------------------------------------- ----------

```CSHARP
public static int Insert(this IDbConnection connection, object entityToInsert)
```

�÷�ʾ����

```CSHARP
[��( ���û���)]
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
ִ�д�SQL�Ľ��
```SQL
Insert into [Users] (FirstName, LastName, Age) VALUES (@FirstName, @LastName, @Age)
```

�ʼǣ�
 - Ĭ�ϱ����ƽ���������ƥ�� - �������Խ����Ǵ�����
 - Ĭ������ΪId  -  Key���ԻḲ�Ǵ���
 - Ĭ������£�insert��佫�������е��������� - �ɱ༭(false)��ֻ��(true)��IgnoreInsert���Դ�insert�����ɾ����
 - ��ֻ��(true)���ε����Խ�����ѡ��
 - �������Ͳ�������insert����� - ��ʹû��Editable���ԣ���Ҳ��ʹList <User>���ڲ����С����ʹ��Editable(true)װ�����ǣ�����԰����������͡������ö���������á�

���¼�¼
-------------------------------------------------- ----------

```CSHARP
public static int Update(this IDbConnection connection, object entityToUpdate)
```

�÷�ʾ����

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
ִ�д�SQL�Ľ��
```SQL
Update [Users] Set (strFirstName=@FirstName, LastName=@LastName, Age=@Age) Where ID = @ID
```

�ʼǣ�
 - Ĭ������£�update��佫�������е��������� - �ɱ༭(false)��ReadOnly(true)��IgnoreUpdate���Դ�update�����ɾ����

ɾ����¼
-------------------------------------------------- ----------

```CSHARP
public static int Delete<T>(this IDbConnection connection, int Id)
```

�÷�ʾ����

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
����

```CSHARP
public static int Delete<T>(this IDbConnection connection, T entityToDelete)
```

�÷�ʾ����

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

ִ�д�SQL�Ľ��
```SQL
Delete From [User] Where ID = @ID
```

ɾ��������¼������
-------------------------------------------------- ----------

```CSHARP
public static int DeleteList<T>(this IDbConnection connection, object whereConditions, IDbTransaction transaction = null, int? commandTimeout = null)
```

�÷�ʾ����

```CSHARP
connection.DeleteList<User>(new { Age = 10 });
```

ʹ��where�Ӿ�ɾ�������¼
-------------------------------------------------- ----------

```CSHARP
public static int DeleteList<T>(this IDbConnection connection, string conditions, object parameters = null, IDbTransaction transaction = null, int? commandTimeout = null)
```

�÷�ʾ����

```CSHARP
connection.DeleteList<User>("Where age > 20");
```
�������

```CSHARP
connection.DeleteList<User>("Where age > @Age", new {Age = 20});
```

��ȡ��¼��
-------------------------------------------------- ----------

```CSHARP
public static int RecordCount<T>(this IDbConnection connection, string conditions = "", object parameters = null)
```

�÷�ʾ����

```CSHARP
var count = connection.RecordCount<User>("Where age > 20");
```
�������

```CSHARP
var count = connection.RecordCount<User>("Where age > @Age", new {Age = 20});
```

�Զ����������ƽ�����
---------------------
�������Ը��ı����������ĸ�ʽ�����ȴ���һ��ʵ��ITableNameResolver��/��IColumnNameResolver�ӿڵ���
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
Ȼ���ڳ�ʼ������Ӧ�ó���ʱӦ�ý�����
```CSHARP
    var resolver = new CustomResolver();
    SimpleCRUD.SetTableNameResolver(resolver);
    SimpleCRUD.SetColumnNameResolver(resolver);
```

���ݿ�֧��
---------------------
*����ѡ��������ݿ����ԡ�Ĭ��ֵΪMicrosoft SQL Server�������Ը���ΪPostgreSQL��MySQL�����Ƿ�����.Net Core�汾��SQLite֧�֡�
```CSHARP
   SimpleCRUD.SetDialect(SimpleCRUD.Dialect.PostgreSQL);
    
   SimpleCRUD.SetDialect(SimpleCRUD.Dialect.MySQL);
```

����
---------------------
�������Կ�Ӧ����ģ���е�����

[Table("YourTableName")]  - Ĭ������£����ݿ�����ƽ���ģ������ƥ�䣬������ʹ�ô����Ƹ�������
   
[Column("YourColumnName"]  - Ĭ������£������ƽ�����������ƥ�䣬������ʹ�ô����Ƹ��ǡ�������������where�Ӿ���ʹ��ģ������������������SimpleCRUD�������ʵ���where�Ӿ���ƥ����������Ե����ݿ�
   
[Key]  - Ĭ������£�Id�����ֶα���Ϊ���������Ӳ������ų��� [Key]����������ָ���κ�Int��Guid��Ϊ������
   
[Required]  - Ĭ������²�����[Key]���ԣ���Ϊ��Ӧ�������ݿ��Զ����ӵġ����Ҫ�ڲ����ڼ��Լ�ָ��ֵ�����Խ����Ա��Ϊ[Key]��[Required]��

[Editable(false)]   - Ĭ������£�select��insert��update���������е��������� - �ɱ༭(false)�����Բ��������ԡ�һ���ܺõ�������FullName���ԣ����Ǵ�ģ���е�FirstName��Lastname��϶��õģ���FullName�ֶ�ʵ���ϲ��������ݿ��С��������Ͳ�������insert����� - ��ʹû��Editable���ԣ���Ҳ��ʹList�����ڲ���֮�⡣

[ReadOnly(true)]  - ʹ��ReadOnly(true)���ε����Խ�����ѡ�񣬲��Ӳ���͸������ų����������CreatedDate�������ֶκ����ã��������ݿ��ڲ���ʱ�������ڶ�����Զ�����޸�����

[IgnoreSelect]  - ��ѡ�����ų�����

[IgnoreInsert]  - �Ӳ������ų�����

[IgnoreUpdate]  - �Ӹ������ų�����

[NotMapped]  - �����в������ų�����


����һ�������������嵥��
---------------------
Dapper.SimpleCRUD��[������Ŀ]����һ�����������׼�(https://github.com/ericdc1/dapper.SimpleCRUD/blob/master/Dapper.SimpleCRUDTests/Tests.cs)

