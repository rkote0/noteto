## 本地文件系统

扇区是对硬盘而言，块是对文件系统而言

扇区：硬盘的最小单位就是扇区，是真实存在的。一般大小是512字节，这个大小是在硬盘出厂时设置，不能修改。

块：也称为逻辑块，是文件系统层面的概念。文件系统不是一个扇区一个扇区的来读数据，太慢了，它是一个块一个块的读取数据，就是说块（block）是文件系统存取数据的最小单位，一般大小是4KB（这个值可以修改，在格式化分区的时候修改）。

读取一个块，实际上是从硬件设备读取一个或多个扇区，一个block只能存放一个文件内容，无论这个文件有多小。一个文件可能会占用多个block，每读取一个block就会消耗一次磁盘io。如果要提升磁盘io性能，那么尽可能一次io读取更多的数据，但是block也不是越大越好，需要结合业务来设置。

例如：block设置为4K，那么创建大量的1K小文件后，磁盘空间会被大量浪费。一个文件占用一个block，100G的小文件（都是1K大小），那么会占用400G的空间，浪费300G，可以做个实验验证下：

- 分配一个100M的磁盘，然后格式化成ext4文件系统，block size默认为1K，并挂载

```bash
$ mkfs.ext4 /dev/sdj
$ mount /dev/sdj /mnt/test3
```

- 分配一个100M的磁盘，然后格式化成ext4文件系统，block size设置为4K，并挂载

```bash
$ mkfs.ext4 -b 4096 /dev/sdk
$ mount /dev/sdk /mnt/test4
```

# 创建存储小文件的文件系统

创建 ext4 文件系统时，我们可以使用 `-T small` 参数来获得更多的 inode，从而来优化对小文件的存储性能。