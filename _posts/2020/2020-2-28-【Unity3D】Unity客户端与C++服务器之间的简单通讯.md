---
layout: post
category: Unity3D-Daily
title: 【Unity3D】Unity客户端与C++服务器之间的简单通讯
tagline: by 恬静的小魔龙
tag: Unity3D
---

C++服务器端
```
// 服务器

# pragma once
using namespace std;
# include <iostream>
# include <string>
# include <stdio.h>
# include <winsock2.h>
# pragma comment(lib,”ws2_32.lib”)

# include “Tool.h”

void main()
{
WSAData    wsadata;
SOCKET    ListeningSocket;
SOCKET    newConnection;

sockaddr_in serverAddr;
sockaddr_in clientAddr;

//    Struct sockaddr_in
//    {
//    short    sin_family;     Sin_family:代表协议族，一般为AF_INET.代表使用TCP/IP协议族
//    u_short    sin_port;    Sin_port: 代表端口号16位。注意字节序
//    struct in_addr    sin_addr;    Sin_addr:代表32位IPV4地址。
//    char    sin_zero[8];    Sin_zero:8个字节的0补充。
//    }

int    clientAddrLen = sizeof(sockaddr_in);
int    port = 5150;
int    ret;
char    dataBuffer[1024];

//    int WSAStartup(WORD version, LPWSADATA lpWSAData);

/*    此函数在应用程序中初始化Windows Sockets DLL ，只有此函数调用成功后，应用程序才可以再调用其他Windows Sockets DLL中的API函数。如果成功返回0。
Version 代表程序需要的Winsock的最高版本。其中主版本号在低
字节，次版本号在高字节。
比如：使用1.2的版本, version的值0x0201。

也可以使用MAKEWORD(2, 2)。
WORD MAKEWORD
(
BYTE bLow,
BYTE bHigh
);

lpWSAData 代表WSADATA的指针，里面包括本机系统的信息。它是返回值
struct WSAData 
{
WORD wVersion;    代表建议使用的版本号
WORD wHighVersion;           代表系统最高支持的版本号
char szDescription[WSADESCRIPTION_LEN + 1];
char szSystemStatus[WSASYSSTATUS_LEN + 1];
unsigned short iMaxSockets;
unsigned short iMaxUdpDg;
char FAR * lpVendorInfo;
};
*/

if ((ret = WSAStartup(MAKEWORD(2, 2), &wsadata)) != 0)
{
cout << “WSAStart up is failed with error\t” << ret << endl;
return;
}

/*    SOCKET socket(int af, int type, int protocol)
af:代表协议族，一般为AF_INET.
Type : 代表套接口类型，SOCK_STREAM流套接字， SOCK_DGRAM数据报套接字

流套接字上数据进出是无边界的，可靠的。
数据报套接字是有边界的，不可靠的（相对于流套接字而言）
流套接字是速度慢，数据报套字速度快。
流套接字是有序的，数据报套接字是无序的（后发的数据有可能先被接收到。）

所以一般tcp 用刘套接字，udp用数据报   关于udp协议，后章会给出（好吧，可能会给出，看有没有时间）
估计qq的消息用得是udp，具体没去研究—-

Protocol : 指定所用的协议.如：IPPROTO_TCP、PPROTO_UDP  
返回值：如果没有错误发生，返回一个新的套接字，
否则返回INVALID_SOCKET。
*/

// 创建一个监听套接字，所谓监听，就是看看有没有客户端连接上来
if (INVALID_SOCKET == (ListeningSocket = socket(AF_INET, SOCK_STREAM, IPPROTO_TCP)))
{
cout << “socket is falied with error\t” << WSAGetLastError() << endl;
WSACleanup();
return;
}

//    本机系统所用的字节序称为：本机字节顺序
//    网络标准使用的字节序称为：网络字节顺序

/*    htons()——本机到网络（short）
htonl()——本机到网络（Long）
ntohs()——网络到本机（short）
ntohl()——网络到本机（long）
*/

serverAddr.sin_family = AF_INET;
serverAddr.sin_port = htons(port);
serverAddr.sin_addr.s_addr = htonl(INADDR_ANY);
//INADDR_ANY就是指定地址为0.0.0.0的地址，这个地址事实上表示不确定地址，或“所有地址”、“任意地址”。 一般来说，在各个系统中均定义成为0值。

//    绑定端口
//    bind(SOCKET s, const struct sockaddr FAR *name, int namelen);

//    接下来要为服务器端定义的这个监听的socket指定一个地
//    及端口(Port)，这样客户端才知道待会要连接哪一个地址的哪个
//    端口，为此我们要调用bind()函数，该函数调用成功返回0，否则
//    返回SOCKET_ERROR

if (SOCKET_ERROR == bind(ListeningSocket, (sockaddr*)&serverAddr, sizeof(serverAddr)))
{
cout << “bind is falied with error\t” << WSAGetLastError() << endl;
closesocket(ListeningSocket);
WSACleanup();
return;
}

//    监听
//    int  listen( SOCKET s, int backlog );

//    服务器端的socket对象绑定完成之后,服务器端必须建立一个监听的队列来接收客户端的连接请求。
//    listen()函数使服务器端的socket 进入监听状态，并设定可以建立的最大连接数(目前最大值限制为 5, 最小值为1)。
//    该函数调用成功返回0，否则返回SOCKET_ERROR。

if (SOCKET_ERROR == listen(ListeningSocket, 5))
{
cout << “listen is falied with error\t” << WSAGetLastError() << endl;
closesocket(ListeningSocket);
WSACleanup();
return;
}

cout << “now we are waiting a connection from client” << endl;

cout <<“sizeof sockaddr_in:\t”<< sizeof(sockaddr_in) << “\tsizeof sockaddr\t” << sizeof(sockaddr) << endl;

//    服务器端接受客户端的连接请求
//    SOCKET  accept( SCOKET s, struct sockaddr FAR *addr,int FAR *addrlen );
//    为了使服务器端接受客户端的连接请求，就要使用accept() 函数，
//    该函数新建一个socket与客户端的socket相通，原先监听之socket继续进入监听状态，
//    等待他人的连接要求。该函数调用成功返回一个新产生的socket对象，否则返回INVALID_SOCKET。
if (INVALID_SOCKET == (newConnection = accept(ListeningSocket, (sockaddr*)&clientAddr, &clientAddrLen)))
{
cout << “accept is falied with error\t” << WSAGetLastError() << endl;
closesocket(ListeningSocket);
WSACleanup();
return;
}

cout << “get a connect from client addr\t” << inet_ntoa(clientAddr.sin_addr) << “\t” << ntohs(clientAddr.sin_port) << endl;

//    数据的接收
//    int recv( SOCKET s, char FAR *buf, int len, int flags );

//    参数：
//    s为socket 的识别码 
//    buf：存放接收到的信息的暂存区 
//    len:buf的长度 
//    flags：此函数被调用的方式
//    返回值就是接受的字符串的长度（在没出错的情况下）

if (SOCKET_ERROR == (ret =  recv(newConnection, dataBuffer, sizeof(dataBuffer), 0)))
{
cout << “recv is errow with\t” << WSAGetLastError() << endl;
closesocket(newConnection);
WSACleanup();
return;
}

dataBuffer[ret] = ‘\0’;

//wchar_t *temp = Tool::getSingleTon()->charToWchar(dataBuffer);

//cout << “ret:\t” << ret << “\tdata\t” << temp << endl;

//  暂时不知道怎么解析unity客户端发过来的中文，，，忧伤的不行,试了好几种方法，有知道的可以告诉我下

cout << dataBuffer << endl;

//    结束 socket 连接

//    结束服务器和客户端的通信连接是很简单的，这一过程可以由服务器或客户机的任一端启动，
//    只要调用closesocket()就可以了，而要关闭Server端监听状态的socket，同样也是利用此函数。
//    另外，与程序启动时调用WSAStartup()憨数相对应，程式结束前，需要调用 WSACleanup()
//    来通知Winsock Stack释放socket所占用的资源。这两个函数都是调用成功返回0，否则返回SOCKET_ERROR。

//    int PASCAL FAR closesocket(SOCKET s);
//    参 数：s为socket 的识别码；
//    int PASCAL FAR WSACleanup(void);

closesocket(newConnection);

WSACleanup();
}
```
unity客户端

