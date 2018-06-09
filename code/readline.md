```c
struct packet
{
    int len;
    char buf[1024];
};

// ssize_t 无符号整数，size_t有符号整数
// 从fd中读
// 读到buf中
// 读count中字节
// 返回已读到的字节数
ssize_t readn(int fd, void *buf, size_t count)
{
    size_t nleft = count;	//剩余要读取的字节数
    ssize_t nread;				//已读字节数
    char *bufp = (char*)buf;

    while (nleft > 0)
    {
        if ((nread = read(fd, bufp, nleft)) < 0)//可能被信号中断，可能出错
        {
            if (errno == EINTR)//信号中断
                continue;
            return -1;//读失败
        }
        else if (nread == 0)//读到EOF,即对等方关闭
            return count - nleft;

        bufp += nread;	//指针偏移
        nleft -= nread;	//剩余字节数
    }

    return count;//返回
}

ssize_t writen(int fd, const void *buf, size_t count)
{
    size_t nleft = count;	//剩余要发送的字节数
    ssize_t nwritten;			//已写字节数
    char *bufp = (char*)buf;

    while (nleft > 0)
    {
        if ((nwritten = write(fd, bufp, nleft)) < 0)
        {
            if (errno == EINTR)//信号中断
                continue;
            return -1;
        }
        else if (nwritten == 0)//nwritten==0，相当于什么都没做
            continue;

        bufp += nwritten;
        nleft -= nwritten;
    }

    return count;
}

//对recv的封装，偷窥缓冲区中的内容,每次偷窥数据长度不一定是len,有数据则返回，返回值是偷窥到的数据长度，没有数据则阻塞
//recv取走数据后不将数据从缓冲区中移除
//只能用于从socket套接字中读取数据
//参数：套接字，数据偷窥存放的区，要读取的长度
//返回：0:套接口关闭; <0:失败, >0:偷窥到的长度
ssize_t recv_peek(int sockfd, void *buf, size_t len)
{
	while (1)
	{
		
		int ret = recv(sockfd, buf, len, MSG_PEEK);//MSG_PEEK表示偷窥，并不从缓冲区中读走数据
		if (ret == -1 && errno == EINTR)		//返回值为-1说明接收失败，errno == EINTR说明被信号中断
			continue;//继续做一遍
		return ret;//返回已读到的字节数
	}
}

//利用recv_peek封装一个readline函数
//maxline一行最大字节数
ssize_t readline(int sockfd, void *buf, size_t maxline)
{
	int ret;						//偷窥的到数据长度
	int nread;					//接收到的字节数
	char *bufp = buf;		//定义一个指针指向buf
	int nleft = maxline;//未读取的节数，一行最大的字节数
	while (1)	//循环读取
	{
		//
		ret = recv_peek(sockfd, bufp, nleft);//返回已偷窥到的字节数
		if (ret < 0)				//失败
			return ret;
		else if (ret == 0)	//套接口关闭
			return ret;

		nread = ret;				//接收到的字节数为recv_peek的返回值
		
		//下面判断接收到的缓冲区是否有\n，
		int i;						
		for (i=0; i<nread; i++)
		{
			if (bufp[i] == '\n')
			{
				ret = readn(sockfd, bufp, i+1);//读i+1个字符，包括\n
				if (ret != i+1)	//ret 为读到的字符数，必然等i+1
					exit(EXIT_FAILURE);

				return ret;
			}
		}

		//没有\n也从缓冲区中将数据读出，直到读到\n返回
		if (nread > nleft)//当前读到的字节数据不可能超过剩余字节数
			exit(EXIT_FAILURE);

		nleft -= nread;
		ret = readn(sockfd, bufp, nread);//读走nread长度的数据
		if (ret != nread) //些时读到的长度应和要读的数据长度相等
			exit(EXIT_FAILURE);

		bufp += nread;//指针偏移，不然读到内容会被覆盖
	}

	return -1;//如果程序运行到这里说明出错了
}

```
