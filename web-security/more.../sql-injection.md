# SQL Injection

由于把用户输入的字符串拼接到SQL语句之中执行，造成意外的语法效果导致的问题。

例如

```javascript
let querySQL = `
    SELECT *
    FROM user
    WHERE username='${username}'
    AND psw='${password}'
`;
```

正常比如说

```sql
SELECT * FROM user WHERE username='amy'               AND psw='secret'
```

如果攻击者提交`username=admin' --`，`password=anything`，这个SQL语句就会变成

```sql
SELECT * FROM user WHERE username='admin' --' AND psw='anything'
```

其中 `--`是注释的意思…，所以密码的限制就失效了，能直接查出admin的信息来。

> **SQL注入的本质:数据和代码未分离，即数据当做了代码来执行。**

### 防御

> **严格限制Web应用的数据库的操作权限**，给此用户提供仅仅能够满足其工作的最低权限，从而最大限度的减少注入攻击对数据库的危害
>
> **后端代码检查输入的数据是否符合预期**，严格限制变量的类型，例如使用正则表达式进行一些匹配处理。
>
> **对进入数据库的特殊字符（'，"，\，&lt;，&gt;，&，\*，; 等）进行转义处理，或编码转换**。基本上所有的后端语言都有对字符串进行转义处理的方法，比如 lodash 的 lodash.\_escapehtmlchar 库。
>
> **所有的查询语句建议使用数据库提供的参数化查询接口**，参数化的语句使用参数而不是将用户输入变量嵌入到 SQL 语句中，即不要直接拼接 SQL 语句。例如 Node.js 中的 mysqljs 库的 query 方法中的 ? 占位参数。
>
> 作者：浪里行舟  
> 链接：https://juejin.im/post/6844903772930441230  
> 来源：掘金  
> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

