# groupadd命令(2019.07.30)

用于创建一个新的工作组，新工作组的信息将被添加到系统文件中

## 一、语法

`groupadd (选项) (参数)`

## 二、选项

| 选项 | 作用 |
| :--- | :--- |
| -g | 指定新建工作组的ID |
| -r | 创建系统工作组，系统工作组ID小于500 |
| -K | 覆盖配置文件“/etc/login.defs” |
| -o | 允许添加组ID不唯一的工作组 |


## 三、参数

组名： 指定新建工作组的组名

## 四、实例

### 4.1 建立一个新组，并设置组ID加入系统

```
[root@izbp18hovh1qxijbodbas9z ~]# groupadd -g 344 wpc
[root@izbp18hovh1qxijbodbas9z ~]# 
```