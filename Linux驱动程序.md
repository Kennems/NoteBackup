# 过程

1. 编译驱动并生成`xxx.ko`：

```
make
```

1. 编译上层应用程序：

```
make AppTest
```

1. 加载驱动：

```
sudo insmod hello.ko
```

1. 查看打印的信息：

```
dmesg
```

1. 上层应用调用驱动：

```
sudo ./AppTest /dev/device_test
```

1. 卸载驱动：

```
sudo rmmod hello
```

# chrdev_fops

## chrdev_fops.c

```c
#include <linux/init.h>
#include <linux/module.h>
#include <linux/fs.h>
#include <linux/kdev_t.h>
#include <linux/cdev.h>
#include <linux/uaccess.h>


#define MOD_INC_USE_COUNT try_module_get(THIS_MODULE)
#define MOD_DEC_USE_COUNT module_put(THIS_MODULE)

//#define DEMO_MAJOR 21
#define BUFFSIZE 256
#define DEMO_MINOR 0
/* the counter to record the char number in wbuff  */
int count;
int i;
/* buffer pointers, the buffer will be allocated in device init */
char wbuff[BUFFSIZE],rbuff[BUFFSIZE];
static int chrdev_open(struct inode *inode, struct file *file)
{
	printk("This is chrdev_open \n");
	return 0;
}

//写入数据
static ssize_t chrdev_write(struct file *file,const char __user *buf,size_t size,loff_t *off)
{
    // 从缓冲区读取
	printk("This is chrdev_read \n");
    
    // 检查写入的数据量是否超过缓冲区大小
    if (size > BUFFSIZE)
        size = BUFFSIZE;

    // 从用户空间拷贝数据到内核缓冲区 wbuff
    copy_from_user(wbuff, buf, size); // copy_from_user(to, from, size) 内置函数

    // 更新缓冲区中数据的字节数
    count = size;

    // 返回实际写入的字节数
    return size;
}
//读出数据
static ssize_t chrdev_read(struct file *filp, char __user *buf, size_t size, loff_t *off)
{
    // 同样对buf操作
	printk("This is chrdev_write \n");

    // 如果缓冲区中没有数据可读，则返回 0 表示读取结束
    if (!count)
        return 0;

	//前64个，即前一半
    // 将写入缓冲区中的数据逆序复制到读取缓冲区 rbuff
    for (i = 0; i < count/2; i++)
        rbuff[i] = wbuff[i]; // 正序
    
    //后64个，即后一半
    // 将写入缓冲区中的数据逆序复制到读取缓冲区 rbuff
    for (i = count/2; i < count; i++)
        rbuff[i] = wbuff[count - i - 1]; // 倒序
    
    // 如果要读取的数据量大于缓冲区中的数据量，则限制为缓冲区中的数据量
    if (size > count)
        size = count;

    // 将读取缓冲区中的数据发送给用户空间
    copy_to_user(buf, rbuff, size); // 将数据发送给用户空间 内置函数

    // 返回实际读取的字节数
    return size;
}

static int chrdev_release(struct inode *inode, struct file *file)
{
	return 0;
}

static dev_t dev_num;//定义dev_t类型变量dev_num来表示设备号
static struct cdev cdev_test;//定义struct cdev 类型结构体变量cdev_test，表示要注册的字符设备
static struct file_operations cdev_fops_test = {
    .owner = THIS_MODULE,//将owner字段指向本模块，可以避免在模块的操作正在被使用时卸载该模块
	.open = chrdev_open,
	.read = chrdev_read,
	.write = chrdev_write,
	.release = chrdev_release,
};//定义file_operations结构体类型的变量cdev_test_ops

static struct class *class_test;//定于struct class *类型结构体变量class_test，表示要创建的类

static int __init chrdev_fops_init(void)//驱动入口函数
{
	int ret;//定义int类型的变量ret，用来对函数返回值进行判断
    int major,minor;//定义int类型的主设备号major和次设备号minor
	ret = alloc_chrdev_region(&dev_num,0,1,"chrdev_name");//自动获取设备号，设备名chrdev_name
    if (ret  < 0){
        printk("alloc_chrdev_region is error \n");
    }
    printk("alloc_chrdev_region is ok \n");
    major = MAJOR(dev_num);//使用MAJOR()函数获取主设备号
    minor = MINOR(dev_num);//使用MINOR()函数获取次设备号
    printk("major is %d\n",major);
	printk("minor is %d\n",minor);
    cdev_init(&cdev_test,&cdev_fops_test);//使用cdev_init()函数初始化cdev_test结构体，并链接到cdev_test_ops结构体
	cdev_test.owner = THIS_MODULE;//将owner字段指向本模块，可以避免在模块的操作正在被使用时卸载该模块
	ret = cdev_add(&cdev_test,dev_num,1); //使用cdev_add()函数进行字符设备的添加
    if (ret < 0){
         printk("cdev_add is error \n");
    }
    printk("cdev_add is ok \n");                                                                                
    class_test  = class_create(THIS_MODULE,"class_work2");//使用class_create进行类的创建，类名称为device_test_work2
    device_create(class_test,NULL,dev_num,NULL,"device_work2");//使用device_create进行设备的创建，设备名称为device_test_work2
    return 0;
}

static void __exit chrdev_fops_exit(void)//驱动出口函数
{
	device_destroy(class_test,dev_num);//删除创建的设备
    class_destroy(class_test);//删除创建的类
    cdev_del(&cdev_test);//删除添加的字符设备cdev_test
	unregister_chrdev_region(dev_num,1);//释放字符设备所申请的设备号
    printk("module exit \n");
}

module_init(chrdev_fops_init);//注册入口函数
module_exit(chrdev_fops_exit);//注册出口函数
MODULE_LICENSE("GPL v2");//同意GPL开源协议
MODULE_AUTHOR("topeet");//作者信息
```

