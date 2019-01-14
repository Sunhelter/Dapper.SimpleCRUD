Dapper.SimpleCRUD  -  Dapper�ļ�CRUD����
========================================
����
--------
<img align =��right��src =��https://raw.githubusercontent.com/ericdc1/Dapper.SimpleCRUD/master/images/SimpleCRUD-200x200.png"alt =��SimpleCRUD��>
Dapper.SimpleCRUD��һ��[�����ļ�]��https://github.com/ericdc1/Dapper.SimpleCRUD/blob/master/Dapper.SimpleCRUD/SimpleCRUD.cs����������ֱ�ӽ��뽫��չIDbConnection�ӿڵ���Ŀ�� �������Ҫ��̬֧�֣�����Ҫ[�����ļ�]��https://github.com/ericdc1/Dapper.SimpleCRUD/blob/master/Dapper.SimpleCRUD/SimpleCRUDAsync.cs������

˭���д������read / insert / update / delete��䣿

���е�Dapper��չ�������ҵ�����ģʽ����ϣ���򵥵�CRUD������������Ĭ��ֵ�����������κβ������һ�����ģ�;���δֱ��ӳ�䵽���ݿ���������ԡ����� - ����getter�����FirstName��LastName��FullName���� - �����ǽ�FullName��ӵ�Insert��Update����С�

�ڴ��������£���ϣ��������ΪId�������������ԡ�

�����ϣ������Ĭ��ƥ�������������������ԡ�

����չ�������8����������

 -  Get��id�� - ����������ȡһ����¼
 -  GetList��lt; Type��gt;���� - ��ȡ�������м�¼�ļ�¼�б�
 -  GetList��lt; Type��gt;��where�Ӿ���������� - ��ȡ��whereѡ��ƥ������м�¼���б�
 -  GetList��lt; Type��gt;���������ַ��������������������� - ��ȡ������ƥ������м�¼���б�
 -  GetListPaged��lt; Type��gt;��int pagenumber��int itemsperpage�������ַ����������ַ��������������������� - ��ȡ������ƥ������м�¼�ķ�ҳ�б�
 - ���루ʵ�壩 - �����¼�������µ�����
 - ���£�ʵ�壩 - ���¼�¼
 - ɾ����lt;���ͣ�gt;��id�� - ɾ�����������ļ�¼
 - ɾ����ʵ�壩 - ���ݼ����ʵ��ɾ����¼
 -  DeleteList��lt; Type��gt;��where�Ӿ���������� - ɾ����whereѡ��ƥ������м�¼
 -  DeleteList��lt; Type��gt;���������ַ��������������������� - ɾ���������������м�¼���б�
 -  RecordCount��lt; Type��gt;���������ַ���������������������-gets����������ƥ������м�¼


��������.NET 4.5����߰汾����Ŀ���첽������������8����������

 -  GetAsync��id�� - ����������ȡһ����¼
 -  GetListAsync��lt; Type��gt;���� - ��ȡ�������м�¼�ļ�¼�б�
 -  GetListAsync��lt; Type��gt;��where�Ӿ���������� - ��ȡ��whereѡ��ƥ������м�¼���б�
 -  GetListAsync��lt; Type��gt;���������ַ��������������������� - ��ȡ�������������м�¼���б�
 -  GetListPagedAsync��lt; Type��gt;��int pagenumber��int itemsperpage�������ַ����������ַ��������������������� - ��ȡ������ƥ������м�¼�ķ�ҳ�б�
 -  InsertAsync��entity�� - �����¼�������µ�����
 -  UpdateAsync��entity�� - ���¼�¼
 -  DeleteAsync��lt; Type��gt;��id�� - ɾ�����������ļ�¼
 -  DeleteAsync��entity�� - ���ݼ����ʵ��ɾ����¼
 -  DeleteListAsync��lt; Type��gt;��where�Ӿ���������� - ɾ����whereѡ��ƥ������м�¼
 -  DeleteListAsync��lt; Type��gt;���������ַ��������������������� - ɾ���������������м�¼���б�
 -  RecordCountAsync��lt; Type��gt;���������ַ���������������������-gets����������ƥ������м�¼

