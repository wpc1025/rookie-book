# comm命令(2019.08.07)

用来比较两个已排过序的文件

若未指定任何参数，结果分3行显示：
- 第一列仅是在第1个文件中出现过的列
- 第二列是仅在第2个文件中出现过的列
- 第三列是在第1个和第2个文件里都出现过的列

## 一、语法

`comm [-123][--help][--version][第1个文件][第2个文件]`

## 二、参数

| 参数 | 作用 |
| :--- | :--- |
| -1 | 不显示只在第1个文件里出现过的列 |
| -2 | 不显示只在第2个文件里出现过的列 |
| -3 | 不显示在第1和第2个文件中同时出现的列 |


## 三、实例

```
[root@izbp18hovh1qxijbodbas9z ~]# 
[root@izbp18hovh1qxijbodbas9z ~]# cat aaa.txt 
aaa 
bbb 
ccc 
ddd 
eee 
111 
222
[root@izbp18hovh1qxijbodbas9z ~]# cat bbb.txt 
bbb 
ccc 
aaa 
hhh 
ttt 
jjj
[root@izbp18hovh1qxijbodbas9z ~]# comm aaa.txt bbb.txt 
aaa 
		bbb 
		ccc 
comm: file 2 is not in sorted order
	aaa 
ddd 
eee 
comm: file 1 is not in sorted order
111 
222
	hhh 
	ttt 
	jjj
[root@izbp18hovh1qxijbodbas9z ~]# 
```

第一列代表`aaa.txt`包含的内容  
第二列代表`bbb.txt`包含的内容  
第三列代表在`aaa.txt`和`bbb.txt`中相同的行