## chrdev_fops.mod.c

```c
#include <linux/build-salt.h>
#include <linux/module.h>
#include <linux/vermagic.h>
#include <linux/compiler.h>

BUILD_SALT;

MODULE_INFO(vermagic, VERMAGIC_STRING);
MODULE_INFO(name, KBUILD_MODNAME);

__visible struct module __this_module
__section(.gnu.linkonce.this_module) = {
	.name = KBUILD_MODNAME,
	.init = init_module,
#ifdef CONFIG_MODULE_UNLOAD
	.exit = cleanup_module,
#endif
	.arch = MODULE_ARCH_INIT,
};

#ifdef CONFIG_RETPOLINE
MODULE_INFO(retpoline, "Y");
#endif

static const struct modversion_info ____versions[]
__used __section(__versions) = {
	{ 0xa00faa6f, "module_layout" },
	{ 0x6091b333, "unregister_chrdev_region" },
	{ 0x22b90774, "cdev_del" },
	{ 0xb65e5a32, "class_destroy" },
	{ 0xbd9cb3ec, "device_destroy" },
	{ 0x17c294af, "device_create" },
	{ 0x2871e975, "__class_create" },
	{ 0xc4952f09, "cdev_add" },
	{ 0x2064fa56, "cdev_init" },
	{ 0xe3ec2f2b, "alloc_chrdev_region" },
	{ 0xc5850110, "printk" },
	{ 0xbdfb6dbb, "__fentry__" },
};

MODULE_INFO(depends, "");

MODULE_INFO(srcversion, "3A192D7C680BE0009C411F1");

```



## app.c

```c
#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <unistd.h>
#include <string.h>

int main(int argc,char *argv[])
{
    int fd;//定义int类型的文件描述符
    char buf[32];//定义读取缓冲区buf
    fd=open(argv[1],O_RDWR,0666);//fcntl.h库内函数，调用open函数，打开输入的第一个参数文件，权限为可读可写
    if(fd<0){
        printf("open is error\n"); //应用程序
        return -1;
    }
    printf("open is ok\n");
	/*如果第二个参数为read，条件成立，调用read函数，对文件进行读取*/                                                                                                                                  
    if(!strcmp(argv[2], "read")){
        read(fd,buf,32);
    }
	/*如果第二个参数为write，条件成立，调用write函数，对文件进行写入*/  
    else if(!strcmp(argv[2], "write")){
        write(fd,"hello\n",6);
    }
    close(fd);//调用close函数，对取消文件描述符到文件的映射
    return 0;
}
```

# Demo

## demo.c

