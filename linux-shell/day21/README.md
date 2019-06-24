# fc命令(2019.06.24)

显示或执行历史列表中的命令

## 一、语法格式

`fc [参数]`

## 二、常用参数

| 参数 | 作用 |
| :--- | :--- |
| -e <文本编辑器> | 指定用来编辑命令的文本编辑器，默认是vi |
| -n | 显示命令历史时不显示命令序号 |
| -l | 列出第一条和最后一天命令范围内的历史命令，如果不跟命令范围则默认显示最近使用过的16条历史命令 |
| -r | 反序显示所有命令历史 |
| -s <命令名> | 从历史命令中当前位置往前找到指定命令，并执行 |

## 三、栗子

### 3.1 显示历史命令列表（默认打印最近的16条历史命令）

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ fc -l
    991	 df -h /home/rookie/
    992	 df -t ext4
    993	 df -ht ext4
    994	 free
    995	 free -m
    996	 free -k
    997	 free -t
    998	 free -gt 
    999	 free -s 10
    1000	 man free
    1001	 fc -l
    1002	 find ~/ -name page-treeview
    1003	 su - root
    1004	 man fc
    1005	 fc --help
    1006	 help fc
    [rookie@iZbp18hovh1qxijbodbas9Z 
    

### 3.2 指定使用ex文本编辑器编辑命令：

    [rookie@iZbp18hovh1qxijbodbas9Z ~]$ fc -e ex
    
