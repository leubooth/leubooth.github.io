---
title: Unity兼容IPv6解决办法
date: 2016-6-15 12:45:35
tags:
---

## Unity-IPv6
互联网通信协议第6版（英文：Internet Protocol version 6，缩写：IPv6）是互联网协议的最新版本，用于分组交换互联网络的网络层协议，旨在解决IPv4地址枯竭问题。

Apple最近宣布从2016年6月1日起所有的提交到App Store的应用必须支持纯IPv6的网络。
Apple这项规定针对所有新提交的应用或对已提交应用的更新。

本文旨在记录过渡时期Unity工程的IPv6兼容解决方法。

## Unity && Socket

如果Unity中使用了Socket连接，需要通过下面步骤实现兼容：

1.Mac系统下打开XCode，创建Object-C文件，命名ipv6.mm，写入以下代码：

``` [ipv6.mm]
#import <sys/socket.h>
#import <netdb.h>
#import <arpa/inet.h>
#import <err.h>

#define MakeStringCopy( _x_ ) ( _x_ != NULL && [_x_ isKindOfClass:[NSString class]] ) ? strdup( [_x_ UTF8String] ) : NULL
extern "C"{
    const char* getIPv6(const char *mHost,const char *mPort)
    {
        if( nil == mHost )
            return NULL;
        const char *newChar = "No";
        const char *cause = NULL;
        struct addrinfo* res0;
        struct addrinfo hints;
        struct addrinfo* res;
        int n, s;
        
        memset(&hints, 0, sizeof(hints));
        
        hints.ai_flags = AI_DEFAULT;
        hints.ai_family = PF_UNSPEC;
        hints.ai_socktype = SOCK_STREAM;
        //此处是IOS的关键函数
        if((n=getaddrinfo(mHost, "http", &hints, &res0))!=0)
        {
            printf("getaddrinfo error: %s\n",gai_strerror(n));
            return NULL;
        }
        
        struct sockaddr_in6* addr6;
        struct sockaddr_in* addr;
        NSString * NewStr = NULL;
        char ipbuf[32];
        s = -1;
        for(res = res0; res; res = res->ai_next)
        {
            if (res->ai_family == AF_INET6)
            {
                addr6 =( struct sockaddr_in6*)res->ai_addr;
                newChar = inet_ntop(AF_INET6, &addr6->sin6_addr, ipbuf, sizeof(ipbuf));
                NSString * TempA = [[NSString alloc] initWithCString:(const char*)newChar
                                                            encoding:NSASCIIStringEncoding];
                NSString * TempB = [NSString stringWithUTF8String:"&&ipv6"];
                
                NewStr = [TempA stringByAppendingString: TempB];
                printf("%s\n", newChar);
            }
            else
            {
                addr =( struct sockaddr_in*)res->ai_addr;
                newChar = inet_ntop(AF_INET, &addr->sin_addr, ipbuf, sizeof(ipbuf));
                NSString * TempA = [[NSString alloc] initWithCString:(const char*)newChar
                                                            encoding:NSASCIIStringEncoding];
                NSString * TempB = [NSString stringWithUTF8String:"&&ipv4"];
                
                NewStr = [TempA stringByAppendingString: TempB];
                printf("%s\n", newChar);
            }
            break;
        }
        
        
        freeaddrinfo(res0);
        
        printf("getaddrinfo OK");
        
        NSString * mIPaddr = NewStr;
        return MakeStringCopy(mIPaddr);
    }
}
```

2.将ipv6.mm放入Unity工程目录下Assets/Plugins/iOS/的位置，如果没有该目录，手动创建。

3.创建新的C#脚本，编写静态方法供调用。

``` [CompatibilityIP.cs]
using UnityEngine;
using System.Runtime.InteropServices; //引入此程序集可以调用IOS代码了。
using System.Net.Sockets;
using System;
using System.Text.RegularExpressions;

public class CompatibilityIP
{
	[DllImport("__Internal")]
	private static extern string getIPv6(string host, string port);
	
	private static string GetIPv6(string host, string port)
	{
		#if UNITY_IPHONE && !UNITY_EDITOR
		return getIPv6(host, port);
		#endif
		return host + "&&ipv4";
	}
	
	public static void GetIpType(string serverIp,string serverPort,out  string newServerIp,out AddressFamily newServerAddressFamily)
	{
		newServerAddressFamily = AddressFamily.InterNetwork;
		newServerIp = serverIp;
		try
		{
			string mIPv6 = GetIPv6(serverIp, serverPort);
			if (!string.IsNullOrEmpty(mIPv6))
			{
				string[] strTemp = Regex.Split(mIPv6, "&&");
				if (strTemp.Length >= 2)
				{
					string type = strTemp[1];
					if (type == "ipv6")
					{
						newServerIp = strTemp[0];
						newServerAddressFamily = AddressFamily.InterNetworkV6;
					}
				}
			}
		}
		catch (Exception e)
		{
			Debug.LogError(e.Message);
		}
	}
}
```

4.Socket方法调整。

``` [CompatibilityIP.cs]
    //转化前IP
    string ip;
    
    //转化后IP
    string serverIP;
    
    //端口
    string serverPort;
    //类型
    AddressFamily ipType;
    
    CompatibilityIP.GetIpType(ip, serverPort, out serverIP, out ipType);
    
    mSocket = new Socket(ipType, SocketType.Stream, ProtocolType.Tcp);
    
    IPAddress ip = IPAddress.Parse(serverIP);
    
    try
    {
        mSocket.Connect(new IPEndPoint(ip, port));
    }
    catch (System.Exception ex)
    {
    }
```

5.IPv6测试环境搭建参考苹果的官方文档
- [https://developer.apple.com/library/ios/documentation/NetworkingInternetWeb/Conceptual/NetworkingOverview/UnderstandingandPreparingfortheIPv6Transition/UnderstandingandPreparingfortheIPv6Transition.html#//apple_ref/doc/uid/TP40010220-CH213-SW23](https://developer.apple.com/library/ios/documentation/NetworkingInternetWeb/Conceptual/NetworkingOverview/UnderstandingandPreparingfortheIPv6Transition/UnderstandingandPreparingfortheIPv6Transition.html#//apple_ref/doc/uid/TP40010220-CH213-SW23)


## Unity && WWW

- 如果Unity使用的是WWW或者UnityWebRequest而无Socket代码，则可将Unity版本升级到指定版本即可，无需做代码改动。Unity特定版本的WWW&&UnityWebRequest已经加入对IPv6的兼容和支持。
        
        Unity4.7.2

	    Unity5.1.5
	
	    Unity5.2.5
	
	    Unity5.3.4p4
	    
## 参考链接

- [http://www.manew.com/thread-90569-1-1.html](http://www.manew.com/thread-90569-1-1.html)
- [http://www.codeinsect.net/blog/2016/05/26/unity-ipv6-socket-%E6%94%AF%E6%8C%81%EF%BC%8C%E5%B7%B2%E6%B5%8B%E8%AF%95%E9%80%9A%E8%BF%87/](http://www.codeinsect.net/blog/2016/05/26/unity-ipv6-socket-%E6%94%AF%E6%8C%81%EF%BC%8C%E5%B7%B2%E6%B5%8B%E8%AF%95%E9%80%9A%E8%BF%87/)
- [https://zh.wikipedia.org/wiki/IPv6](https://zh.wikipedia.org/wiki/IPv6)