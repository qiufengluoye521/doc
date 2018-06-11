# 使用TFTP下载
1. 使用tftp 

   tftp boot -> tftpd32.exe ->选择主机地址，目标文件目录

   ![tftp工具](/pic/tftp.jpg)

2. 设置服务器IP，和开发板IP

      set ipaddr 192.168.1.17->set serverip 192.168.1.12->save

3. 从服务器下载程序
    tftp 30000000 uIamge

4. mtdpart 显示有哪些分区

5. nand erase kernel 

6. nand write.jffs2 30000000 kernel

    以上为烧写内核

7. 重新选择yaffs2目录
8. tftp 30000000 fs_qtopia.yaffs2
9. nand erase root
10. nand write.yaffs 260000 2f7640(offset size)
    nand write.yaffs 30000000 root 不能直接写root，文件只有1M，用root的话，会把1M+63M全部覆盖
    
# 使用NFS下载
1. nfs 30000000 192.168.1.123:/work/nfs_root/uImage
    /work/nfs/nfs_root/uImage 为什么是这个目录呢， cat /etc/exports 查看这个文件，这个文件中指定了只有某些目录可以挂载。

    ![](/pic/etc_exports.jpg)

2. nand erase kenel 

3. nand write.jffs2 30000000 kernel
    (1下载 2 擦除 3烧录)
    以上用于kernel 下面烧录文件系统

4. nfs 30000000 192.168.1.123:/work/nfs_root/fs_qtopia.yaff2

5. nand erase root

6. nand write.yaffs 30000000 260000 2f7640

    

    

# linux 下的dnw使用方法

1. 把linux下的dnw应用程序放到/bin目录 
    sudo cp /work/nfs_root/dnw /bin

2. 添加超级权限

​    sudo chmod +x /bin/dnw

​    sudo chmod +s /bin/dnw

3. 先设置成u-boot模式，如果使用的是虚拟机，让vmwawre位于前台，再接线 

​    lsusb看一下usb设备
4. 在u-boot界面输入k,然后在linux下输入sudo dnw arch/arm/boot/uImage(如果只有一个dnw，则不用加sudo)
5.  文件系统方法一样
  

  ​    
