---
title: "使用fork的server"
description: "unix网络"
date: 2022-05-16T08:08:08+08:00
---
> GNU Compiler Collection(GCC)

在“[Socket服务器端程序](https://tdtc-hrb.github.io/csdn/post/c-bsd_socket/)”基础上增加子进程。

[Beej’s Guide Network to Programming](https://blog.csdn.net/xiaobin_hlj80/category_6092337.html)
===
> 关于Unix/Linux子进程：
>> 相当于Windows的线程

  调用fork()即可增加子进程。

  要使用fork必须调用waitpid() ！

# waitpid()
waitpid函数原型：
```c
#include<sys/types.h>   /* 提供类型pid_t的定义 */
#include<sys/wait.h>

pid_t waitpid(pid_t pid, int *status, int options);
```

- pid    
等于-1时，等待任何一个子进程退出，没有任何限制，此时waitpid和wait的作用一模一样。

- status    
我们设置为NULL。

- options    
提供了一些额外的选项来控制waitpid，WNOHANG常量表示，
即使没有子进程退出，它也会立即返回，不会像wait那样永远等下去。

# sigaction()
> sigaction()是waitpid()的开关

  只有给waitpid()信号时，才开启它(waitpid)！
  这时，需要sigaction()。

sigaction函数原型：
```c
#include<signal.h>
int sigaction(int signum, const struct sigaction *act, struct sigaction *oldact);
```

- signum    
等于SIGCHLD时，只有子进程停止或者退出才进行调用。

- act    
一个指向sigaction结构的指针。

- oldact    
一般废止不用。

我们主要设置结构sigaction的sa_handler、sa_mask和sa_flags三个成员变量即可！

# code
```c
/**
 * Version: 0.2
 *
 * Description: Add signal
 *
 */

#include <stdio.h>
#include <string.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <netdb.h>
#include <arpa/inet.h>
#include <signal.h>
#include <sys/wait.h>
#include <stdlib.h>		/* exit declare */
#include <unistd.h>		/* fork declare */

#define SERVPORT "2349"

void sigchild_handler()
{
	while (waitpid(-1, NULL, WNOHANG) > 0);
}

int main(int argc, char *argv[])
{
	struct addrinfo hints, *res;
	int status;
	int sockfd;

	int connFd;
	/* struct sockaddr_in cliAddr;  Only IPv4  */
	struct sockaddr_storage clientAddr;	/* both IPv4 and IPv6 */

	struct sigaction sa;

	int sendSta;
	char *msg;

	if (argc != 2) {
		fprintf(stderr, "Usage: Not found Read File");
		return 1;
	}

	memset(&hints, 0, sizeof(hints));
	hints.ai_family = AF_UNSPEC;
	hints.ai_socktype = SOCK_STREAM;
	hints.ai_flags = AI_PASSIVE;

	status = getaddrinfo(NULL, SERVPORT, &hints, &res);

	if (status != 0) {
		fprintf(stderr, "getaddrinfo, fail!");
		return 2;
	}

	sockfd = socket(res->ai_family, res->ai_socktype, res->ai_protocol);

	bind(sockfd, res->ai_addr, res->ai_addrlen);

	listen(sockfd, 5);

	printf("======== Please Wait Client =========\n");

	/* struct sigaction notes from POSIX:
 	 *
 	 *  (1) Routines stored in sa_handler should take a single int as
 	 *      their argument although the POSIX standard does not require this.
 	 *  (2) The fields sa_handler and sa_sigaction may overlap, and a conforming
 	 *      application should not use both simultaneously.
 	 */
	sa.sa_handler = sigchild_handler;
	sigemptyset(&sa.sa_mask);	/* Additional set of signals to be blocked */
                              	/* during execution of signal-catching function. */
	sa.sa_flags = SA_RESTART;	/* Special flags to affect behavior of signal */
								/* SA_RESTART 0x10000000 	// Restart syscall on signal return */

	if (sigaction(SIGCHLD, &sa, NULL) == -1) {	/* SIGCHLD 20	// to parent on child stop or exit */
		fprintf(stderr, "sigaction fail!");
		exit(3);
	}

	while(1) { // loop forever!
		char ipstr[INET_ADDRSTRLEN];
		void *addr;

		int len = sizeof(clientAddr);
		connFd = accept(sockfd, (struct sockaddr *)&clientAddr, &len);

		/* View Client IP */
		struct sockaddr_in *ipv4 = (struct sockaddr_in *)&clientAddr;
		addr = &(ipv4->sin_addr);

		inet_ntop(AF_INET, addr, ipstr, sizeof(ipstr));
		printf("client: %s\n", ipstr);

		if (!fork()) {
			close(sockfd);	/* child doesn't need the listener */
			/* Copy Data */
			msg = "The server simply displays a message!";
			sendSta = send(connFd, msg, strlen(msg), 0);
			if (sendSta == -1)
				fprintf(stderr, "send fail!");
			close(connFd);
			exit(0);
		}

		close(connFd);
	}

	close(sockfd);
	return 0;
}
```

## 测试Windows客户端程序
- Server : WSL (Windows 1809 LTSC) - Ubuntu 18.04.1
- [Client code](https://tdtc-hrb.github.io/csdn/post/vc-bsd_socket/)

```c++
/**
 * Version: 0.2
 *
 * Description: Add signal
 * file: bgnetServ.c
 */
/**
 * Version: 0.2.1
 *
 * New features:
 * Receive messages from the Windows client program(prj1Clt.exe/prj2Clt.exe).
 * 
 * file: bgnetServ2.c
 */

/**
 * $ gcc bgnetServ2.c -o server2.o
 * $ ./server2.o
 */

#include <stdio.h>
#include <string.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <netdb.h>
#include <arpa/inet.h>
#include <signal.h>
#include <sys/wait.h>
#include <stdlib.h>		/* exit declare */
#include <unistd.h>		/* fork declare */

#define SERVPORT "3293"

void sigchild_handler()
{
	while (waitpid(-1, NULL, WNOHANG) > 0);	
}

int main(int argc, char *argv[])
{
	struct addrinfo hints, *res;
	int status;
	int sockfd;
	
	int connFd;
	/* struct sockaddr_in cliAddr;  Only IPv4  */
	struct sockaddr_storage clientAddr;	/* both IPv4 and IPv6 */
	
	struct sigaction sa;
	
	int recvSta;
	char buffer[1024];
	
	int sendSta;
	char *msg;
	
	/*
	if (argc != 2) {
		fprintf(stderr, "Usage: Not found Read File");
		return 1;
	}
	*/
	memset(&hints, 0, sizeof(hints));
	hints.ai_family = AF_UNSPEC;
	hints.ai_socktype = SOCK_STREAM;
	hints.ai_flags = AI_PASSIVE;
	
	status = getaddrinfo(NULL, SERVPORT, &hints, &res);
	
	if (status != 0) {
		fprintf(stderr, "getaddrinfo, fail!");
		return 2;
	}
	
	sockfd = socket(res->ai_family, res->ai_socktype, res->ai_protocol);
	
	bind(sockfd, res->ai_addr, res->ai_addrlen);
	
	listen(sockfd, 5);
	
	printf("======== Please Wait Client =========\n");
	
	/* struct sigaction notes from POSIX:
 	 *
 	 *  (1) Routines stored in sa_handler should take a single int as
 	 *      their argument although the POSIX standard does not require this.
 	 *  (2) The fields sa_handler and sa_sigaction may overlap, and a conforming
 	 *      application should not use both simultaneously.
 	 */
	sa.sa_handler = sigchild_handler;	
	sigemptyset(&sa.sa_mask);	/* Additional set of signals to be blocked */
                              	/* during execution of signal-catching function. */
	sa.sa_flags = SA_RESTART;	/* Special flags to affect behavior of signal */
								/* 0x10000000 	// Restart syscall on signal return */
								
	if (sigaction(SIGCHLD, &sa, NULL) == -1) {	/* SIGCHLD 20	// to parent on child stop or exit */
		fprintf(stderr, "sigaction fail!");
		exit(3);	
	}
	
	while(1) { // loop forever!
		char ipstr[INET_ADDRSTRLEN];
		void *addr;
		
		int len = sizeof(clientAddr);
		connFd = accept(sockfd, (struct sockaddr *)&clientAddr, &len);
		
		/* View Client IP */
		struct sockaddr_in *ipv4 = (struct sockaddr_in *)&clientAddr;
		addr = &(ipv4->sin_addr);
	
		inet_ntop(AF_INET, addr, ipstr, sizeof(ipstr));
		printf("client: %s\n", ipstr);
		
		recvSta = recv(connFd, buffer, sizeof(buffer), 0);
		if(recvSta != -1)
			printf("\n%s\n", buffer);
		
		if (!fork()) {
			close(sockfd);	/* child doesn't need the listener */
			/* Copy Data */
			msg = "The Server Simply displays a message!\r";
			sendSta = send(connFd, msg, strlen(msg), 0);
			if (sendSta == -1)
				fprintf(stderr, "send fail!");
			close(connFd);
			exit(0);
		}
		
		close(connFd);
	}
	
	close(sockfd);
	return 0;
}

```