�������Ҫ�����ӵĶ���������ʹ��Dapper��Query��Execute������

ע�⣺������չ�������ٶ������Ѵ򿪣���������ѹرգ����ǽ�ʧ�ܡ�

ͨ��NuGet��װ -  https://nuget.org/packages/Dapper.SimpleCRUD

�鿴ģ��������[T4ģ��]��https://nuget.org/packages/Dapper.SimpleCRUD.ModelGenerator/������������POCO���ĵ�λ��https://github.com/ericdc1/Dapper.SimpleCRUD/wiki/T4-Template

��ȡӳ�䵽ǿ���Ͷ���ĵ�����¼
-------------------------------------------------- ----------

```CSHARP
 public static T Get <T>����IDbConnection���ӣ�int id��
```

ʾ�������÷���

```CSHARP
�������û�
{
    public int Id {get;��; }
    public string Name {get;��; }
    public int Age {get;��; }
}
      
var user = connection.Get <User>��1��;
```
ִ�д�SQL�Ľ��
```SQL
��[�û�]��ѡ��ID�����ƣ����䣬����Id = 1
```

�����ӵ����ӣ�
```CSHARP
    [�� ���û�����]
    �������û�
    {
        [��]
        public int UserId {get;��; }
        [���� ��strFirstName����]
        public string FirstName {get;��; }
        public string LastName {get;��; }
        public int Age {get;��; }
    }
    
    var user = connection.Get <User>��1��;
```

ִ�д�SQL�Ľ��
```SQL
��[Users]��ѡ��UserId��strFirstName��ΪFirstName��LastName��Age������UserId = @UserID
```

�ʼǣ�

 - ���Դ�Dapper�����ռ��System.ComponentModel.DataAnnotations��ʹ��[Key]���ԡ�
 - ���Դ�Dapper�����ռ䣬System.ComponentModel.DataAnnotations.Schema��System.Data.Linq.Mapping��ʹ��[Table]���� - Ĭ������£����ݿ�����ƽ���ģ������ƥ�䣬������ʹ�ô����Ƹ�������
 -  [Column]���Կ�����Dapper�����ռ䣬System.ComponentModel.DataAnnotations.Schema��System.Data.Linq.Mapping��ʹ�� - Ĭ������£������ƽ�����������ƥ�䣬������ʹ�ô����Ƹ�������������������where�Ӿ���ʹ��ģ������������������SimpleCRUD�����������������ʵ���where�Ӿ���ƥ�����ݿ�

 - ֧��GUID��uniqueidentifier�����������û�д���ֵ�����Զ���䣩

ִ�в�ѯ�������ӳ�䵽ǿ�����б�
-------------------------------------------------- ----------

```CSHARP
public static IEnumerable <T> GetList <T>����IDbConnection���ӣ�
```

�÷�ʾ����

```CSHARP
�������û�
{
    public int Id {get;��; }
    public string Name {get;��; }
    public int Age {get;��; }
}
```

```CSHARP
var user = connection.GetList <User>����;
```
�����
```SQL
��[�û�]��ѡ��*
```

ִ�о��������Ĳ�ѯ���������ӳ�䵽ǿ�����б�
-------------------------------------------------- ----------

```CSHARP
public static IEnumerable <T> GetList <T>����IDbConnection���ӣ�����whereConditions��
```

�÷�ʾ����

```CSHARP
�������û�
{
    public int Id {get;��; }
    public string Name {get;��; }
    public int Age {get;��; }
}
  
var user = connection.GetList <User>��new {Age = 10}��;
```
�����
```SQL
��[User]��ѡ��*������Age = @Age
```
�ʼǣ�
 - Ҫ��ȡ���м�¼����ʹ�ÿյ��������� - ��{}
 -  whereѡ�ӳ��Ϊ��where [name] = [value]��
 - �������Ҫ> <like�ȣ�ֻ��ʹ��manual where�Ӿ䷽����Dapper��Query����
 - Ĭ������£�select��佫�������е��������� -  IgnoreSelect���Դ�select�����ɾ����

 
ʹ��where�Ӿ�ִ�в�ѯ���������ӳ�䵽ǿ�����б�
-------------------------------------------------- ----------

```CSHARP
public static IEnumerable <T> GetList <T>����IDbConnection���ӣ��ַ����������������= null��
```

�÷�ʾ����

```CSHARP
�������û�
{
    public int Id {get;��; }
    public string Name {get;��; }
    public int Age {get;��; }
}
  
var user = connection.GetList <User>����where age = 10��Name like'��Smith��'����;
```

�������

```CSHARP
var encodeForLike = term => term.Replace����[������[[]������Replace������������[��]����;
string likename =������+ encodeForLike����Smith����+������;
var user = connection.GetList <User>����where age = @Age��Name like @Name����new {Age = 10��Name = likename}��;
```
�����
```SQL
������= 10��[�û�]��ѡ��*������'��Smith��'����������
```

�ʼǣ�
 - ��ʹ��ԭʼSQL�������ע�ⲻҪ����SQLע��©����ʹ�á�������ѡ��
 - û��ʲô������ֹ��ʹ�ô˷������order by�Ӿ�

ʹ��where�Ӿ�ִ�в�ѯ���������ӳ�䵽����Paging��ǿ����List
-------------------------------------------------- ----------

```CSHARP
public static IEnumerable <T> GetListPaged <T>����IDbConnection���ӣ�int pageNumber��int rowsPerPage���ַ����������ַ���orderby���������= null��
```

�÷�ʾ����

```CSHARP
�������û�
{
    public int Id {get;��; }
    public string Name {get;��; }
    public int Age {get;��; }
}
  
var user = connection.GetListPaged <User>��1,10����where age = 10��Name like'��Smith��'������Name desc����;
```
�����SQL Server���ԣ�
```SQL
SELECT * FROM��SELECT ROW_NUMBER����OVER��ORDER BY Name desc��AS PagedNumber��Id��Name��Age FROM [User] where age = 10��Name like'��Smith��'��AS u WHERE PagedNUMBER BETWEEN����1  -  1�� ��* 10 + 1���ͣ�1 * 10��
```
�������
```CSHARP
var user = connection.GetListPaged <User>��1,10����where age = @Age������Name desc����new {Age = 10}��;
```
�����SQL Server���ԣ�
```SQL
SELECT * FROM��SELECT ROW_NUMBER����OVER��ORDER BY Name desc��AS PagedNumber��Id��Name��Age FROM [User] where age = 10��AS u WHERE PagedNUMBER BETWEEN����1  -  1��* 10 + 1��AND�� 1 * 10��
```

�ʼǣ�
 - ��ʹ��ԭʼSQL�������ע�ⲻҪ����SQLע��©����ʹ�á�������ѡ��
 - ����ʹ��https://github.com/martijnboland/MvcPaging��Ϊ������ͼ�ķ�ҳ����
   -  @ Html.Pager��10,1,100�� - ÿҳ����Ŀ��ҳ�룬�ܼ�¼


�����¼
-------------------------------------------------- ----------

```CSHARP
public static int Insert����IDbConnection���ӣ�����entityToInsert��
```

�÷�ʾ����

```CSHARP
[�� ���û�����]
�������û�
{
   [��]
   public int UserId {get;��; }
   public string FirstName {get;��; }
   public string LastName {get;��; }
   public int Age {get;��; }

   //�������ݿ��е���������
   [�༭���٣�]
   public string FullName {get {return string.Format����{0} {1}����FirstName��LastName��; }}
   public List <User> Friends {get;��; }
   [ֻ�����棩]
   public DateTime CreatedDate {get;��; }
}

var newId = connection.Insert��new User {FirstName =��User����LastName =��Person����Age = 10}��;
```
ִ�д�SQL�Ľ��
```SQL
����[Users]��FirstName��LastName��Age��VALUES��@ FirstName��@ LastName��@ Age��
```

�ʼǣ�
 - Ĭ�ϱ����ƽ���������ƥ�� - �������Խ����Ǵ�����
 - Ĭ������ΪId  -  Key���ԻḲ�Ǵ���
 - Ĭ������£�insert��佫�������е��������� - �ɱ༭��false����ReadOnly��true����IgnoreInsert���Դ�insert�����ɾ����
 - ��ReadOnly��true�����ε����Խ�����ѡ��
 - �������Ͳ�������insert����� - ��ʹû��Editable���ԣ���Ҳ��ʹList <User>���ڲ����С����ʹ��Editable��true��װ�����ǣ�����԰����������͡������ö���������á�

���¼�¼
-------------------------------------------------- ----------

```CSHARP
public static int Update����IDbConnection���ӣ�����entityToUpdate��
```

�÷�ʾ����

```CSHARP
[�� ���û�����]
�������û�
{
   [��]
   public int UserId {get;��; }
   [���� ��strFirstName����]
   public string FirstName {get;��; }
   public string LastName {get;��; }
   public int Age {get;��; }

   //�������ݿ��е���������
   [�༭���٣�]
   public string FullName {get {return string.Format����{0} {1}����FirstName��LastName��; }}
   public List <User> Friends {get;��; }
}
connection.Update��ʵ�壩;

```
ִ�д�SQL�Ľ��
```SQL
����[�û�]���ã�strFirstName = @ FirstName��LastName = @ LastName��Age = @ Age������ID = @ID
```

�ʼǣ�
 - Ĭ������£�update��佫�������е��������� - �ɱ༭��false����ReadOnly��true����IgnoreUpdate���Դ�update�����ɾ����

ɾ����¼
-------------------------------------------------- ----------

```CSHARP
public static int Delete <T>����IDbConnection���ӣ�int Id��
```

�÷�ʾ����

```CSHARP
�������û�
{
   public int Id {get;��; }
   public string FirstName {get;��; }
   public string LastName {get;��; }
   public int Age {get;��; }
}
connection.Delete <�û�>��NEWID��;

```
Ҫô

```CSHARP
public static int Delete <T>����IDbConnection���ӣ�T entityToDelete��
```

�÷�ʾ����

```CSHARP
�������û�
{
   public int Id {get;��; }
   public string FirstName {get;��; }
   public string LastName {get;��; }
   public int Age {get;��; }
}

connection.Delete��ʵ�壩;
```

ִ�д�SQL�Ľ��
```SQL
��[�û�]ɾ��ID = @ID
```

ɾ��������¼������
-------------------------------------------------- ----------

```CSHARP
public static int DeleteList <T>����IDbConnection���ӣ�����whereConditions��IDbTransaction����= null��int��commandTimeout = null��
```

�÷�ʾ����

```CSHARP
connection.DeleteList <User>��new {Age = 10}��;
```

ʹ��where�Ӿ�ɾ�������¼
-------------------------------------------------- ----------

```CSHARP
public static int DeleteList <T>����IDbConnection���ӣ��ַ����������������= null��IDbTransaction����= null��int��commandTimeout = null��
```

�÷�ʾ����

```CSHARP
connection.DeleteList <User>����Where age> 20����;
```
�������

```CSHARP
connection.DeleteList <User>����Where age> @Age����new {Age = 20}��;
```

��ȡ��¼��
-------------------------------------------------- ----------

```CSHARP
public static int RecordCount <T>����IDbConnection���ӣ��ַ�������=�������������= null��
```