```
//unity客户端

using UnityEngine;
using System.Collections;
using System.Text;
using System.Net;
using System.Net.Sockets;
using System.IO;

public class TestScoket : MonoBehaviour
{

    void Start()
    {
        ConncetServer();
    }

    void ConncetServer()
    {
        IPAddress ipAdr = IPAddress.Parse(“10.0.0.22”);

        IPEndPoint ipEp = new IPEndPoint(ipAdr, 5150);

        Socket clientScoket = new Socket(AddressFamily.InterNetwork, SocketType.Stream, ProtocolType.Tcp);

        clientScoket.Connect(ipEp);

        string output = “zhangsan”;//  中文暂时不知道怎么解析  “中文”

        byte[] concent = Encoding.UTF8.GetBytes(output);

        //byte[] concent = Encoding.Unicode.GetBytes(output);
        int count = clientScoket.Send(concent);

        Debug.LogError(count);

        //byte[] response = new byte[1024];

        //int bytesRead = clientScoket.Receive(response);

        //string input = Encoding.UTF8.GetString(response, 0, bytesRead);

        //print(“Client request:” + input);
        clientScoket.Shutdown(SocketShutdown.Both);
        clientScoket.Close();
    }

    private void ConnectCallBack(System.IAsyncResult ar)
    {
        Debug.LogError(“连接成功”);
    }
}
```