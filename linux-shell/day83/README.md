# dig(2019.08.25)

常用的域名查询工具，可以用来测试域名系统工作是否正常

## 一、语法

`dig (选项) (参数)`

## 二、选项

```
@<服务器地址>：指定进行域名解析的域名服务器；
-b<ip地址>：当主机具有多个IP地址，指定使用本机的哪个IP地址向域名服务器发送域名查询请求；
-f<文件名称>：指定dig以批处理的方式运行，指定的文件中保存着需要批处理查询的DNS任务信息；
-P：指定域名服务器所使用端口号；
-t<类型>：指定要查询的DNS数据类型；
-x<IP地址>：执行逆向域名查询；
-4：使用IPv4；
-6：使用IPv6；
-h：显示指令帮助信息。
```

## 三、参数

- 主机：指定要查询域名主机
- 查询类型：指定DNS查询的类型；
- 查询类：指定查询DNS的class；
- 查询选项：指定查询选项。

## 四、示例

```
bogon:~ hanfeige$ dig www.mrrookie.com

; <<>> DiG 9.10.6 <<>> www.mrrookie.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 44849
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 0

;; QUESTION SECTION:
;www.mrrookie.com.		IN	A

;; ANSWER SECTION:
www.mrrookie.com.	1	IN	A	47.98.124.76

;; Query time: 73 msec
;; SERVER: 192.168.1.1#53(192.168.1.1)
;; WHEN: Sun Aug 25 21:52:46 CST 2019
;; MSG SIZE  rcvd: 50

bogon:~ hanfeige$
```