```c
/****************************************************************************************
** 
**  file name: demo.c
**  author   : Zou jian guo
**  date     : 2003-12-22
**  email    : ah_zou@tom.com
**  updated by Wang jun
**  data:  2006-6-2
**  email:wju_uptech@126.com

**  Descriptions: a demo for Linux device deiver  

**  This demo character device driver implements the following functions:
**  two buffers will be used to simulate a real port.  wbuff is a buffer
**  which will be used to store the data that is written to the device,
**  rbuff is another buffer which will be used when the data is red from
**  the device.
**  the string written to wbuff will be copied to rbuff in reverse order.

** Copyright (C) 2002-2006  UP-TECH  All rights reserved.
**
** This file is part of basic demos for UP-NETARM2410 or UP-NETARM2410-S
** 
** This example program may be used, distributed and modified without limitation.
** No warranty is attached, we cannot take responsibility for errors or fitness for use.
****************************************************************************************/

/*************************************************************************************/
//#define CONFIG_DEVFS_FS      //use devfs

#ifndef __KERNEL__
#define __KERNEL__
#endif

#ifndef MODULE
#define MODULE
#endif
/*************************************************************************************/
#include <linux/config.h>
#include <linux/module.h>
#include <linux/kernel.h>
#include <linux/fs.h>
#include <linux/errno.h>
#include <linux/slab.h>
#include <linux/mm.h>
#include <linux/init.h>
#include <linux/interrupt.h>
#include <asm/uaccess.h>
#include <asm/hardware.h>
#include <linux/irq.h>
#include "demo.h"

static int DEMO_MAJOR = 0;          //major device number allocated dynamicly 

/****************************************************************************/

/* open and close */
/**********************************************************
** function name : demo_open         
** input param   :   
** output param  :  0
** description   :  open the demo device
************************************************************/
static int demo_open(struct inode *inode, struct file *filp)
{
    // 初始化写入和读取缓冲区，以及数据计数
    memset(wbuff, 0, BUFFSIZE);
    memset(rbuff, 0, BUFFSIZE);
    count = 0;

    // 增加内核模块的使用计数，表示有进程正在使用该模块
    MOD_INC_USE_COUNT; // 增加用户计数

    // 返回 0 表示成功打开设备
    return 0;
}

/*********************************************************************************/
/***********************************************************
** function name : demo_close         
** input param   :   
** output param  :  0
** description   :  close the demo device 
************************************************************/
static int demo_close(struct inode *inode, struct file *filp)
{
    // 减少内核模块的使用计数
    MOD_DEC_USE_COUNT; // 操作使用计数的宏

    // 返回 0 表示成功关闭设备
    return 0;
}

/*********************************************************************************/
/* read and write */
/***********************************************************
** function name : demo_read         
** input param   :   
** output param  : cnt
** description   :  send reversed data to user
************************************************************/
static ssize_t demo_read(struct file *filp, char *buf, size_t cnt, loff_t *off)
{
    int i;

    // 如果缓冲区中没有数据可读，则返回 0 表示读取结束
    if (!count)
        return 0;

    // 将写入缓冲区中的数据逆序复制到读取缓冲区 rbuff
    for (i = 0; i < count; i++)
        rbuff[i] = wbuff[count - i - 1]; // 反转数据

    // 如果要读取的数据量大于缓冲区中的数据量，则限制为缓冲区中的数据量
    if (cnt > count)
        cnt = count;

    // 将读取缓冲区中的数据发送给用户空间
    copy_to_user(buf, rbuff, cnt); // 将数据发送给用户空间

    // 返回实际读取的字节数
    return cnt;
}
/***************************************************************************/
/***********************************************************
** function name : demo_write         
** input param   :   
** output param  :  cnt
** description   :  receive data from user
************************************************************/
static ssize_t demo_write(struct file *filp, const char *buf, size_t cnt, loff_t *off)
{
    // 检查写入的数据量是否超过缓冲区大小
    if (cnt > BUFFSIZE)
        cnt = BUFFSIZE;

    // 从用户空间拷贝数据到内核缓冲区 wbuff
    copy_from_user(wbuff, buf, cnt); // copy_from_user(to, from, size)

    // 更新缓冲区中数据的字节数
    count = cnt;

    // 返回实际写入的字节数
    return cnt;
}

/*************************************************************************************/
/* ioctl */
/***********************************************************
** function name : demo_ioctl         
** input param   :   
** output param  :  0
** description   :  ioctl frame
************************************************************/
static int demo_ioctl(struct inode *inode, struct file *filp, 
    unsigned int cmd, unsigned long arg)
{
    // 在这里实现对设备的 IOCTL 操作

    switch(cmd) {
        // 如果 cmd 是某个 IOCTL 命令的情况，可以在这里添加相应的处理代码
        // case YOUR_IOCTL_COMMAND:
        //     // 处理相应的 IOCTL 操作
        //     break;
        // 其他可能的 IOCTL 命令可以继续添加在这里

        default:
            // 如果 cmd 不是已知的 IOCTL 命令，可以选择忽略或者返回错误
            // 这里暂时什么都不做，返回 0 表示成功
            break;
    }

    // 在这里返回处理结果，返回 0 表示成功，负值表示失败
    return 0;
}

/*************************************************************************************/
/* the device fops */

static struct file_operations demo_fops = {
	owner:		THIS_MODULE,
	read:		demo_read,
	write:		demo_write,
	ioctl:		demo_ioctl,
	open:		demo_open,
    release:	demo_close,
};
/*************************************************************************************/
/*use devfs*/
/*
#ifdef CONFIG_DEVFS_FS
static devfs_handle_t  devfs_demo_dir, devfs_demoraw;
#endif
*/
/*************************************************************************************/


/* init and exit */
/***********************************************************
** function name : demo_init         
** input param   :   
** output param  :  
** description   :  initial demo module
************************************************************/
static int __init demo_init(void)
{
	int result;

	wbuff = kmalloc(BUFFSIZE, GFP_KERNEL);
	rbuff = kmalloc(BUFFSIZE, GFP_KERNEL);
	result = register_chrdev(DEMO_MAJOR, "demo", &demo_fops);

	if(result < 0) {
		printk(KERN_ERR "Cannot register demo device.\n");
		kfree(wbuff);
		kfree(rbuff);
		return -EIO;
	}
    if(DEMO_MAJOR==0)
    	DEMO_MAJOR = result;
	/*devfs*/
/*
#ifdef CONFIG_DEVFS_FS
    devfs_demo_dir = devfs_mk_dir(NULL, "demo", NULL);   //make demo device directory
    devfs_demoraw = devfs_register(devfs_demo_dir, "0", DEVFS_FL_DEFAULT,
                    DEMO_MAJOR, DEMO_MINOR, S_IFCHR | S_IRUSR | S_IWUSR,
                    &demo_fops, NULL);   //register demo device "0"
#endif
*/
   	/* some furthertreatment for this case */
	
	return 0;
}
/***************************************************************************/
/***********************************************************
** function name : demo_exit         
** input param   :   
** output param  :  
** description   :  exit demo module
************************************************************/
static void __exit demo_exit(void)
{
	int result;
/*
#ifdef CONFIG_DEVFS_FS	
	devfs_unregister(devfs_demoraw);    //delete demo device 
	devfs_unregister(devfs_demo_dir);   //delete demo device directory 
#endif
*/
	result = unregister_chrdev(DEMO_MAJOR, "demo");

	if(result < 0) {
		printk(KERN_ERR "Cannot remove demo device.\n");
		return;
	}

	if(wbuff)
		kfree(wbuff);          //free memory
	if(rbuff)
		kfree(rbuff);

	return;
}
/*************************************************************************************/
MODULE_LICENSE("GPL");
module_init(demo_init);
module_exit(demo_exit);
```

