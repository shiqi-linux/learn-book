
文件对象（file object）：存放打开文件与进程之间交互的有关信息。这类信息仅当进程访问文件期间存在于内核内存中。

进程与vfs对象之间的交互：

![[进程与vfs交互.png]]

文件对象是在文件被打开时创建的，由一个file结构组成。

文件对象在磁盘上没有对应的映像，即它是一个纯内存概念对象，因此file结构中没有“脏”字段表示文件对象是否被修改。

file结构对象：

![[file数据结构.png]]

文件指针指的是文件当前的位置，文件下一个操作将在该位置发生。**由于几个进程可能同时访问同一文件，因此文件指针必须存放在文件对象而不是索引节点对象中。**

文件对象的分配来自slab，存放在filp_cachep变量中。

```c
void __init files_init(void)

{

	filp_cachep = kmem_cache_create("filp", sizeof(struct file), 0,

	SLAB_HWCACHE_ALIGN | SLAB_PANIC | SLAB_ACCOUNT, NULL);

	percpu_counter_init(&nr_files, 0, GFP_KERNEL);

}
```

系统中最大文件数由max_files指定。

*todo*:超过max_files会怎么样呢？文章中写道即使max_files被分配，超级用户也可以获得一个文件对象。

每个超级块对象把文件对象链表的头存放s_files字段中，因此，属于不同文件系统的文件对象就包含在不同的链表中。链表中分别指向前一个元素和后一个元素的指针都存放在文件对象的f_list字段中。*todo*：一个进程打开多个文件系统中的文件，那么超级块记录的是该文件系统打开的文件？

文件对象的f_count字段是一个引用计数器：它记录使用文件对象的进程数（以CLONE_FILES标志创建的轻量级进程共享打开文件表，因此他们可以使用相同的文件对象）。当内核本身使用该文件对象时也要增加计数值，例如，把对象插入链表或者发出dup（）系统调用时。

进程描述符task_struct中的files字段表示进程当前打开的文件，类型为files_struct。

files_struct结构中的字段：

