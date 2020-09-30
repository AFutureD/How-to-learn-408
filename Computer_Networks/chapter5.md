# 第五章 传输层
- 5.1 传输层提供的服务
    - 5.1.1 传输层的功能
        - 概述
            - 从通信和信息处理的角度看,传输层向它上面的应用层提供通信服务,它属于面向通信部分 的最高层,同时也是用户功能中的最低层。
            - 为运行在不同主机上的进程之间提供了逻辑通信,而网络层提供 主机之间的逻辑通信。
            - 显然,即使网络层协议不可靠(网络层协议使分组丢失、混乱或重复), 传输层同样能为应用程序提供可靠的服务。
            - 网络的边缘部分的两台主机使用网络核心部分的功能进行端到端的通信 时,只有主机的协议栈才有传输层和应用层,而路由器在转发分组时都只用到下三层的功能(即 在通信子网中没有传输层,传输层只存在于通信子网以外的主机中)。
                - ![](https://firebasestorage.googleapis.com/v0/b/firescript-577a2.appspot.com/o/imgs%2Fapp%2FAFuture%2F5zFmLg7v93.png?alt=media&token=b0a154ed-0a7d-4ef5-9c90-f2ddfad2270a)
            - 向高层用户屏蔽了低层网络核心的细节(如网络拓扑、路由协议等),它使应用进程 看见的是好像在两个传输层实体之间有一条端到端的逻辑通信信道,这条逻辑通信信道对上层的 表现却因传输层协议不同而有很大的差别。
            - 当传输层采用面向连接的TCP时,尽管下面的网络是 不可靠的,但这种逻辑通信信道就相当于一条全双工的可靠信道。但当传输层采用无连接的UDP 时,这种逻辑通信信道仍然是一条不可靠信道。
        - 功能
            - 1) 传输层提供应用进程之间的逻辑通信(即端到端的通信)。
                - 与网络层的区别是,网络层提 供的是主机之间的逻辑通信。
                - 从网络层来说,通信的双方是两台主机,IP数据报的首部给出了这两台主机的IP地址。
                    - 但“两台主机之间的通信”实际上是两台主机中的应用进程之间的通信,应用进程之间 的通信又称端到端的逻辑通信。
            - 2)  复用和分用。
                - 复用是指发送方不同的应用进程都可使用同一个传输层协议传送数据;
                - 分用是指接收方的传输层在剥去报文的首部后能够把这些数据正确交付到目的应用进程。
            - 3) 传输层还要对收到的报文进行差错检测(首部和数据部分)。
                - 而网络层只检查IP数据报 的首部,不检验数据部分是否出错。
            - 4) 提供两种不同的传输协议,即面向连接的TCP和无连接的UDP。
                - 网络层无法同时实现 两种协议(即在网络层要么只提供面向连接的服务,如虚电路;要么只提供无连接服务, 如数据报,而不可能在网络层同时存在这两种方式)。
    - 5.1.2 传输层的寻址与端口
        - l. 端口的作用
            - 标识主机中的应用进程
            - 端口能够让应用层的各种应用进程将其数据通过端口向下交付给传输层,以及让传输层知道 应当将其报文段中的数据向上通过端口交付给应用层相应的进程。
            - 传输层服务访问点 (TSAP),它在传输层的作用类似于IP地址在网络层的作用或MAC地址在数据链路层的作用, 只不过IP地址和MAC地址标识的是主机,而端口标识的是主机中的应用进程。
                - 数据链路层的SAP是MAC地址,网络层的SAP是IP地址,传输层的SAP是端口。
            - 在协议栈层间的抽象的协议端口是软件端口,它与路由器或交换机上的硬件端口是完全不同 的概念。
                - 硬件端口是不同硬件设备进行交互的接口,而软件端日是应用层的各种协议进程与传输 实体进行层间交互的一种地址。传输层使用的是软件端口。
        - 2. 端口号
            - 长度为16bit,能够表示65536(2^16)个不同的端口号
            - 只具有本地意义,即端口号只标识本计算机应用层中的各进程
            - 两类
                - 1)服务端使用的端口号。
                    - 熟知端口号
                        - 数值为0〜1023
                        - ![](https://firebasestorage.googleapis.com/v0/b/firescript-577a2.appspot.com/o/imgs%2Fapp%2FAFuture%2FJFM1_NrB5o.png?alt=media&token=c5a34d73-5bad-4c33-bf6a-2a4323e4c46b)
                    - 登记端口号
                        - 数值为1024〜49151
                        - 供没有熟知端 口号的应用程序使用的,使用这类端口号必须在IANA登记,以防止重复。
                - 2) 客户端使用的端口号,
                    - 数值为49152〜65535
                    - 这类端口号仅在客户进程运行时才动 态地选择,因此又称短暂端口号(也称临时端口)。
                    - 通信结束后,刚用过的客户端口号就 不复存在,从而这个端口号就可供其他客户进程使用。
        - 3.套接字
            - 网络中采用发送方和接收方的套接字( Socket) 组合来识别端点。
            - 所谓套接字, 实际上是一个通信端点;它唯一地标识网络中的一台主机和其上的一个应用(进程)。
                - 套接字=(主机IP地址,端口号)
    - 5.1.3 无连接服务与面向连接服务
        - 面向连接服务
            - 就是在通信双方进行通信之前,必须先建立连接,在通信过程中,整个连接的 情况一直被实时地监控和管理。通信结束后,应该释放这个连接。
        - 无连接服务
            - 指两个实体之间的通信不需要先建立好连接,需要通信时,直接将信息发送到 “网络〃中,让该信息的传递在网上尽力而为地往目的地传送。
        - TCP/IP协议族
            - 面向连接的传输控制协议(TCP)
                - 采用TCP时,传输层向上提供的是一条全双工的可靠逻辑信道;
                - 在传送数据之前必须先建立连接,数据传送结束后要释放连接。 
                - TCP不提供广播或组播服务
                - 由于TCP提供面向连接的可靠传输服务,因此不可避免地增加了许 多开销,如确认、流量控制、计时器及连接管理等。
                - 主要适用于可靠性更重要的场合,如文件传输协议(FTP) 、 超文本传输协议(HTTP) 、远程登录(TELNET) 等。
            - 无连接的用户数据报协 议(UDP)
                - 采用UDP时,传输层向上提供的是一条不可靠的逻辑信道。
                - 两个附加服务
                    - 多路复用
                    - 对 数据的错误检查
                - UDP在传送数据之前不需要先建立连接,远程主机的传输层收到UDP报文后,不需要给 出任何确认。
                - 执行速度比较快、实时性好。
                - 应用主要包括 小文件传送协议(TFTP) 、DNS、SNMP和实时传输协议(RTP) 。
