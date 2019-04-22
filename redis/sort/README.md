# sort 排序命令

根据某种比较规则对一系列元素进行有序的排列。负责执行排序操作的SORT命令可以根据字符串、列表、集合、有序集合、散列这5种键里面存储着的数据，对列表、集合以及有序集合进行排序。

| 命令 | 行为 |
| :--- | :--- |
| SORT | SORT key [BY pattern] [LIMIT offset count] [GET pattern [GET pattern ...]] [ASC|DESC] [ALPHA] [STORE destination] 根据给定的选项，对输入列表、集合或者有序集合进行排序，然后返回或者存储排序的结果 |

