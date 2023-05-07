
格式化mtd：

```bash
./ubiformat /dev/mtd0 -y
```

attach到mtd：

```bash
./ubiattach /dev/ubi_ctrl -m 0 -b 20
```

创建卷宗：

```bash
./ubimkvol /dev/ubi0 -s 8388608 -N ubitest
```

挂载命令：

```bash
mount -t ubifs /dev/ubi0_0 /mnt/
```


