```json
{
  "date": "2022.06.06 23:55",
  "tags": ["mysql"]
}
```


### 安装命令

```bash
apt-get install -y mysql-server
```

### 重置密码
```bash
mysql -u root  #进入mysql命令行
```
```bash
use mysql;   #选择mysql库
```

```sql
update mysql.user set authentication_string=PASSWORD('新密码') where user = 'root';
```

```sql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '新密码';
```

```sql
flush privileges;
```

```bash
service mysql restart  # 重启MySQL服务
```