## demo.h

```c
/*
 * demo.h
 */

#define MOD_INC_USE_COUNT try_module_get(THIS_MODULE)
#define MOD_DEC_USE_COUNT module_put(THIS_MODULE)

//#define DEMO_MAJOR 21
#define BUFFSIZE 64
#define DEMO_MINOR 0
/* the counter to record the char number in wbuff  */
int count;

/* buffer pointers, the buffer will be allocated in device init */
char *wbuff, *rbuff;

```

## test_demo.c

```c
#include <stdio.h>
#include <stdlib.h>
#include <fcntl.h>
#include <unistd.h>
#include <sys/ioctl.h>


void showbuf(char *buf);
int MAX_LEN=128;

int main()
{
	int fd;
	int i;
	char buf[255];

	for(i=0; i<MAX_LEN; i++){
		buf[i]=i;
	}

	fd=open("/dev/demo",O_RDWR);
	if(fd < 0){
		printf("####DEMO  device open fail####\n");
		return (-1);
	}
	printf("write %d bytes data to /dev/demo \n",MAX_LEN);
	showbuf(buf);
	write(fd,buf,MAX_LEN);

	printf("Read %d bytes data from /dev/demo \n",MAX_LEN);
	read(fd,buf,MAX_LEN);
	showbuf(buf);
	
	ioctl(fd,1,NULL);
	ioctl(fd,4,NULL);
	close(fd);
	return 0;

}


void showbuf(char *buf)
{
	int i,j=0;
	for(i=0;i<MAX_LEN;i++){
		if(i%8 ==0)
			printf("\n%4d: ",j++);
		printf("%4d ",buf[i]);
	}
	printf("\n*****************************************************\n");
}
```

# work2

## 修改后

## chrdev_fops.c

```c
#include <linux/init.h>
#include <linux/module.h>
#include <linux/fs.h>
#include <linux/kdev_t.h>
#include <linux/cdev.h>
#include <linux/uaccess.h>


#define MOD_INC_USE_COUNT try_module_get(THIS_MODULE)
#define MOD_DEC_USE_COUNT module_put(THIS_MODULE)


#define BUFFSIZE 256
#define DEMO_MINOR 0

// 定义一些 IOCTL 命令
#define MEM_IOC_MAGIC 'm' //定义类型
#define ioctl_SET      _IOW(MEM_IOC_MAGIC, 1, int)
#define ioctl_GET      _IOR(MEM_IOC_MAGIC, 2, int)
#define ioctl_READ     _IOR(MEM_IOC_MAGIC, 3, int)
#define ioctl_TOZERO      _IOW(MEM_IOC_MAGIC, 4, int)
#define ioctl_MAJOR    _IOR(MEM_IOC_MAGIC, 5, int)
#define ioctl_MINOR    _IOR(MEM_IOC_MAGIC, 6, int)

/* the counter to record the char number in wbuff  */
int count;
int i;
int status;
int major,minor;//定义int类型的主设备号major和次设备号minor
char mode[50];  // 存储当前设备的模式
/* buffer pointers, the buffer will be allocated in device init */
char wbuff[BUFFSIZE],rbuff[BUFFSIZE];
static int chrdev_open(struct inode *inode, struct file *file)
{
	printk("This is chrdev_open \n");
	return 0;
}

//写入数据
static ssize_t chrdev_write(struct file *file,const char __user *buf,size_t size,loff_t *off)
{
    // 从缓冲区读取
	printk("This is chrdev_write \n");
    
    // 检查写入的数据量是否超过缓冲区大小
    if (size > BUFFSIZE)
        size = BUFFSIZE;

    // 从用户空间拷贝数据到内核缓冲区 wbuff
    copy_from_user(wbuff, buf, size); // copy_from_user(to, from, size) 内置函数

    // 更新缓冲区中数据的字节数
    count = size;

    // 返回实际写入的字节数
    return size;
}

//读出数据
static ssize_t chrdev_read(struct file *filp, char __user *buf, size_t size, loff_t *off)
{
    // 同样对buf操作
	printk("This is chrdev_read \n");

    // 如果缓冲区中没有数据可读，则返回 0 表示读取结束
    if (!count)
        return 0;

	//前64个，即前一半
    // 将写入缓冲区中的数据逆序复制到读取缓冲区 rbuff
    for (i = 0; i < count/2; i++)
        rbuff[i] = wbuff[count - i - 1 - count/2]; // 倒序
    
    //后64个，即后一半
    // 将写入缓冲区中的数据逆序复制到读取缓冲区 rbuff
    for (i = count/2; i < count; i++)
        rbuff[i] = wbuff[i]; //正序
    
    // 如果要读取的数据量大于缓冲区中的数据量，则限制为缓冲区中的数据量
    if (size > count)
        size = count;

    // 将读取缓冲区中的数据发送给用户空间
    copy_to_user(buf, rbuff, size); // 将数据发送给用户空间 内置函数

    // 返回实际读取的字节数
    return size;
}

//ioctl
static long int chrdev_ioctl(struct file *filp, unsigned int cmd, long unsigned int arg) {

    switch (cmd) {
        case ioctl_MAJOR: {
            // 打印设备主设备号
            printk("设备主设备号为： %d .\n", major);
            // 将主设备号写入 arg，以供用户空间获取
            arg = major;
            break;
        }
        case ioctl_MINOR: {
            // 打印设备次设备号
            printk("设备次设备号为： %d .\n", minor);
            // 将次设备号写入 arg，以供用户空间获取
            arg = minor;
            break;
        }
        case ioctl_SET: {
            // 使用 copy_from_user 将用户空间的字符串拷贝到内核空间
            if (copy_from_user(mode, (char __user *)arg, sizeof(mode)) != 0) {
                // 如果拷贝失败，返回适当的错误码
                return -EFAULT;
            }
            // 打印改变后的模式
            printk("模式已改变，当前模式为： %s .\n", mode);
            break;
        }
        case ioctl_GET: {
            // 打印当前设备模式
            printk("当前设备模式为: %s\n", mode);
            break;
        }
        case ioctl_READ: { // 读取某个数
            // 在这里处理读取数据的操作
            // 打印缓冲区中指定位置的数据
            printk("缓冲区写入数据中第%ld个数据为: %4d \n", arg, wbuff[arg]);
            break;
        }
        case ioctl_TOZERO: { // 之后添加某个数
            // 打印清零前的数值
            printk("缓冲区内第%ld个数原本为 %4d ,现清零成功！\n", arg, wbuff[arg]);
            // 将缓冲区中指定位置的值清零
            wbuff[arg] = 0;
            break;
        }
        default:
            // 如果 cmd 不是已知的 IOCTL 命令，可以选择忽略或者返回错误
            // 这里暂时什么都不做，返回 0 表示成功
            break;
    }
    
    // 返回 1 表示成功
    return 1;
}



static int chrdev_release(struct inode *inode, struct file *file)
{
	return 0;
}

static dev_t dev_num;//定义dev_t类型变量dev_num来表示设备号
static struct cdev cdev_test;//定义struct cdev 类型结构体变量cdev_test，表示要注册的字符设备
static struct file_operations cdev_fops_test = {
    
    .owner 			= 	THIS_MODULE,//将owner字段指向本模块，可以避免在模块的操作正在被使用时卸载该模块
	.open 			= 	chrdev_open,
	.read 			= 	chrdev_read,
	.write 			= 	chrdev_write,
    .unlocked_ioctl	=	chrdev_ioctl,
	.release 		= 	chrdev_release,
    
};//定义file_operations结构体类型的变量cdev_test_ops

static struct class *class_test;//定于struct class *类型结构体变量class_test，表示要创建的类

static int __init chrdev_fops_init(void)//驱动入口函数
{
	int ret;//定义int类型的变量ret，用来对函数返回值进行判断
	ret = alloc_chrdev_region(&dev_num,0,1,"chrdev_work2");//动态自动分配设备号，设备名chrdev_work2
    if (ret  < 0){
        printk("alloc_chrdev_region is error \n");
    }
    printk("alloc_chrdev_region is ok \n");

    major = MAJOR(dev_num);//使用MAJOR()函数获取主设备号
    minor = MINOR(dev_num);//使用MINOR()函数获取次设备号
    printk("major is %d\n",major);
	printk("minor is %d\n",minor);

    cdev_init(&cdev_test,&cdev_fops_test);//使用cdev_init()函数初始化cdev_test结构体，并链接到cdev_test_ops结构体
	cdev_test.owner = THIS_MODULE;//将owner字段指向本模块，可以避免在模块的操作正在被使用时卸载该模块

	ret = cdev_add(&cdev_test,dev_num,1); //使用cdev_add()函数进行字符设备的添加
    if (ret < 0){
         printk("cdev_add is error \n");
    }
    printk("cdev_add is ok \n");      

    class_test  = class_create(THIS_MODULE,"class_work2");//使用class_create进行类的创建，类名称为device_test_work2
    device_create(class_test,NULL,dev_num,NULL,"device_work2");//使用device_create进行设备的创建，设备名称为device_test_work2
    return 0;
}


static void __exit chrdev_fops_exit(void)//驱动出口函数
{
	device_destroy(class_test,dev_num);//删除创建的设备
    class_destroy(class_test);//删除创建的类
    cdev_del(&cdev_test);//删除添加的字符设备cdev_test
	unregister_chrdev_region(dev_num,1);//释放字符设备所申请的设备号
    printk("module exit \n");
}

module_init(chrdev_fops_init);//注册入口函数
module_exit(chrdev_fops_exit);//注册出口函数
MODULE_LICENSE("GPL v2");//同意GPL开源协议
MODULE_AUTHOR("topeet");//作者信息
```

## chrdev_fops.mod.c

```c
#include <linux/build-salt.h>
#include <linux/module.h>
#include <linux/vermagic.h>
#include <linux/compiler.h>

BUILD_SALT;

MODULE_INFO(vermagic, VERMAGIC_STRING);
MODULE_INFO(name, KBUILD_MODNAME);

__visible struct module __this_module
__section(.gnu.linkonce.this_module) = {
	.name = KBUILD_MODNAME,
	.init = init_module,
#ifdef CONFIG_MODULE_UNLOAD
	.exit = cleanup_module,
#endif
	.arch = MODULE_ARCH_INIT,
};

#ifdef CONFIG_RETPOLINE
MODULE_INFO(retpoline, "Y");
#endif

static const struct modversion_info ____versions[]
__used __section(__versions) = {
	{ 0xc2b78c96, "module_layout" },
	{ 0x6091b333, "unregister_chrdev_region" },
	{ 0x22b90774, "cdev_del" },
	{ 0xb65e5a32, "class_destroy" },
	{ 0xbd9cb3ec, "device_destroy" },
	{ 0x17c294af, "device_create" },
	{ 0x2871e975, "__class_create" },
	{ 0xc4952f09, "cdev_add" },
	{ 0x2064fa56, "cdev_init" },
	{ 0xe3ec2f2b, "alloc_chrdev_region" },
	{ 0xb44ad4b3, "_copy_to_user" },
	{ 0x88db9f48, "__check_object_size" },
	{ 0x56470118, "__warn_printk" },
	{ 0x362ef408, "_copy_from_user" },
	{ 0xc5850110, "printk" },
	{ 0xbdfb6dbb, "__fentry__" },
};

MODULE_INFO(depends, "");


MODULE_INFO(srcversion, "2149E30252F6BCFF36F3FF6");

```



## app.c

