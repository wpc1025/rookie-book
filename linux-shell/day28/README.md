# watch命令(2019.07.01)

- 将已存在文件的时间标签更新为系统当前的时间，他们的数据原封不动的保留下来
- 用来创建新的空白文件

## 一、语法

`touch [选项] (参数)`

## 二、选项

| 参数 | 作用 |
| :--- | :--- |
| -a | 只更改存取时间 |
| -c | 不建立任何文件 |
| -d | 使用指定的日期时间，而非现在的时间 |
| -m | 只更改变动时间 |

## 三、栗子

	bogon:~ hanfeige$ touch test.txt
	bogon:~ hanfeige$ ls -al | grep test.txt
	-rw-r--r--   1 hanfeige  staff      0  7  1 22:05 test.txt
	bogon:~ hanfeige$ 

