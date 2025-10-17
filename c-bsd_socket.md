---
title: "unix网络的client和server"
description: "addrinfo"
date: 2023-02-14T00:08:08+08:00
---
使用GNU Compiler Collection(GCC)编译的
一个简单的客户端和服务器端的程序！

# Client Side

- flow chart
```
  getaddrinfo() -> socket() -> connect() -> recv()
```
- src
```c
#include <stdio.h>
#include <string.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <arpa/inet.h>
#include <netdb.h>

#define SERVIP   "192.168.56.112"
#define SERVPORT "2349"

int main(int argc, char *argv[])
{
	struct addrinfo hints, *res;
	int status;
	int sockfd;
	int connSta;

	int recvSta;
	char buffer[1024];
	int len;

	char ipstr[INET_ADDRSTRLEN];
	void *addr;

	if (argc != 3) {
		fprintf(stderr, "Usage: Not Server IP And Port!");
		return 1;
	}

	memset(&hints, 0, sizeof(hints));
	hints.ai_family = AF_UNSPEC;
	hints.ai_socktype = SOCK_STREAM;

	status = getaddrinfo(argv[1], argv[2], &hints, &res);

	if (status != 0) {
		fprintf(stderr, "Error, Server IP And Port!");
		return 2;
	}

	sockfd = socket(res->ai_family, res->ai_socktype, res->ai_protocol);

	if (sockfd == 0) {
		fprintf(stderr, "Create, socket fail!");
		return 3;
	}

	connSta = connect(sockfd, res->ai_addr, res->ai_addrlen);

	if (connSta != 0) {
		fprintf(stderr, "Connect, Remote Host fail!");
		return 4;
	}
	else {
		struct sockaddr_in *ipv4 = (struct sockaddr_in *)res->ai_addr;
		addr = &(ipv4->sin_addr);

		inet_ntop(res->ai_family, addr, ipstr, sizeof(ipstr));

		printf("Linked: Host = %s, Port = %d\n", ipstr, ntohs(ipv4->sin_port));
	}


	recvSta = recv(sockfd, buffer, sizeof(buffer), 0);

	printf("\n%s\n", buffer);

	freeaddrinfo(res);
	close(sockfd);

	return 0;
}
```

# Server Side
- flow chart
```
getaddrinfo() -> socket() -> bind() -> listen() -> accept() -> send()
```
- src
```c
#include <stdio.h>
#include <string.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <netdb.h>
#include <arpa/inet.h>

#define SERVPORT "2349"

int main(int argc, char *argv[])
{
	struct addrinfo hints, *res;
	int status;
	int sockfd;

	int connFd;
	struct sockaddr_in cliAddr;

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

	while(1) { // loop forever!
		char ipstr[INET_ADDRSTRLEN];
		void *addr;

		int len = sizeof(cliAddr);
		connFd = accept(sockfd, (struct sockaddr *)&cliAddr, &len);

		/* View Client IP */
		struct sockaddr_in *ipv4 = (struct sockaddr_in *)&cliAddr;
		addr = &(ipv4->sin_addr);

		inet_ntop(AF_INET, addr, ipstr, sizeof(ipstr));
		printf("client: %s\n", ipstr);

		/* Copy Data */
		msg = "Hello world!";
		sendSta = send(connFd, msg, strlen(msg), 0);

		close(connFd);
	}

	close(sockfd);
	return 0;
}
```


# Ref
- [Beej’s Guide Network to Programming- chapter 6](https://beej.us/guide/bgnet/html/split/client-server-background.html)