�÷�ʾ����

```CSHARP
var count = connection.RecordCount <User>����where age> 20����;
```
�������

```CSHARP
var count = connection.RecordCount <User>����Where age> @Age����new {Age = 20}��;
```

�Զ����������ƽ�����
---------------------
�������Ը��ı����������ĸ�ʽ�����ȴ���һ��ʵ��ITableNameResolver��/��IColumnNameResolver�ӿڵ���
```CSHARP
public class CustomResolver��SimpleCRUD.ITableNameResolver��SimpleCRUD.IColumnNameResolver
{
    public string ResolveTableName��Type type��
    {
        return string.Format����tbl_ {0}����type.Name��;
    }

    public string ResolveColumnName��PropertyInfo propertyInfo��
    {
        return string.Format����{0} _ {1}����propertyInfo.DeclaringType.Name��propertyInfo.Name��;
    }
}
```
Ȼ���ڳ�ʼ������Ӧ�ó���ʱӦ�ý�����
```CSHARP
    var resolver = new CustomResolver����;
    SimpleCRUD.SetTableNameResolver���ֽ⣩;
    SimpleCRUD.SetColumnNameResolver���ֽ⣩;
```

���ݿ�֧��
---------------------
*����ѡ��������ݿⷽ�ԡ�Ĭ��ֵΪMicrosoft SQL Server�������Ը���ΪPostgreSQL��MySQL�����Ƿ�����.Net Core�汾��SQLite֧�֡�
```CSHARP
   SimpleCRUD.SetDialect��SimpleCRUD.Dialect.PostgreSQL��;
    
   SimpleCRUD.SetDialect��SimpleCRUD.Dialect.MySQL��;
```

����
---------------------
�������Կ�Ӧ����ģ���е�����

[Table����YourTableName����]  - Ĭ������£����ݿ�����ƽ���ģ������ƥ�䣬������ʹ�ô����Ƹ�������
   
[Column����YourColumnName��]  - Ĭ������£������ƽ�����������ƥ�䣬������ʹ�ô����Ƹ��ǡ�������������where�Ӿ���ʹ��ģ������������������SimpleCRUD�������ʵ���where�Ӿ���ƥ����������Ե����ݿ�
   
[Key]  - Ĭ������£�Id�����ֶα���Ϊ���������Ӳ������ų��� [Key]����������ָ���κ�Int��Guid��Ϊ������
   
[����]  - Ĭ������²�����[Key]���ԣ���Ϊ��Ӧ�������ݿ��Զ����ӵġ����Ҫ�ڲ����ڼ��Լ�ָ��ֵ�����Խ����Ա��Ϊ[Key]��[Required]��

[�ɱ༭��false��]  - Ĭ������£�select��insert��update���������е��������� - �ɱ༭��false�������Բ��������ԡ�һ���ܺõ�������FullName���ԣ����Ǵ�ģ���е�FirstName��Lastname��϶��õģ���FullName�ֶ�ʵ���ϲ��������ݿ��С��������Ͳ�������insert����� - ��ʹû��Editable���ԣ���Ҳ��ʹList�����ڲ���֮�⡣

[ReadOnly��true��]  - ʹ��ReadOnly��true�����ε����Խ�����ѡ�񣬲��Ӳ���͸������ų����������CreatedDate�������ֶκ����ã��������ݿ��ڲ���ʱ�������ڶ�����Զ�����޸�����

[IgnoreSelect]  - ��ѡ�����ų�����

[IgnoreInsert]  - �Ӳ������ų�����

[IgnoreUpdate]  - �Ӹ������ų�����

[NotMapped]  - �����в������ų�����


����һ�������������嵥��
---------------------
Dapper.SimpleCRUD��[������Ŀ]����һ�����������׼���https://github.com/ericdc1/dapper.SimpleCRUD/blob/master/Dapper.SimpleCRUDTests/Tests.cs��

