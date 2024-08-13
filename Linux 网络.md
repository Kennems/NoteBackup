# Linux 网络

## tcpclient.c

```c
#include <stdlib.h> 
#include <stdio.h> 
#include <errno.h> 
#include <string.h> 
#include <netdb.h> 
#include <sys/types.h> 
#include <netinet/in.h> 
#include <sys/socket.h> 
#include <arpa/inet.h> //inet_ntoa()函数的头文件

#define portnumber 3333 //定义端口号：（0-1024为保留端口号，最好不要用）

int main(int argc, char *argv[]) 
{ 
    int sockfd; 
    char buffer[1024]; 
    struct sockaddr_in server_addr; //描述服务器的地址
    struct hostent *host; 
    int nbytes; 

    /* 使用hostname查询host 名字 */
    if(argc!=2) 
    { 
        fprintf(stderr,"Usage:%s hostname \a\n",argv[0]); 
        exit(1); 
    } 

    if((host=gethostbyname(argv[1]))==NULL) 
    { 
        fprintf(stderr,"Gethostname error\n"); 
        exit(1); 
    } 

    /* 客户程序开始建立 sockfd描述符 */ 
    if((sockfd=socket(AF_INET,SOCK_STREAM,0))==-1) // AF_INET:Internet;SOCK_STREAM:TCP
    { 
        fprintf(stderr,"Socket Error:%s\a\n",strerror(errno)); 
        exit(1); 
    } 

    /* 客户程序填充服务端的资料 */ 
    bzero(&server_addr,sizeof(server_addr)); // 初始化,置0
    server_addr.sin_family=AF_INET; // IPV4
    server_addr.sin_port=htons(portnumber); // (将本机器上的short数据转化为网络上的short数据)端口号
    server_addr.sin_addr=*((struct in_addr *)host->h_addr); // IP地址
    
    /* 客户程序发起连接请求 */ 
    if(connect(sockfd,(struct sockaddr *)(&server_addr),sizeof(struct sockaddr))==-1) 
    { 
        fprintf(stderr,"Connect Error:%s\a\n",strerror(errno)); 
        exit(1); 
    } 

    /* 连接成功了 */ 
    if((nbytes=read(sockfd,buffer,1024))==-1) 
    { 
        fprintf(stderr,"Read Error:%s\n",strerror(errno)); 
        exit(1); 
    } 
    buffer[nbytes]='\0'; 
    printf("I have received:%s\n",buffer); 

    /* 结束通讯 */ 
    close(sockfd); 
    exit(0); 
}
```

## tcpserver.c

```c
#include <stdlib.h> 
#include <stdio.h> 
#include <errno.h> 
#include <string.h> 
#include <netdb.h> 
#include <sys/types.h> 
#include <netinet/in.h> 
#include <sys/socket.h> 
#include <arpa/inet.h> //inet_ntoa()函数的头文件

#define portnumber 3333 //定义端口号：（0-1024为保留端口号，最好不要用）

int main(int argc, char *argv[]) 
{ 
    int sockfd,new_fd; 
    struct sockaddr_in server_addr; //描述服务器地址
    struct sockaddr_in client_addr; //描述客户端地址
    int sin_size; 
    char hello[]="Hello! Are You Fine?\n"; 
    

    /* 服务器端开始建立sockfd描述符 */ 
    if((sockfd=socket(AF_INET, SOCK_STREAM, 0))==-1) // AF_INET:IPV4;SOCK_STREAM:TCP
    { 
        fprintf(stderr,"Socket error:%s\n\a",strerror(errno)); 
        exit(1); 
    } 

    /* 服务器端填充 sockaddr结构 */ 
    bzero(&server_addr,sizeof(struct sockaddr_in)); // 初始化,置0
    server_addr.sin_family=AF_INET; // Internet
    server_addr.sin_addr.s_addr=htonl(INADDR_ANY); // (将本机器上的long数据转化为网络上的long数据)和任何主机通信 //INADDR_ANY 表示可以接收任意IP地址的数据，即绑定到所有的IP
    //server_addr.sin_addr.s_addr=inet_addr("192.168.1.1"); //用于绑定到一个固定IP,inet_addr用于把数字加格式的ip转化为整形ip
    server_addr.sin_port=htons(portnumber); // (将本机器上的short数据转化为网络上的short数据)端口号
    
    /* 捆绑sockfd描述符到IP地址 */ 
    if(bind(sockfd,(struct sockaddr *)(&server_addr),sizeof(struct sockaddr))==-1) 
    { 
        fprintf(stderr,"Bind error:%s\n\a",strerror(errno)); 
        exit(1); 
    } 

    /* 设置允许连接的最大客户端数 */ 
    if(listen(sockfd,5)==-1) 
    { 
        fprintf(stderr,"Listen error:%s\n\a",strerror(errno)); 
        exit(1); 
    } 

    while(1) 
    { 
        /* 服务器阻塞,直到客户程序建立连接 */ 
        sin_size=sizeof(struct sockaddr_in); 
        if((new_fd=accept(sockfd,(struct sockaddr *)(&client_addr),&sin_size))==-1) 
        { 
            fprintf(stderr,"Accept error:%s\n\a",strerror(errno)); 
            exit(1); 
        } 
        fprintf(stderr,"Server get connection from %s\n",inet_ntoa(client_addr.sin_addr)); // 将网络地址转换成.字符串，并打印到输出终端
        
        //向客户端程序写入hello数组里的字符
        if(write(new_fd,hello,strlen(hello))==-1) 
        { 
            fprintf(stderr,"Write Error:%s\n",strerror(errno)); 
            exit(1); 
        } 
        /* 这个通讯已经结束 */ 
        close(new_fd); 
        /* 循环下一个 */ 
    } 

    /* 结束通讯 */ 
    close(sockfd); 
    exit(0); 
}
```

## client.c

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

#define port 5000
#define BUFFER_SIZE 128
int main()
{    
    
    FILE *stream;

    struct sockaddr_in svraddr;
    int clientsocket;
    int i=0;
    bzero(&svraddr,sizeof(svraddr));
    //AF_INET： Ipv4网络协议；SOCK_STREAM： 提供面向连接的稳定数据传输，即TCP协议
    if((clientsocket=socket(AF_INET,SOCK_STREAM,0))<0)
    {
        perror("socket error");
        exit(1);
    }
    svraddr.sin_family=AF_INET;
    svraddr.sin_port=htons(port);//将一个无符号短整型的主机数值转换为网络字节顺序
    svraddr.sin_addr.s_addr=inet_addr("192.168.56.130");

    //客户端的地址是系统自动分配端口号和自身的IP
    if(connect(clientsocket,(struct sockaddr *)&svraddr,sizeof(svraddr))<0)
    {
        perror("connect");
        exit(1);
    }
    printf("connect with destination host....\n");

    char buffer[BUFFER_SIZE];
    bzero(buffer,BUFFER_SIZE);
	
	for(i=0;i<BUFFER_SIZE;i++)
	{
		buffer[i] = i;
	}
  /* 连接成功了 */ 
	int nbytes;
    if((nbytes=write(clientsocket,buffer,BUFFER_SIZE))==-1) 
    { 
        fprintf(stderr,"send Error:%s\n",strerror(errno)); 
        exit(1); 
    } 

	int lengsize = 0;
	printf("client start to read data \n");
	while((lengsize = read(clientsocket,buffer,BUFFER_SIZE)) > 0)
    {
		for(i=0;i<BUFFER_SIZE;i++)
		{
			//printf(" %d",buffer[i]);
			if(i%4 == 0)
			{
				printf("\n");
			}
                        printf(" %-4d",buffer[i]);
		}
	    printf("\n");
	}
	/*
    if((stream = fopen("H264.ts","rb"))==NULL)
    {
        printf("The file 'H264.ts' was not opened! \n");
        
    }
    else
    {
        bzero(buffer,BUFFER_SIZE);
        int lengsize = 0;
        while((lengsize = fread(buffer,sizeof(char),BUFFER_SIZE,stream)) > 0)
        {
            printf("lengsize = %d\n",lengsize);
            if(send(clientsocket,buffer,lengsize,0) < 0)
            {
                printf("Send File is Failed\n");
                break;
            }
            else
            {
                bzero(buffer, BUFFER_SIZE);
            }

        }
        fclose(stream);
        printf("The file 'H264.ts' transfer finished! \n");
    }*/
    close(clientsocket);
    return 0;
}

```

## server.c

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

#define server_port 5000     
#define LENGTH_OF_LISTEN_QUEUE 20
#define BUFFER_SIZE 128

int main()
{
    int serversocket;
    struct sockaddr_in server_addr;
    struct sockaddr_in client_addr;
    int addr_len = sizeof(client_addr);
    int client;
    char buffer[BUFFER_SIZE];
	char WriteBuf[BUFFER_SIZE];
    int length;
    FILE *stream;
    int i=0;
   
    if((serversocket = socket(AF_INET,SOCK_STREAM,0))<0)
    {
        perror("socket");
        exit(1);
    }

    bzero(&server_addr,sizeof(server_addr));
    server_addr.sin_family = AF_INET;
    server_addr.sin_addr.s_addr = htonl(INADDR_ANY);
    server_addr.sin_port = htons(server_port);

    if(bind(serversocket,(struct sockaddr *)&server_addr,sizeof(server_addr))<0)
    {
        perror("connect");
        exit(1);
    }
    if(listen(serversocket,LENGTH_OF_LISTEN_QUEUE)<0)
    {
        perror("listen");
        exit(1);
    }
    
    //if((stream = fopen("data.ts","wb"))==NULL)
    //{
       // perror("File");
       // exit(1);
    //}
    while(1)
    {
        client=accept(serversocket,(struct sockaddr*)&client_addr,(socklen_t*)&addr_len);
        if(client<0)
        {
            perror("accept");
            continue;
        }
        printf("IP is %s\n",inet_ntoa(client_addr.sin_addr));

        bzero(buffer,BUFFER_SIZE);
		bzero(WriteBuf,BUFFER_SIZE);
        while((length = read(client,buffer,BUFFER_SIZE))>0)
        {
            if(length < 0)
            {
                printf("Recieve Data From Server %s Failed!\n", inet_ntoa(server_addr.sin_addr));
                break;
            }
			for(i=0;i<BUFFER_SIZE;i++)
			{
				WriteBuf[i] = buffer[BUFFER_SIZE-1-i]; 
				//printf("buffer[%d]=%d ",i,WriteBuf[i]);
			}
            int write_length = write(client,WriteBuf,length);
            if(write_length < length)
            {
                printf("File is Write Failed\n");
                break;
            }
            bzero(buffer,BUFFER_SIZE);
       }
       printf("Recieve File Finished\n", inet_ntoa(server_addr.sin_addr));
       //fclose(stream);	
       
       //exit(0);
    }
    close(serversocket);
	exit(0);
    return 0;
}
```

# 修改

## client.c

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
				printf("\n");
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

## server.c

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
#define port 5000
#define BUFFER_SIZE 128
int main()
{    
    
    FILE *stream;
    struct sockaddr_in svraddr;
    int clientsocket;
    int i = 0;

    // 初始化服务器地址结构体
    bzero(&svraddr, sizeof(svraddr));

    // 创建客户端套接字
    if ((clientsocket = socket(AF_INET, SOCK_STREAM, 0)) < 0) {
        perror("socket error"); // 输出错误信息并退出程序
        exit(1);
    }

    // 设置服务器地址信息
    svraddr.sin_family = AF_INET;
    svraddr.sin_port = htons(8080); // 将主机数值转换为网络字节顺序
    svraddr.sin_addr.s_addr = inet_addr("192.168.56.130");

    // 尝试与服务器建立连接
    if (connect(clientsocket, (struct sockaddr *)&svraddr, sizeof(svraddr)) < 0) {
        perror("connect"); // 输出错误信息并退出程序
        exit(1);
    }
    printf("Connected to the destination host....\n");

    char buffer[BUFFER_SIZE];
    bzero(buffer, BUFFER_SIZE);

    // 准备数据并发送给服务器
    for (i = 0; i < BUFFER_SIZE; i++) {
        buffer[i] = i; // 将数据填充到缓冲区
    }

    /* 连接成功了 */
    int nbytes;
    // 尝试通过套接字发送数据
    if ((nbytes = write(clientsocket, buffer, BUFFER_SIZE)) == -1) {
        fprintf(stderr, "send Error: %s\n", strerror(errno)); // 输出错误信息并退出程序
        exit(1);
    }

    int lengsize = 0;
    printf("client start to read data \n");

    // 从服务器端读取数据
    while ((lengsize = read(clientsocket, buffer, BUFFER_SIZE)) > 0) 
    {
        for (i = 0; i < BUFFER_SIZE; i++) 
        {
            // 按照一定格式输出读取到的数据
            if (i % 4 == 0) 
            {
                printf("\n");
            }
            printf(" %-4d", buffer[i]);
        }
        printf("\n");
    }

    // 尝试以二进制只读模式打开文件 "H264.ts"
    if ((stream = fopen("H264.ts", "rb")) == NULL) 
    {
        printf("The file 'H264.ts' was not opened! \n");
    }
    else 
    {
        bzero(buffer, BUFFER_SIZE);
        int lengsize = 0;
        
        // 从文件中读取数据并发送给服务器端
        while ((lengsize = fread(buffer, sizeof(char), BUFFER_SIZE, stream)) > 0) 
        {
            printf("lengsize = %d\n", lengsize);
            // 尝试通过套接字发送数据
            if (send(clientsocket, buffer, lengsize, 0) < 0) 
            {
                printf("Send File is Failed\n");
                break;
            }
            else 
            {
                bzero(buffer, BUFFER_SIZE);
            }
        }
        fclose(stream);
        printf("The file 'H264.ts' transfer finished! \n");
    }

    // 关闭套接字连接
    close(clientsocket);

    return 0;
}
```

# 自动获取IP地址

```c

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

```

