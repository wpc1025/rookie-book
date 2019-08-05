# host命令(2019.08.05)

host命令是常用的分析域名查询工具，用来测试域名系统工作是否正常

## 一、语法

`host (选项) (参数)`

## 二、选项

| 选项 | 作用 |
| :--- | :--- |
| -a | 显示详细的DNS信息 |
| -c <类型> | 指定查询类型，默认值为"IN" |
| -C | 查询指定主机的完整SOA记录 |
| -r | 在查询域名时，不使用递归的查询方式 |
| -t <类型> | 指定查询的域名信息类型 |
| -w | 如果域名服务器没有给出应答，则总是等待，直到域名服务器给出应答 |
| -W <时间> | 指定域名查询的最长时间，如果在指定时间内域名服务器没有给出应答信息，则推出指令 |

## 三、参数

主机：指定要查询信息的主机信息


## 四、示例

```
bogon:~ hanfeige$ host www.baidu.com
www.baidu.com is an alias for www.a.shifen.com.
www.a.shifen.com has address 61.135.169.121
www.a.shifen.com has address 61.135.169.125
;; connection timed out; no servers could be reached
bogon:~ hanfeige$ 
```