```c
#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <sys/fcntl.h> 
#include <sys/ioctl.h>  
#include <unistd.h>
#include <string.h>

// 定义一些 IOCTL 命令
#define MEM_IOC_MAGIC 'm' //定义类型
#define ioctl_SET      _IOW(MEM_IOC_MAGIC, 1, int)
#define ioctl_GET      _IOR(MEM_IOC_MAGIC, 2, int)
#define ioctl_READ     _IOR(MEM_IOC_MAGIC, 3, int)
#define ioctl_TOZERO      _IOW(MEM_IOC_MAGIC, 4, int)
#define ioctl_MAJOR    _IOR(MEM_IOC_MAGIC, 5, int)
#define ioctl_MINOR    _IOR(MEM_IOC_MAGIC, 6, int)

// 声明函数 showbuf，用于显示缓冲区内容
void showbuf(char *buf);

// 定义缓冲区最大长度
int MAX_LEN = 128;

int main(int argc, char *argv[]) {
    int fd;
    int i;
    char buf[255];

    // 初始化缓冲区数据
    for (i = 0; i < MAX_LEN; i++) {
        buf[i] = i;
    }

    // 获取文件路径
    char *path = argv[1];

    // 调用 open 函数打开文件，权限为可读可写
    fd = open(argv[1], O_RDWR, 0666);

    // 检查文件是否成功打开
    if (fd < 0) {
        printf("open is error\n"); // 应用程序
        return -1;
    }

    // 检查第二个参数是否为空
    if (argv[2] == NULL) {
        printf("应用程序输出： \n");
        showbuf(buf); // 先打印一遍
        printf("应用程序参数为空！\n");
        return 0;
    }

    /* 如果第二个参数为 read，条件成立，调用 read 函数，对文件进行读取 */
    if (!strcmp(argv[2], "read")) {
        printf("Read %d bytes data from %s \n", MAX_LEN, path);
        read(fd, buf, MAX_LEN);
        showbuf(buf);
    }
    /* 如果第二个参数为 write，条件成立，调用 write 函数，对文件进行写入 */
    else if (!strcmp(argv[2], "write")) {
        printf("应用程序输出： \n");
        showbuf(buf); // 先打印一遍
        printf("write %d bytes data to %s \n", MAX_LEN, path);
        showbuf(buf);
        write(fd, buf, MAX_LEN);
    }
    /* 如果第三个参数为 ioctl 操作，条件成立，调用 ioctl 操作函数，进行应用程序和驱动程序之间参数传递并打印 */
    else if (!strcmp(argv[2], "ioctl")) {
        printf("ioctl mode : \n");
        char *cmd_str = argv[3];
        int ioctl_cmd;

        // 根据命令字符串设置对应的 ioctl 命令
        if (!strcmp(cmd_str, "ioctl_SET")) {
            ioctl_cmd = ioctl_SET;
            char *mode = argv[4]; // 模式输入为第四个参数，存储上一次修改的系统状态

            // 调用 ioctl 函数执行 SET 操作
            if (ioctl(fd, ioctl_cmd, mode) == 1) {
                printf("已修改，当前模式为：%s。请用 dmesg 查看\n", mode);
            } else {
                printf("修改模式失败，请检查原因\n");
            }
        } else if (!strcmp(cmd_str, "ioctl_GET")) {
            ioctl_cmd = ioctl_GET;

            // 调用 ioctl 函数执行 GET 操作
            if (ioctl(fd, ioctl_cmd, NULL) == 1) {
                printf("获取主设备号成功!请用 dmesg 查看\n");
            } else {
                printf("获取主设备号失败，请检查原因\n");
            }
        } else if (!strcmp(cmd_str, "ioctl_READ")) {
            ioctl_cmd = ioctl_READ;
            char *ind_str = argv[4]; // 读取数字下标输入为第四个参数
            int index = atoi(ind_str);

            // 调用 ioctl 函数执行 READ 操作
            if (ioctl(fd, ioctl_cmd, index) == 1) {
                printf("数据读取成功!请用 dmesg 查看\n");
            } else {
                printf("数据读取失败，请检查原因\n");
            }
        } else if (!strcmp(cmd_str, "ioctl_TOZERO")) {
            ioctl_cmd = ioctl_TOZERO;
            char *add_str = argv[4]; // 读取数字下标输入为第四个参数
            int index = atoi(add_str);

            // 调用 ioctl 函数执行 TOZERO 操作
            if (ioctl(fd, ioctl_cmd, index) == 1) {
                printf("数据置零成功!请用 dmesg 查看\n");
            } else {
                printf("数据置零失败，请检查原因\n");
            }
        } else if (!strcmp(cmd_str, "ioctl_MAJOR")) {
            ioctl_cmd = ioctl_MAJOR;

            // 调用 ioctl 函数执行 MAJOR 操作
            if (ioctl(fd, ioctl_cmd, NULL) == 1) {
                printf("获取主设备号成功!请用 dmesg 查看\n");
            } else {
                printf("获取主设备号失败，请检查原因\n");
            }
        } else if (!strcmp(cmd_str, "ioctl_MINOR")) {
            ioctl_cmd = ioctl_MINOR;

            // 调用 ioctl 函数执行 MINOR 操作
            if (ioctl(fd, ioctl_cmd, NULL) == 1) {
                printf("获取次设备号成功!请用 dmesg 查看\n");
            } else {
                printf("获取次设备号失败，请检查原因\n");
            }
        } else {
            // 处理未知的 ioctl 命令，这里可能需要返回错误或执行其他操作
            printf("Unknown ioctl command\n");
            return -1;
        }
    } else {
        printf("应用程序输出： \n");
        showbuf(buf); // 先打印一遍
        printf("应用程序参数错误，请检查参数输入！\n");
    }

    // 关闭文件描述符
    close(fd);
    return 0;
}


void showbuf(char *buf)
{
    // 初始化变量
    int i, j = 0;

    // 遍历缓冲区
    for (i = 0; i < MAX_LEN; i++) {
        // 检查是否应该开始新的一行
        if (i % 8 == 0)
            printf("\n第 %4d 行: ", j++);  // 打印行号并递增

        // 打印缓冲区当前索引处的值
        printf("%4d ", buf[i]);
    }

    // 在显示缓冲区后打印分隔线
    printf("\n*****************************************************\n");
}
```

## Makefile

```makefile
ifneq ($(KERNELRELEASE),)
obj-m:=chrdev_fops.o
else
KERNELDIR:=/lib/modules/$(shell uname -r)/build
PWD:=$(shell pwd)
default:
	$(MAKE) -C $(KERNELDIR) M=$(PWD) modules
clean:
	rm -rf *.o *.mod.c *.mod.o *.ko
endif
```

