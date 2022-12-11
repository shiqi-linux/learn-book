# seq_file

procfs文件系统的read方法使用的缓存是一个page，也就是4K。如果想用更大的空间，内核提供了seq_file方法。

seq_file的方法和demo：https://linux.die.net/lkmpg/x861.html

