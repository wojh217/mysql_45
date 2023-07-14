查看字段

```
desc 表名;
show columns from 表名;
```

查看建表语句：

```
show create table 表名;
```

通过系统表查看：

```
use information_schema
select * from columns where table_name='表名';
```

