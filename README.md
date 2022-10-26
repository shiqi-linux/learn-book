# vim-learn

vim learn ing...

# nerdtree

https://github.com/preservim/nerdtree

https://blog.csdn.net/fengbingchun/article/details/107988884

# cscope

cscope -Rbq：生成索引文件。

cs find c|d|e|f|g|i|s|t name

| 命令 | 解释 |
| ---- | ---- |
| 0 或 s | 查找本C符号(可以跳过注释) |
| 1 或 g	| 查找本定义 |
| 2 或 d	| 查找本函数调用的函数 |
| 3 或 c	| 查找调用本函数的函数 |
| 4 或 t	| 查找本字符串 |
| 6 或 e	| 查找本egrep模式 |
| 7 或 f	| 查找本文件 |
| 8 或 i	| 查找包含本文件的文件 |

# YouCompleteMe

https://github.com/ycm-core/YouCompleteMe

YCM从主页看当前只支持Vundle安装，在通过Vundle安装失败时，可以在vim中输入l查看Vundle安装日志，也可以看通过[知乎链接](https://zhuanlan.zhihu.com/p/509259317
)安装。

安装第三方库失败时，可以单独再使用`git clone`下载。