```c
#include<sys/types.h>
#include<sys/socket.h>
#include<stdio.h>
#include<netinet/in.h>
#include<arpa/inet.h>
#include<unistd.h>
#include<string.h>
#include<netdb.h>
#include<sys/ioctl.h>
#include<termios.h>
#include<stdlib.h>
#include<sys/stat.h>
#include<fcntl.h>
#include<signal.h>
#include<sys/time.h>
#include<errno.h>
#include<arpa/inet.h>
#include<net/if.h>
#include<ifaddrs.h>

#define port 5000
#define BUFFER_SIZE 128


int main()
{    
    
    FILE *stream;                     // 声明一个文件流指针
    struct sockaddr_in svraddr;       // 声明一个用于存储服务器地址信息的结构体
    int clientsocket;                  // 声明一个客户端套接字描述符
    int i = 0;                         // 初始化一个整型变量 i，用于循环计数
    bzero(&svraddr, sizeof(svraddr)); // 使用 bzero 函数将 svraddr 结构体清零
    //AF_INET： Ipv4网络协议；SOCK_STREAM： 提供面向连接的稳定数据传输，即TCP协议
    // 尝试创建套接字
    if ((clientsocket = socket(AF_INET, SOCK_STREAM, 0)) < 0) 
    {
        perror("socket error"); // 如果套接字创建失败，打印错误信息
        exit(1);                // 退出程序，返回错误码 1
    }

    svraddr.sin_family = AF_INET;              // 设置地址族为 IPv4
    svraddr.sin_port = htons(port);            // 设置服务器端口号，将主机字节序转换为网络字节序
    svraddr.sin_addr.s_addr = inet_addr("192.168.88.130"); // 设置服务器 IP 地址

    //客户端的地址是系统自动分配端口号和自身的IP
    //尝试与目标主机建立连接
    if (connect(clientsocket, (struct sockaddr *)&svraddr, sizeof(svraddr)) < 0) 
    {
        perror("connect"); // 如果连接失败，打印错误信息
        exit(1);           // 退出程序，返回错误码 1
    }
    printf("connect with destination host....\n"); // 连接成功后，打印提示信息

    char buffer[BUFFER_SIZE]; // 声明一个字符数组作为缓冲区
    bzero(buffer, BUFFER_SIZE); // 使用 bzero 将缓冲区初始化为零，确保其中的内容为空

    // 循环遍历缓冲区，为每个元素赋值
    for (int i = 0; i < BUFFER_SIZE; i++) 
    {
        buffer[i] = i; // 将当前索引值赋给缓冲区的相应位置
    }

    /* 连接成功了 */ 
    int nbytes;
    // 尝试通过套接字发送数据
    if ((nbytes = write(clientsocket, buffer, BUFFER_SIZE)) == -1) 
    {
        fprintf(stderr, "send Error: %s\n", strerror(errno)); // 如果发送失败，打印错误信息
        exit(1); // 退出程序，返回错误码 1
    }

	int lengsize = 0;
	printf("client start to read data \n");
	while((lengsize = read(clientsocket,buffer,BUFFER_SIZE)) > 0)
    {
		for(i=0;i<BUFFER_SIZE;i++)
		{
			//printf(" %d",buffer[i]);
			if(i%8 == 0)
			{
                printf("\n第 %4d 行: ", j++);  // 打印行号并递增
			}
            printf(" %-4d",buffer[i]);
		}
	    printf("\n");
	}
/*
    if ((stream = fopen("H264.ts", "rb")) == NULL) //"rb"（二进制只读）
    {
        // 如果文件 'H264.ts' 打开失败
        printf("文件 'H264.ts' 打开失败！\n");
    } 
    else 
    {
        // 如果文件 'H264.ts' 成功打开
        bzero(buffer, BUFFER_SIZE);
        int lengsize = 0;

        // 循环读取文件内容并发送给客户端
        while ((lengsize = fread(buffer, sizeof(char), BUFFER_SIZE, stream)) > 0) 
        {
            // 打印读取到的数据大小
            printf("lengsize = %d\n", lengsize);

            // 将读取到的数据发送给客户端
            if (send(clientsocket, buffer, lengsize, 0) < 0) 
            {
                // 如果发送失败，打印错误信息并退出循环
                printf("文件发送失败\n");
                break;
            } 
            else 
            {
                // 发送成功后清空缓冲区
                bzero(buffer, BUFFER_SIZE);
            }
        }
        // 关闭文件
        fclose(stream);
        // 打印文件传输完成的信息
        printf("文件 'H264.ts' 传输完成！\n");
    }
    */
    close(clientsocket);
    return 0;
}

/*
// 获取本机IP地址的函数
const char* get_local_ip();
// 在你的代码中使用这个函数获取本机IP地址
const char* local_ip = get_local_ip();
// 设置服务器地址
svraddr.sin_addr.s_addr = inet_addr(local_ip);
// 获取本机IP地址的函数
const char* get_local_ip()
{
    struct ifaddrs *ifap, *ifa;
    struct sockaddr_in *sa;
    char *addr;

    getifaddrs(&ifap);
    for (ifa = ifap; ifa; ifa = ifa->ifa_next) 
    {
        if (ifa->ifa_addr->sa_family == AF_INET) 
        {
            sa = (struct sockaddr_in *)ifa->ifa_addr;
            addr = inet_ntoa(sa->sin_addr);
            
            // 排除回环地址
            if (strcmp(addr, "127.0.0.1") != 0) 
            {
                freeifaddrs(ifap);
                return addr;
            }
        }
    }
    freeifaddrs(ifap);
    return NULL;
}
*/
```

c
