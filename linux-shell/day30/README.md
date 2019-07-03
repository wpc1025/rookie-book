# alias命令(2019.07.03)

- `alias`命令用来设置命令的别名，可以将一些较长的命令进行简化。
- `alias`命令的作用只局限于该次登录的操作，若要每次登入都能够使用这些命令别名，则可将相应的`alias`命令存放到bash的初始化文件`/etc/bashrc/`中

## 一、语法格式

`alias [参数]`    

## 二、常用参数

| 参数 | 作用 |
| :--- | :--- |
| -p | 打印已经设置的命令别名 |

## 三、栗子

### 3.1 查看系统已经设置的别名

    [rookie@izbp18hovh1qxijbodbas9z ~]$ alias -p
    alias egrep='egrep --color=auto'
    alias fgrep='fgrep --color=auto'
    alias grep='grep --color=auto'
    alias l.='ls -d .* --color=auto'
    alias ll='ls -l --color=auto'
    alias ls='ls --color=auto'
    alias vi='vim'
    alias which='alias | /usr/bin/which --tty-only --read-alias --show-dot --show-tilde'

### 3.2 给命令设置别名

    [rookie@izbp18hovh1qxijbodbas9z ~]$ alias la='ls -al'
    [rookie@izbp18hovh1qxijbodbas9z ~]$ alias -p
    alias egrep='egrep --color=auto'
    alias fgrep='fgrep --color=auto'
    alias grep='grep --color=auto'
    alias l.='ls -d .* --color=auto'
    alias la='ls -al'
    alias ll='ls -l --color=auto'
    alias ls='ls --color=auto'
    alias vi='vim'
    alias which='alias | /usr/bin/which --tty-only --read-alias --show-dot --show-tilde'