- 5.2 UDP 协议
    - 5.2.1 UDP数据报
        - 1. UDP概述
            - 做了传输协议能够做的最少工作,它仅在IP的数据报服务之上增 加了两个最基本的服务:复用和分用以及差错检测。
            - 优点
                - 1) UDP无须建立连接。因此UDP不会引入建立连接的时延。
                - 2) 无连接状态。TCP需要在端系统中维护连接状态。此连接状态包括接收和发送缓存、拥 塞控制参数和序号与确认号的参数。而UDP不维护连接状态,也不跟踪这些参数。因此, 某些专用应用服务器使用UDP时,一般都能支持更多的活动客户机。
                - 3) 分组首部开销小。TCP有20B的百部开销,而UDP仅有8B的开销
                - 4) 应用层能更好地控制要发送的数据和发送时间。UDP没有拥塞控制,因此网络中的拥塞 不会影响主机的发送效率。
            - 特点
                - UDP常用于一次性传输较少数据的网络应用.如DNS、SNMP等
                - UDP也常用于多媒体应用(如IP电 话、实时视频会议、流媒体等)
                - UDP提供尽最大努力的交付,即不保证可靠交付,但这并不意味着应用对数据的要求是不可 靠的,因此所有维护传输可靠性的工作需要用户在应用层来完成。
                - UDP是面向报文的。发送方UDP对应用层交下来的报文,在添加首部后就向下交付给IP层, 既不合并,也不拆分,而是保留这些报文的边界;接收方UDP对IP层交上来UDP用户数据报, 在去除首部后就原封不动地交付给上层应用进程,一次交付一个完整的报文。因此报文不可分割, 是UDP数据报处理的最小单位。
        - 2. UDP的首部格式
            - ![](https://firebasestorage.googleapis.com/v0/b/firescript-577a2.appspot.com/o/imgs%2Fapp%2FAFuture%2FEiEybJPD7q.png?alt=media&token=efe9314c-6331-4340-8059-f60ddafe0698)
            - 1) 源端口。源端口号。在需要对方回信时选用,不需要时可用全0。
            - 2)  目的端口。目的端口号。这在终点交付报文时必须使用到。
                - 如果接收方UDP发现收到的报文中的目的端口号不正确(即不 存在对应于端口号的应用进程),那么就丢弃该报文,并由ICMP发 送“端口不可达”差错报文给发送方。
            - 3)  长度。UDP数据报的长度(包括首部和数据),其最小值是8 (仅有首部)。
            - 4)  校验和。检测UDP数据报在传输中是否有错。有错就丢弃。该字段是可选的,当源主机 不想计算校验和时,则直接令该字段为全0。
    - 5.2.2 UDP 校验
        - 在计算校验和时,要在UDP数据报之前增加12B的伪首部,伪首部并不是UDP的真正首部。
        - 只是在计算校验和时,临时添加在UDP数据报的前面,得到一个临时的UDP数据报。校验和就 是按照这个临时的UDP数据报计算的。
        - ![](https://firebasestorage.googleapis.com/v0/b/firescript-577a2.appspot.com/o/imgs%2Fapp%2FAFuture%2F0vLDKEKmFx.png?alt=media&token=1045817d-7521-4158-875b-24a1cc702c4a)
        - UDP校验和的计算方法
            - 发送方
                - Subtopic
                - 使用二进制反码运算求 和再取反。
                - 首先把全零放入校验和字段并添加伪首部,
                - 把UDP数据报视为许多16位的字连 接起来。
                - 若UDP数据报的数据部分不是偶数个字节,则要在数据部分末尾增加一个全零字节(但 此字节不发送)。
                - 按二进制反码计算出这些16位字的和,并将此和的二进制反码写入校验 和字段。
            - 接收方
                - 把收到的UDP数据报加上伪首部(如果不为偶数个字节,那么还需要补上全零 字节)后,
                - 按二进制反码计算出这些16位字的和。
                - 当无差错时其结果应全为L否则表明有差错 出现,接收方就应该丢弃这个UDP数据报。
- 5.3 TCP协议
    - 5.3.1 TCP协议的特点
        - 概述
            - 在不可靠的IP层之上实现的可靠的数据传输协议
            - 主要解决传输的可靠、有序、 无丢失和不重复问题。
        - 主要特点
            - 1) TCP是面向连接的传输层协议。
            - 2)  每条TCP连接只能有两个端点,每条TCP连接只能是点对点的(一对一)。
            - 3)  TCP提供可靠的交付服务,保证传送的数据无差错、不丢失、不重复且有序。
            - 4)  TCP提供全双工通信,允许通信双方的应用进程在任何时候都能发送数据,为此TCP连 接的两端都设有发送缓存和接收缓存,用来临时存放双向通信的数据。
                - 发送缓存用来暂时存放以下数据:
                    - 1. 发送应用程序传送给发送方TCP准备发送的数据;
                    - 2. TCP已发送但尚未收到确认的数据。
                - 接收缓存用来暂时存放以下数据:
                    - 1. 按序到达但 尚未被接收应用程序读取的数据;
                    - 2. 不按序到达的数据。
            - 5) TCP是面向字节流的,虽然应用程序和TCP的交互是一次一个数据块(大小不等),但 TCP把应用程序交下来的数据仅视为一连串的无结构的字节流。
    - 5.3.2 TCP报文段
        - 概述
            - TCP传送的数据单元称为报文段。
            - 一个TCP报文段分为TCP首部和TCP数据两部分
            - TCP报文段的首部最短为20B,后面有4N字节是根据需要而增加的选项,通常长 度为4B的整数倍。
        - 作用
            - TCP报文段既可以用来运载数据,又可以用来建立连接、释放连接和应答。
        - 示例
            - ![](https://firebasestorage.googleapis.com/v0/b/firescript-577a2.appspot.com/o/imgs%2Fapp%2FAFuture%2F5LJJyohIF9.png?alt=media&token=f657ec6c-83a4-4614-b5f7-7557db7c0ce7)
        - 各字段意义
            - 1) 源端口和目的端口字段。
                - 各占2B。
                - 端口是运输层与应用层的服务接口,运输层的复用和 分用功能都要通过端口实现。
            - 2) 序号字段。
                - 占4B。
                - TCP是面向字节流的(即TCP传送时是逐个字节传送的),所以TCP 连接传送的数据流中的每个字节都编上一个序号。序号字段的值指的是本报文段所发送 的数据的第一个字节的序号。
            - 3)  确认号字段。
                - 占4B
                - 期望收到对方的下一个报文段的数据的第一个字节的序号。若确认 号为M则表明到序号N- 1为止的所有数据都已正确收到。
            - 4)  数据偏移(即首部长度)。
                - 占4位
                - 表示 首部长度,它指出TCP报文段的数据起始处距离TCP报文段的起始处有多远。
                - 单位是32位(以4B为计算单位)。
            - 5)  保留字段。
                - 占6位,
                - 目前应置为0
            - 6)  紧急位URG。
                - URG= 1时,表明紧急指针字段有效。
                - 告诉系统报文段中有紧急数据, 应尽快传送(相当于高优先级的数据)。
                - URG需要和紧急指针配套使用,即数据从第 一个字节到紧急指针所指字节就是紧急数据。
            - 7)  确认位ACK。
                - 只有当ACK=1时确认号字段才有效。当ACK = 0时,确认号无效。
                - TCP规定,在连接建立后所有传送的报文段都必须把ACK置1。
            - 8)  推送位PSH ( Push)
                - 接收TCP收到PSH=1的报文段,就尽快地交付给接收应用进程, 而不再等到整个缓存都填满后再向上交付。
            - 9)  复位位RST ( Reset) 
                - RST=1时,表明TCP连接中出现严重差错(如主机崩溃或其他原 因),必须释放连接,然后再重新建立运输连接。
            - 10)  同步位SYN
                - 同步SYN=1表示这是一个连接请求或连接接收报文。
                - 当SYN=L ACK = 0时,表明这是一个连接请求报文?对方若同意建立连接,
                - 在响应 报文中使用SYN=1, ACK=1。
            - 11)  终止位FIN ( Finish)
                - 用来释放一个连接。FIN= 1表明此报文段的发送方的数据已发送 完毕,并要求释放传输连接。
            - 12)  窗口字段。
                - 占2B。
                - 指出现在允许对方发送的数据量
                - 例如,假设确认号是70L窗口字段是1000。这表明,从701一号算起,发送此报文段的 一方还有接收1000B数据(字节序号为701-1700) 的接收缓存空间。
            - 13)  校验和。
                - 占2B。
                - 校验和字段检验的范围包括首部和数据两部分。
                - 在计算校验和时,和 UDP 一样,要在TCP报文段的前面加上12B的伪首部
            - 14)  紧急指针字段。
                - 占16位
                - 指出在本报文段中紧急数据共有多少字节(紧急数据放在本 报文段数据的最前面)。
            - 15)  选项字段。
                - 长度可变。
                - TCP最初只规定了一种选项,即最大报文段长度( Maximum Segment Size, MSS) O MSS是TCP报文段中的数据字段的最大长度。
            - 16)  填充字段。
                - 这是为了使整个首部长度是4B的整数倍。
    - 5.3.3 TCP连接管理
        - 概述
            - 每个TCP连接都有三个阶段
                - 连接建立、数据传送和连接释 放。
            - TCP连接的管理就是使运输连接的建立和释放都能正常进行。
            - 解决以下三个问题
                - 1) 要使每一方都能够确知对方的存在。
                - 2)  要允许双方协商一些参数(如最大窗口值、是否使用窗口扩大选项、时间戳选项及服务 质量等)。
                - 3)  能够对运输实体资源(如缓存大小、连接表中的项目等)进行分配。
            - 每条TCP连接唯一地被通信两端的两个端点(即两个套接字)确定。
        - 1. TCP连接的建立
            - ![](https://firebasestorage.googleapis.com/v0/b/firescript-577a2.appspot.com/o/imgs%2Fapp%2FAFuture%2FVABzx0ibT9.png?alt=media&token=02be4f93-c618-47b4-8db7-16b19a66f94e)
            - 过程
                - A→ B：SYN = 1 ，ACK = 0，seq = x (x随机)
                - B→ A：SYN = 1 ，ACK = 1，seq = y (y随机)，ack = x+1
                - A→ B：SYN = 0 ，ACK = 1，seq = x+1，ack = y+1 （不携带数据则不消耗序号）
        - 2. TCP连接的释放
            - ![](https://firebasestorage.googleapis.com/v0/b/firescript-577a2.appspot.com/o/imgs%2Fapp%2FAFuture%2FvIlF2C8gGT.png?alt=media&token=32a9ef3d-6650-4418-b63f-1a253889a429)
            - 过程
                - A → B：FIN = 1 ，seq = u (最后一个序号)。释放申请
                - B → A：FIN = 0 ，ACK = 1 ，seq = v ，ack = u + 1  (确认收到释放申请)。半闭合状态。
                - B → A：FIN = 1 ，ACK = 1 ，seq = w ，ack = u + 1 。通知释放。
                - A → B：FIN = 0 ，ACK = 1 ，seq = u + 1 ，ack = w + 1 。释放
        - SYN 泛洪攻击
            - 过程
                - SYN洪泛攻击发生在OSI第四层,这种方式利用TCP协议的特性,就是三次握手攻击者发送 TCP
                - 攻击者发送 TCP三次握手中的第一个数据包,而当服务器返回  TCP三次握手中的第二个数据包后, 该攻击者就不对其进行再确认。
                - 那这个TCP连接就处于挂起状态,也就是所谓的半连接状态,服务器收不到再确认的话,还会重复发送ACK给攻击者。
            - 影响
                - 浪费服务器的资源
                - 在服务器上,这些TCP连接会因为挂起状态而消耗CPU和内存,最后服务器可能死机
    - 5.3.4 TCP可靠传输
        - 1. 序号
            - 序号字段用来保证数据能有序提交给应用层,TCP把数据视为一个无结构但有序 的字节流,序号建立在传送的字节流之上,而不建立在报文段之上。
            - TCP连接传送的数据流中的每个字节都编上一个序号。序号字段的值是指本报文段所发送的数据的第一个字节的序号。
        - 2. 确认
            - 确认号是期望收到对方的下一个报文段的数据的第一个字节的序号。
            - 发送方缓存区会继续存储那些已发送但未收 到确认的报文段,以便在需要时重传。
            - TCP默认使用累计确认,即 TCP 只确认数据流中至第一个丢失字节为止的字节。返回的 ack 为未确认的第一个字节的序号。
        - 3. 重传
            - 两种事件
                - (1) 超时
                    - TCP每发送一个报文段,就对这个报文段设置一次计时器。
                    - 计时器设置的重传时间到期但还 未收到确认时,就要重传这一报文段。
                    - 超时计时器的重传时间
                        - TCP采用一种自适应算法
                        - 报文段的往返时间(Round-Trip Time, RTT) = 一个报文段发出的时间,以及收到相应确认的时间,这两个时间之差
                        - new_RTTS = （1-\alpha) * old_RTTS + \alpha * new_RTT , 推荐 \alpha = 0.125
                        - new_RTTD = (1-\beta) * old_RTTD + \beta * |RTTS - new_RTT|, 推荐 \beta = 0.25
                        - 超时重传时间(Retransmission Time-Out,RTO) = RTTS + 4 * RTTD
                - (2) 冗余ACK (冗余确认)
                    - 发送方通常可在超时事件发生 之前通过注意所谓的冗余ACK来较好地检测丢包情况。
                    - TCP规定每当比期望序号大的失序报文段到达时,就发送一个冗余ACK, 指明下一个期待字节的序号[RFC 1122, RFC 2581]。
                    - TCP规定当发送方收到对同一个报文段的3个冗余ACK时,就可以认为跟在这个被 确认报文段之后的报文段已经丢失。
                    - 这种技术通常称为快速重传。
                    - Seq 的表现情况
                      
                        - ```python
                          # 当序号为 x 的报文丢失时，发送对x - 1 号报文的冗余ACK。
                          A 的发送情况：
                           - A → B: seq = x - 1  (收到)
                           - A → B: seq = x (未收到)
                          B 的表现：
                           - B → A: seq = x 
                           - B → A: seq = x
                           - B → A: seq = x
                          ```
    - 5.3.5 TCP流量控制
      
        - 概述
            - TCP提供流量控制服务来消除发送方使接收方缓存区溢出的可能性,因此可以说流量控制是 一个速度匹配服务
            - TCP提供一种基于滑动窗口协议的流量控制机制,
        - 原理
            - 接收方：根据自己接收缓存的大小,动态地调整发送方的发送窗口大小,这称为接收窗口 rwnd,即调整TCP报文段首部中的“窗口〃字段值,来限制发送方向网络注入报文的速率。
                - A发送数据给B，则 B发给A的TCP报文中的窗口字段表示：还能发送从 ack 起，共“窗口”大小的字节数。
            - 发送方根据其对当前网络拥塞程序的估计而确定的窗口值,这称为拥塞窗口 cwnd ,其大小与网络的带宽和时延密切相关。
                - A 的发送窗口大小，由 rwnd 与拥塞窗口 cwnd 共同决定（取较小的）。
    - 5.3.6 TCP拥塞控制
        - 概述
            - 指防止过多的数据注入网络,保证网络中的路由器或链路不致过载。
            - 出现拥塞时,端点并不了解到拥塞发生的细节,对通信连接的端点来说,拥塞往往表现为通信时延的 增加。
            - 拥塞控制与流量控制的区别:
                - 拥塞控制是让网络能够承受现有的网络负荷,是一个全局性的 过程,涉及所有的主机、所有的路由器,以及与降低网络传输性能有关的所有因素。
                - 流量 控制往往是指点对点的通信量的控制,即接收端控制发送端,它所要做的是抑制发送端发送数据 的速率,以便使接收端来得及接收。
            - 因特网建议标准定义了以下4种算法
                - 慢开始、拥塞避 免、快重传、快恢复。
            - TCP协议要求发送方维护以下两个窗口
                - 1)  接收窗口 rwnd
                    - 接收方根据目前接收缓存大小所许诺的最新窗口值,反映接收方的容量。 
                    - 由接收方根据其放在TCP报文的首部的窗口字段通知发送方。
                - 2)  拥塞窗口 cwnd
                    - 发送方根据自己估算的网络拥塞程度而设置的窗口值,反映网络的当前 容量。只要网络未出现拥塞,拥塞窗口就再增大一些,以便把更多的分组发送出去。但 只要网络出现拥塞,拥塞窗口就减小一些,以减少注入网络的分组数。
                - 发送窗口的上限值应取接收窗口 rwnd和拥塞窗口 cwnd中较小的一个
                    - 发送窗口的上限值=min[rwnd, cwnd]
        - 1. 慢开始和拥塞避免
            - ![](https://firebasestorage.googleapis.com/v0/b/firescript-577a2.appspot.com/o/imgs%2Fapp%2FAFuture%2FnrC7KSAtVL.png?alt=media&token=3ff0a5cb-5eb2-42ad-8054-17d81379e102)
            - (1) 慢开始算法 cwnd < ssthresh
                - 按 ack 线性增长；按照 RTT 指数增长
                - 过程
                    - 令 cwnd = 1，即一个最大报文段长度（MSS）。
                    - 每收到一个新的报文段的确认后，cwnd + 1
                    - 以指数增长，到门限 ssthresh （阈值）为止
            - (2)  拥塞避免算法 condo > ssthresh
                - 过程
                    - 线性增加，每个 RTT 后 cwnd + 1
                    - 出现一次超时后，ssthresh = cwnd 的一半
                - 根据cwnd的大小执行不同的算法
                    - 当cwnd < ssthresh时,使用慢开始算法。
                    - 当cwnd > ssthresh时,停止使用慢开始算法而改用拥塞避免算法。
                    - 当cwnd = ssthresh时,既可使用慢开始算法,又可使用拥塞避免算法(通常做法)。
            - (3)  网络拥塞的处理
                - 网络出现拥塞时,无论是在慢开始阶段还是在拥塞避免阶段,只要发送方检测到超时事件的 发生(未按时收到确认,重传计时器超时),就要把慢开始门限ssthresh设置为出现拥塞时的发送 方的cwnd值的一半(但不能小于2) 。
        - 2. 快重传和快恢复
            - (1)  快重传
                - 冗余ACK也用于网络拥塞的检测(丢了包当然意味着网络可能出现了拥塞)。
                - 快重传并非取 消重传计时器,而是在某些情况下可更早地重传丢失的报文段。
            - (2)  快恢复
                - ![](https://firebasestorage.googleapis.com/v0/b/firescript-577a2.appspot.com/o/imgs%2Fapp%2FAFuture%2FVQqTavE-rh.png?alt=media&token=f842cbcc-d258-47e1-8c2f-a86889d13eaa)
                - 过程
                    - 收到 3 个冗余 ACK 后，直接重传对应报文。
                    - 令 sshresh = 一半的cwnd，且cwnd 也为原来的一半。（不必从 1 指数增长到 sshresh）