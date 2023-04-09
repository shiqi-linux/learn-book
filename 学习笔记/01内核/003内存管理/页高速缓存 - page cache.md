
页高速缓存中的页可能是下面的类型：

- 含有普通文件数据的页。
- 含有目录的页。
- 含有直接从块设备文件（跳过文件系统层）读出的数据的页。
- 含有用户态进程数据的页，但页中的数据已经被交换到磁盘中。
- 属于特殊文件系统文件的页，如shm文件系统。

**页高速缓存中的每个页所包含的数据肯定属于某个文件。这个文件（或者更准确的是文件的索引节点）就称为页的所有者（owner）。**

页高速缓存中的信息单位显然是一个完整的数据页。

# address_space对象

页高速缓存的核心数据结构是address_space对象。它是嵌在页所有者的索引节点对象的数据结构。

![[inode中的address_space.png]]

页所有者的所有页会被链接到address_space对象中。每个页描述符（page）都包括把页链接到页高速缓存的两个字段mapping和index。mapping指向inode的address_space对象，index代表该inode代表所有者以页为单位的偏移。

![[page中的mapping.png]]


页高速缓存可以包含同一磁盘数据的多个副本。比如，访问同一普通文件同一4kb的数据块：

- 读文件，数据包含在普通文件的索引节点所拥有的页中。
- 从文件所在设备文件（磁盘分区）读取块，因此，数据就包含在块设备文件的主索引节点所拥有的页中。

这样，两个不同夜出现了相同的磁盘数据。

address_space对象的字段

![[address_space对象的字段.png]]

重要的字段：

- host：指向对应的inode节点。
- nrpages：所有者（文件）的页总数。

如果页高速缓存中的页的所有者是一个文件，address_space对象就嵌入在vfs索引节点对象的i_data字段中。索引节点的i_mapping字段总是指向索引节点的数据页所有者的address_space对象。address_space对象的host字段指向其所有者的索引节点对象。

```c
struct inode{
...
struct address_space *i_mapping;
...
struct address_space i_data;
...
}
```

## 基树

address_space对象里面page存在哪里呢？存在基数（radix tree）的叶子结点中。

[基树](https://en.wikipedia.org/wiki/Radix_tree) 是一种数据结构。示意图如下：

![[基数.png]]

每个基树深度对应的最大索引值的文件的最大长度：

![[基树深度对应的最大索引值和文件的最大长度.png]]
### 查找页

find_get_page函数：入参为addresss_space和page的offset偏移。

![[find_get_page函数.png]]

### 增加页

add_to_page_cache函数：入参page，address_space，offset，gfp。

![[add_to_page_cache_lru函数.png]]

### 删除页

remove_from_page_cache函数（最新内涵已经删除）




