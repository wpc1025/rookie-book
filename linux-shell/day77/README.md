# chage(2019.08.19)

chage命令是用来修改帐号和密码的有效期限。

## 语法

`chage [选项] 用户名`

## 选项

| 选项 | 作用 |
| :--- | :--- |
| -m | 密码可更改的最小天数，为零时代表任何时候都可以更改密码 |
| -M | 密码保持有效的最大天数 |
| -w | 用户密码到期前，提前收到警告信息的天数 |
| -E | 账号到期的日期 |
| -d | 上一次更改的日期 |
| -i | 停滞时期。如果一个密码已过期这些天，那么此账号将不可用 |
| -l | 列出当前的设置。由非特权用户来确定他们的密码或账号何时过期 |

## 实例

```
[rookie@izbp18hovh1qxijbodbas9z ~]$ chage -l rookie
Last password change					: Apr 01, 2018
Password expires					: never
Password inactive					: never
Account expires						: never
Minimum number of days between password change		: 0
Maximum number of days between password change		: 99999
Number of days of warning before password expires	: 7
[rookie@izbp18hovh1qxijbodbas9z ~]$ 
```