## packetTracers使用



### 删除所有配置

```shell
Router>enable
Router# erase startup-config 
Erasing the nvram filesystem will remove all configuration files! Continue? [confirm]
[OK]
Erase of nvram: complete
%SYS-7-NV_BLOCK_INIT: Initialized the geometry of nvram
```



### 停止所有debug

```shell
RouterA# undebug all
All possible debugging has been turned off
```





```shell
# 0. 普通用户模式
Router>?
Exec commands:
  <1-99>      Session number to resume
  connect     Open a terminal connection
  disable     Turn off privileged commands
  disconnect  Disconnect an existing network connection
  enable      Turn on privileged commands
  exit        Exit from the EXEC
  logout      Exit from the EXEC
  ping        Send echo messages
  resume      Resume an active network connection
  show        Show running system information
  ssh         Open a secure shell client connection
  telnet      Open a telnet connection
  terminal    Set terminal line parameters
  traceroute  Trace route to destination
# 0.1 进入特权模式
在普通用户模式下输入enable命令即可进入
Router> enable
Router#
# 0.2  查看接口简短信息
Router>show ip interface brief 
Interface              IP-Address      OK? Method Status                Protocol 
FastEthernet0/0        192.168.1.1     YES manual up                    up 
Serial0/0              192.168.2.1     YES manual up                    up 
Serial0/1              unassigned      YES unset  administratively down down 
Serial0/2              unassigned      YES unset  administratively down down 
Serial0/3              unassigned      YES unset  administratively down down 
FastEthernet1/0        unassigned      YES unset  administratively down down
# 1.特权用户模式
模式指示符为“#”,在该模式下我们可以查看路由器的配置信息和调试信息等等。
Router# ?
Exec commands:
  <1-99>      Session number to resume
  auto        Exec level Automation
  clear       Reset functions
  clock       Manage the system clock
  configure   Enter configuration mode
  connect     Open a terminal connection
  copy        Copy from one file to another
  debug       Debugging functions (see also 'undebug')
  delete      Delete a file
  dir         List files on a filesystem
  disable     Turn off privileged commands
  disconnect  Disconnect an existing network connection
  enable      Turn on privileged commands
  erase       Erase a filesystem
  exit        Exit from the EXEC
  logout      Exit from the EXEC
  mkdir       Create new directory
  more        Display the contents of a file
  no          Disable debugging informations
  ping        Send echo messages
  reload      Halt and perform a cold restart
  resume      Resume an active network connection
  rmdir       Remove existing directory
  send        Send a message to other tty lines
  setup       Run the SETUP command facility
  show        Show running system information
  ssh         Open a secure shell client connection
  telnet      Open a telnet connection
  terminal    Set terminal line parameters
  traceroute  Trace route to destination
  undebug     Disable debugging functions (see also 'debug')
  write       Write running configuration to memory, network, or terminal
#1.1 将路由器能够显示历史命令的空间扩大到100
Router# terminal history size 100
#1.2 在特权用户模式下输入configure terminal命令即可进入全局配置模式
Router#configure terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#

#2.全局配置模式：，在该模式下主要完成全局参数的配置。 
Router(config)#?
Configure commands:
  aaa              Authentication, Authorization and Accounting.
  access-list      Add an access list entry
  banner           Define a login banner
  bba-group        Configure BBA Group
  boot             Modify system boot parameters
  cdp              Global CDP configuration subcommands
  class-map        Configure Class Map
  clock            Configure time-of-day clock
  config-register  Define the configuration register
  crypto           Encryption module
  default          Set a command to its defaults
  do               To run exec commands in config mode
  enable           Modify enable password parameters
  end              Exit from configure mode
  exit             Exit from configure mode
  hostname         Set system's network name
  interface        Select an interface to configure
  ip               Global IP configuration subcommands
  key              Key management
  line             Configure a terminal line
  lldp             Global LLDP configuration subcommands
  logging          Modify message logging facilities
  no               Negate a command or set its defaults
  ntp              Configure NTP
  policy-map       Configure QoS Policy Map
  priority-list    Build a priority list
  privilege        Command privilege parameters
  queue-list       Build a custom queue list
  radius-server    Modify Radius query parameters
  router           Enable a routing process
  service          Modify use of network based services
  snmp-server      Modify SNMP engine parameters
  tacacs-server    Modify TACACS query parameters
  username         Establish User Name Authentication

# 2.1 修改路由器的名字(Hostname)
Router(config)# hostname ?
  WORD  This system's network name
Router(config)#hostname abc
abc(config)#
# 2.2 配置某个接口
Router(config)#interface ?
  Dialer            Dialer interface
  Ethernet          IEEE 802.3
  FastEthernet      FastEthernet IEEE 802.3
  GigabitEthernet   GigabitEthernet IEEE 802.3z
  Loopback          Loopback interface
  Serial            Serial
  Virtual-Template  Virtual Template interface
  range             interface range command
例子 
interface s1/0
Router(config-if)# 
完整命令是  interface Serial1/0
# 2.3 进入线控模式 
Router(config)#line vty ?
  <0-15>  First Line number
例子  
Router(config)#line vty 0
Router(config-line)#


# 3. 接口配置  (Interface configuration) interface 接口名称
Router(config-if)#?
  bandwidth          Set bandwidth informational parameter
  cdp                CDP interface subcommands
  clock              Configure serial interface clock
  crypto             Encryption/Decryption commands
  custom-queue-list  Assign a custom queue list to an interface
  delay              Specify interface throughput delay
  description        Interface specific description
  encapsulation      Set encapsulation type for an interface
  exit               Exit from interface configuration mode
  fair-queue         Enable Fair Queuing on an Interface
  frame-relay        Set frame relay parameters
  hold-queue         Set hold queue depth
  ip                 Interface Internet Protocol config commands
  keepalive          Enable keepalive
  mtu                Set the interface Maximum Transmission Unit (MTU)
  no                 Negate a command or set its defaults
  ppp                Point-to-Point Protocol
  priority-group     Assign a priority group to an interface
  service-policy     Configure QoS Service Policy
  shutdown           Shutdown the selected interface
  tx-ring-limit      Configure PA level transmit ring limit
  zone-member        Apply zone name
# 3.1配置ip地址(串行口 以太网口均可)
Router (config-if)# ip address 192.168.1.1 255.255.255.0 //为该接口配置IP地址
Router (config-if)# no shutdown  //（缺省时，接口都是关闭的。输入此命令开启接口）
# 3.2配置Serial(串行)口
Router(config-if)# bandwidth 56    //（串行线两端都需要设定带宽）
Router(config-if)# clock rate 56000 //（串行线中DCE端需设定时钟，DTE端则不需要）
Router(config-if)# ip address 192.168.2.1 255.255.255.0 
Router(config-if) #no shutdown    //（缺省时，接口都是关闭的。输入此命令开启接口）
# 3.3 配置路由协议
Router(config)# router rip  //启用RIP路由协议
Router(config-router)#network 192.168.1.0  //加入路由通报网络
Router(config-router)#network 192.168.2.0


# 4. 线控模式(Line configuration)
Router(config)#line ?
  <2-499>  First Line number
  aux      Auxiliary line
  console  Primary terminal line
  tty      Terminal controller
  vty      Virtual terminal
  x/y/z    Slot/Subslot/Port for Modems


```









| 模式                                    | 访问方式                                                     | 提示符               | 退出方法                                                     | 描述                                                         |
| --------------------------------------- | ------------------------------------------------------------ | -------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 0 用户模式  (User EXEC）                | 在路由器上启动一个会话                                       | Router>              | 输入Logout 或quit                                            | 使用该模式完成基本的测试和系统显示功能                       |
| 1 特权模式  (Privileged EXEC)           | 在用户EXEC模式下，输入enable命令                             | Router#              | 输入disable或exit                                            | 使用该模式来检验所输入的命令。一些配置命令也可以使用。可以用口令来保护对此模式的访问 |
| 2 全局配置  (Global configuration）     | 在特权EXEC模式下，输入configure命令                          | Router(config)#      | 要退回到特权EXEC模式输入exit、end或按下ctr-z                 | 使用该模式配置用于整个路由器的参数                           |
| 3 接口配置   (Interface configuration） | 在全局配置模式下,输入interface 命令以及特定端口号            | Router(config-if)#   | 要退回到全局配置模式，输入exit。要退回特权EXEC模式，按下ctr-z或输入end | 采用此模式为以太网接口配置参数                               |
| 4 连接配置  （Line configuration）      | 在全局配置模式下，使用line vty 或line console命令，并指定连接编号 | Router(config-line)# | 要退回到全局配置模式，输入exit。要退回特权EXEC模式，按下ctr-z或输入end | 使用该模式配置针对终端连接或console连接的参数                |





| 命令                                           | 任务                           |
| ---------------------------------------------- | ------------------------------ |
| ? （特权或者用户模式）                         | 显示常用的命令的列表           |
| Router# **show** version                       | 查看版本及引导信息             |
| Router# **show**　flash                        | 查看ＩＯＳ文件信息             |
| Router# **show** running-config                | 查看运行配置信息               |
| Router# **show** startup-config                | 查看开机配置信息               |
| Router# **show** history                       | 查看曾经键入过的命令的历史记录 |
| Router# **show** interface type slot#/port#    | 显示端口信息                   |
| Router# **copy** running-config startup-config | 将RAM中的当前配置保存到NVRAM中 |
| Router# **copy** startup-config running-config | 加载来自NVRAM的配置信息        |
| Router# **show** ip route                      | 显示路由信息                   |



| 命令                                                        | 任务               |
| ----------------------------------------------------------- | ------------------ |
| Router(config)# **username** username **password** password | 设置访问用户及密码 |
| Router# **enable secret** password                          | 设置特权密码       |
| Router(config)#**hostname** name                            | 设置路由器名       |





### 实验2



| 路由器的信息(子网掩码均为  255.255.255.0) |              |                                           |                                         |          |
| :---------------------------------------: | :----------: | :---------------------------------------: | --------------------------------------- | -------- |
|                 路由器名                  |     类型     |                  IP 地址                  | RIP路由网络                             | 时钟频率 |
|                  RouterA                  |    2620XM    | Fa0/0:  192.168.43.1  S0/0:  192.168.99.1 | 192.168.66.0 255.255.255.0 192.168.99.0 | 56000    |
|                  RouterB                  |    2620XM    | Fa0/0:  192.168.66.1  S0/0:  192.168.99.2 | 192.168.43.0 255.255.255.0 192.168.99.0 |          |
|    PC信息 (子网掩码均为255.255.255.0)     |              |                                           |                                         |          |
|                  主机名                   |    IP地址    |                 缺省网关                  | 所属网段                                |          |
|                    PC0                    | 192.168.43.2 |               192.168.43.1                | 192.168.43.0                            |          |
|                    PC1                    | 192.168.43.3 |               192.168.43.1                | 192.168.43.0                            |          |
|                    PC2                    | 192.168.66.2 |                192.168.3.1                | 192.168.66.0                            |          |
|                    PC3                    | 192.168.66.3 |                192.168.3.1                | 192.168.66.0                            |          |
|                  Hub信息                  |              |                                           |                                         |          |
|                   名称                    |     类型     |                 所属网段                  |                                         |          |
|                   Hub 0                   |    Hub-PT    |               192.168.43.0                |                                         |          |
|                   Hub 1                   |    Hub-PT    |               192.168.66.0                |                                         |          |



#### RouterA的反转线配置 打开远程访问接口

```shell
#   RouterA 的配置 真实情况应该是用反转线接上console口开启远程配置
RouterA>enable
RouterA#configure 
Configuring from terminal, memory, or network [terminal]? 
Enter configuration commands, one per line.  End with CNTL/Z.
RouterA(config)#line vty 0 4
# 设置密码 为 abcd
RouterA(config-line)#password abcd     
RouterA(config-line)#login
RouterA(config-line)#transport input telnet # 真实路由器必须配 虚拟环境随意


# pc0 使用command Prompt 通过FastEthernet0/0 使用telnet登录路由器 RouterA
C:\>telnet 192.168.43.1
Trying 192.168.43.1 ...Open


User Access Verification  # 输入密码 无回显

Password: 
# 成功实现了远程登陆
RouterA>  
```



#### 路由器B配置 (A也是类似)

```shell
# 路由器B配置
Router>enable 
Router#configure
Configuring from terminal, memory, or network [terminal]? 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#hostname RouterB

# 配置RouterB的FastEthernet0/0 的ip地址
RouterB(config)#interface f0/0
RouterB(config-if)#ip addr 192.168.66.1 255.255.255.0
RouterB(config-if)#no shutdown
%LINK-5-CHANGED: Interface FastEthernet0/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/0, changed state to up
RouterB(config-if)#exit

# 配置RouterB的Serial1/0 的ip地址
RouterB(config)#interface serial 1/0
RouterB(config-if)#ip addr 192.168.99.2 255.255.255.0
RouterB(config-if)#no shutdown
%LINK-5-CHANGED: Interface Serial1/0, changed state to down

%LINEPROTO-5-UPDOWN: Line protocol on Interface Serial1/0, changed state to up
RouterB(config-if) exit
# 配置静态路由  ip route    目标网段  子网掩码  下一跳
RouterB(config)#ip route  192.168.43.0 255.255.255.0 192.168.99.0

# 配置rip动态路由 与路由器连接的网段都要加入 
RouterB(config)#router rip
RouterB(config-router)#network 192.168.66.0
RouterB(config-router)#network 192.168.99.0  
# 同理 RouterA需要rip这么配置
RouterA(config)#router rip
RouterA(config-router)#network 192.168.43.0
RouterA(config-router)#network 192.168.99.0  


RouterB#show ip route 
Codes: C - connected, S - static, I - IGRP, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route
Gateway of last resort is not set
R    192.168.43.0/24 [120/1] via 192.168.99.1, 00:00:12, Serial1/0  #这个就是rip生成的路由表  
C    192.168.66.0/24 is directly connected, FastEthernet0/0
C    192.168.99.0/24 is directly connected, Serial1/0
```



### 实验3

| PC信息 (子网掩码均为255.255.255.0) |              |              |              |
| ---------------------------------- | ------------ | ------------ | ------------ |
| 主机名                             | IP地址       | 缺省网关     | 所属网段     |
| PC0                                | 192.168.43.2 | 192.168.43.1 | 192.168.43.0 |
| PC1                                | 192.168.43.3 | 192.168.43.1 | 192.168.43.0 |
| PC2                                | 192.168.43.4 | 192.168.43.1 | 192.168.43.0 |
| PC3                                | 192.168.43.5 | 192.168.43.1 | 192.168.43.0 |

```
ping 192.168.43.3
ping 192.168.43.4
ping 192.168.43.5
```







### 实验4

交换机配置方式及基本命令的熟悉

| 模式                                | 访问方式                                                     | 提示符               | 退出方法                                                     | 描述                                                         |
| ----------------------------------- | ------------------------------------------------------------ | -------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 用户EXEC  （User EXEC）             | 在交换机上启动一个会话                                       | Switch>              | 输入Logout 或quit                                            | 使用该模式完成基本的测试和系统显示功能                       |
| 特权EXEC  (Privileged EXEC)         | 在用户EXEC模式下，输入enable命令                             | Switch#              | 输入disable或exit                                            | 使用该模式来检验所输入的命令。一些配置命令也可以使用。可以用口令来保护对此模式的访问 |
| VLAN配置（VLAN  configuration）     | 在特权EXEC模式下，输入vlan database命令                      | Switch(vlan)#        | 要退回到特权EXEC模式输入exit                                 | 使用该模式完成vlan各项参数的配置                             |
| 全局配置（Global configuration）    | 在特权EXEC模式下，输入configure命令                          | Switch(config)#      | 要退回到特权EXEC模式输入exit、end或按下ctr-z                 | 使用该模式配置用于整个交换机的参数                           |
| 接口配置（Interface configuration） | 在全局配置模式下，输入interface 命令以及特定端口号           | Switch(config-if)#   | 要退回到全局配置模式，输入exit。要退回特权EXEC模式，按下ctr-z或输入end | 采用此模式为以太网接口配置参数                               |
| 连接配置（Line configuration）      | 在全局配置模式下，使用line vty 或line console命令，并指定连接编号 | Switch(config-line)# | 要退回到全局配置模式，输入exit。要退回特权EXEC模式，按下ctr-z或输入end | 使用该模式配置针对终端连接或console连接的参数                |





| 命令                                        | 任务                           |
| ------------------------------------------- | ------------------------------ |
| Switch >enble                               | 由用户模式进入特权模式         |
| ? （特权或者用户模式）                      | 显示常用的命令的列表           |
| Switch # show version                       | 查看版本及引导信息             |
| Switch # show running-config                | 查看运行设置                   |
| Switch # show startup-config                | 查看开机设置                   |
| Switch # show history                       | 查看曾经键入过的命令的历史记录 |
| Switch # show interface type slot/number    | 显示端口信息                   |
| Switch # copy running-config startup-config | 将RAM中的当前配置保存到NVRAM中 |
| Switch # copy startup-config running-config | 加载来自NVRAM的配置信息        |
| Switch # show vlan                          | 显示虚拟局域网信息             |
| Switch(config)#hostname                     | 修改交换机的名称               |
| Switch(config)#interface interface-number   | 对端口进行配置                 |
| Switch(config-if)#duplex full               | 将端口设置为全双工模式         |
| Switch(config-if) #speed                    | 设置端口的速度                 |



```shell
Switch>show version
Cisco Internetwork Operating System Software
IOS (tm) C2950 Software (C2950-I6Q4L2-M), Version 12.1(22)EA4, RELEASE SOFTWARE(fc1)
Copyright (c) 1986-2005 by cisco Systems, Inc.
Compiled Wed 18-May-05 22:31 by jharirba
Image text-base: 0x80010000, data-base: 0x80562000

ROM: Bootstrap program is is C2950 boot loader
Switch uptime is 52 seconds
System returned to ROM by power-on

Cisco WS-C2950-24 (RC32300) processor (revision C0) with 21039K bytes of memory.
Processor board ID FHK0610Z0WC
Last reset from system-reset
Running Standard Image
24 FastEthernet/IEEE 802.3 interface(s)

63488K bytes of flash-simulated non-volatile configuration memory.
Base ethernet MAC Address: 00D0.9799.79A7
Motherboard assembly number: 73-5781-09 
Power supply part number: 34-0965-01
Motherboard serial number: FOC061004SZ
Power supply serial number: DAB0609127D
Model revision number: C0
Motherboard revision number: A0
Model number: WS-C2950-24
System serial number: FHK0610Z0WC
Configuration register is 0xF

Switch> enable

Switch# show interfaces fastEthernet 0/1
FastEthernet0/1 is up, line protocol is up (connected)
  Hardware is Lance, address is 0001.4237.ae01 (bia 0001.4237.ae01)
 BW 100000 Kbit, DLY 1000 usec,
     reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive set (10 sec)
  Full-duplex, 100Mb/s #该接口是全双工的以100Mb/s来收发数据
  input flow-control is off, output flow-control is off
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:08, output 00:00:05, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/75/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue :0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec 
  #显示在前5分钟通过接口发送和接收的平均位数和平均分组数。
     956 packets input, 193351 bytes, 0 no buffer
     Received 956 broadcasts, 0 runts, 0 giants, 0 throttles
     0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored, 0 abort
     0 watchdog, 0 multicast, 0 pause input
     0 input packets with dribble condition detected
     2357 packets output, 263570 bytes, 0 underruns
     0 output errors, 0 collisions, 10 interface resets
     0 babbles, 0 late collision, 0 deferred
     0 lost carrier, 0 no carrier
     0 output buffer failures, 0 output buffers swapped out
# 表示路由器接收的无错误分组的总数量。其次，它还表示路由器接收的无错误分组的总字节数。有无缓冲,接口所接收的广播或多播分组的总数量。
Switch#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#
Switch(config)#hostname YHN
YHN(config)#enable secret yhn

# 再次进入特权状态时要求输入口令。
YHN> enable
Password: # 在此密码不会以明文的形式出现
YHN# configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
YHN(config)#interface fastEthernet 0/1
YHN(config-if)#duplex ? # 设置工作模式
  auto  Enable AUTO duplex configuration # 自动
  full  Force full duplex operation  # 全双工
  half  Force half-duplex operation # 半双工
YHN(config-if)#speed ?  #设置速度
  10    Force 10 Mbps operation
  100   Force 100 Mbps operation
  auto  Enable AUTO speed configuration
YHN(config-if)#no shutdown
# 让配置关机也生效  交换机没有电源  设计上就是不让他断电的
YHN# copy running-config startup-config
Destination filename [startup-config]? 
Building configuration...
[OK]
```





### 实验5

通过该实验理解VLAN的基本概念，掌握在二层交换机上创建VLAN的方法

![image-20201225083407758](C:\Users\yhn\AppData\Roaming\Typora\typora-user-images\image-20201225083407758.png)

#### switchA设置

```shell
Switch>enable
Switch#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#hostname SwitchA
SwitchA(config)#vlan 90
SwitchA(config-vlan)#name v90
SwitchA(config)#vlan 80
SwitchA(config-vlan)#name v80
SwitchA(config)#vlan 70
SwitchA(config-vlan)#name v70
SwitchA(config-vlan)#exit

# 退回到特权模式下 查看所有创建的vlan
SwitchA#show vlan
VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/1, Fa0/2, Fa0/3, Fa0/4
                                                Fa0/5, Fa0/6, Fa0/7, Fa0/8
                                                Fa0/9, Fa0/10, Fa0/11, Fa0/12
                                                Fa0/13, Fa0/14, Fa0/15, Fa0/16
                                                Fa0/17, Fa0/18, Fa0/19, Fa0/20
                                                Fa0/21, Fa0/22, Fa0/23, Fa0/24
70   v70                              active    
80   v80                              active    
90   v90                              active    
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active    

VLAN Type  SAID       MTU   Parent RingNo BridgeNo Stp  BrdgMode Trans1 Trans2
---- ----- ---------- ----- ------ ------ -------- ---- -------- ------ ------
1    enet  100001     1500  -      -      -        -    -        0      0
70   enet  100070     1500  -      -      -        -    -        0      0
80   enet  100080     1500  -      -      -        -    -        0      0
90   enet  100090     1500  -      -      -        -    -        0      0
1002 fddi  101002     1500  -      -      -        -    -        0      0   
1003 tr    101003     1500  -      -      -        -    -        0      0   
1004 fdnet 101004     1500  -      -      -        ieee -        0      0   
1005 trnet 101005     1500  -      -      -        ibm  -        0      0   

VLAN Type  SAID       MTU   Parent RingNo BridgeNo Stp  BrdgMode Trans1 Trans2
---- ----- ---------- ----- ------ ------ -------- ---- -------- ------ ------

Remote SPAN VLANs
------------------------------------------------------------------------------

Primary Secondary Type              Ports
------- --------- ----------------- ------------------------------------------

# 把端口划分到VLAN中去
# 根据连接图可知 端口Fa0/1划到v90, 端口Fa0/2划到v80, 端口Fa0/3划到v70, ) 

# 端口Fa0/1 划到 v90
SwitchA>enable
SwitchA#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
SwitchA(config)#interface fastEthernet 0/1
SwitchA(config-if)#switchport access vlan 90

# 端口Fa0/2划到v80
SwitchA(config-if)#exit
SwitchA(config)#interface fastEthernet 0/2
SwitchA(config-if)#switchport access vlan 80

# 端口Fa0/3划到v70
SwitchA(config-if)#exit
SwitchA(config)#interface fastEthernet 0/3
SwitchA(config-if)#switchport access vlan 70

SwitchA#show vlan

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/4, Fa0/5, Fa0/6, Fa0/7
                                                Fa0/8, Fa0/9, Fa0/10, Fa0/11
                                                Fa0/12, Fa0/13, Fa0/14, Fa0/15
                                                Fa0/16, Fa0/17, Fa0/18, Fa0/19
                                                Fa0/20, Fa0/21, Fa0/22, Fa0/23
                                                Fa0/24
70   v70                              active    Fa0/3   #注意 这里已经改变
80   v80                              active    Fa0/2
90   v90                              active    Fa0/1
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active    

VLAN Type  SAID       MTU   Parent RingNo BridgeNo Stp  BrdgMode Trans1 Trans2
---- ----- ---------- ----- ------ ------ -------- ---- -------- ------ ------
1    enet  100001     1500  -      -      -        -    -        0      0
70   enet  100070     1500  -      -      -        -    -        0      0
80   enet  100080     1500  -      -      -        -    -        0      0
90   enet  100090     1500  -      -      -        -    -        0      0
1002 fddi  101002     1500  -      -      -        -    -        0      0   
1003 tr    101003     1500  -      -      -        -    -        0      0   
1004 fdnet 101004     1500  -      -      -        ieee -        0      0   
1005 trnet 101005     1500  -      -      -        ibm  -        0      0   

VLAN Type  SAID       MTU   Parent RingNo BridgeNo Stp  BrdgMode Trans1 Trans2
---- ----- ---------- ----- ------ ------ -------- ---- -------- ------ ------

Remote SPAN VLANs
------------------------------------------------------------------------------

Primary Secondary Type              Ports
------- --------- ----------------- ------------------------------------------

#设置交换机Switch A上与Switch B相连的端口（Fa0/24）.
#Switch A上与Switch B相连的端口Fa0/24的模式设置为Trunk模式。Trunk是端口汇聚的意思，Trunk（干道）是一种封装技术，它是一条点到点的链路，主要功能就是仅通过一条链路就可以连接多个交换机从而扩展已配置的多个VLAN
SwitchA(config)#interface fastEthernet 0/4
SwitchA(config-if)#switchport mode trunk 
%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/4, changed state to down
%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/4, changed state to up
```

#### switchB设置

```shell
Switch>enable
Switch#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#hostname SwitchB
SwitchB(config)#vlan 90
SwitchB(config-vlan)#name v90
SwitchB(config)#vlan 80
SwitchB(config-vlan)#name v80
SwitchB(config)#vlan 70
SwitchB(config-vlan)#name v70

SwitchB(config)#interface fastEthernet 0/1
SwitchB(config-if)#switchport access vlan 90
SwitchB(config-if)#exit
SwitchB(config)#interface fastEthernet 0/2
SwitchB(config-if)#switchport access vlan 80
SwitchB(config-if)#exit
SwitchB(config)#interface fastEthernet 0/3
SwitchB(config-if)#switchport access vlan 70
SwitchB(config-if)#exit


SwitchB#show vlan
VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/4, Fa0/5, Fa0/6, Fa0/7
                                                Fa0/8, Fa0/9, Fa0/10, Fa0/11
                                                Fa0/12, Fa0/13, Fa0/14, Fa0/15
                                                Fa0/16, Fa0/17, Fa0/18, Fa0/19
                                                Fa0/20, Fa0/21, Fa0/22, Fa0/23
                                                Fa0/24
70   v70                              active    Fa0/3
80   v80                              active    Fa0/2
90   v90                              active    Fa0/1
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active    

VLAN Type  SAID       MTU   Parent RingNo BridgeNo Stp  BrdgMode Trans1 Trans2
---- ----- ---------- ----- ------ ------ -------- ---- -------- ------ ------
1    enet  100001     1500  -      -      -        -    -        0      0
70   enet  100070     1500  -      -      -        -    -        0      0
80   enet  100080     1500  -      -      -        -    -        0      0
90   enet  100090     1500  -      -      -        -    -        0      0
1002 fddi  101002     1500  -      -      -        -    -        0      0   
1003 tr    101003     1500  -      -      -        -    -        0      0   
1004 fdnet 101004     1500  -      -      -        ieee -        0      0   
1005 trnet 101005     1500  -      -      -        ibm  -        0      0   

VLAN Type  SAID       MTU   Parent RingNo BridgeNo Stp  BrdgMode Trans1 Trans2
---- ----- ---------- ----- ------ ------ -------- ---- -------- ------ ------

Remote SPAN VLANs
------------------------------------------------------------------------------

Primary Secondary Type              Ports
------- --------- ----------------- ------------------------------------------

SwitchB(config)#interface fastEthernet 0/4
SwitchB(config-if)#switchport mode trunk 
```



### 实验5进阶

二层交换机+路由器实现VLAN间通信

![image-20201225105020305](C:\Users\yhn\AppData\Roaming\Typora\typora-user-images\image-20201225105020305.png)

#### switch配置

```shell
Switch(config)#vlan 80
Switch(config-vlan)#name v80
Switch(config-vlan)#exit
Switch(config)#vlan 70
Switch(config-vlan)#name v70

Switch(config)#interface fastEthernet 0/1
Switch(config-if)#switchport access vlan 80
Switch(config-if)#exit
Switch(config)#interface fastEthernet 0/2
Switch(config-if)#switchport access vlan 70
Switch(config-if)#exit

# show vlan
70   v70                              active    Fa0/2
80   v80                              active    Fa0/1

#注意：Cisco 2950 只支持802.1Q协议，所以在这里不用专门来指定封装协议。若要指定协议使用命令：
# Switch(config-if)#switchport  trunk  encapsulation dot1q         
# (仿真环境不能使用，因此这里就不必配置)

Switch(config)#interface fastEthernet 0/3
Switch(config-if)#switchport mode trunk 
%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/3, changed state to down
%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/3, changed state to up
```



#### router配置

因为与Trunk相连的路由器端口同时能够与两个Vlan连通，所以设置两个子接口（Fa0/0.1和Fa0/0.2），分别分配到两个Vlan中，并分别为这两个子接口分配IP地址，使之可以同时属于两个网段。

子接口的产生打破了物理接口的局限性，它允许一个路由器的单个物理接口通过划分多个子接口的方式，实现多个VLAN间的路由和通信。每个子接口从功能、作用上来说，与每个物理接口是没有任何区别的。
子接口与主接口的关系
子接口共用主接口的物理层参数，又可以分别配置各自的链路层和网络层参数。用户可以禁用或者激活子接口，这不会对主接口产生影响；但主接口状态的变化会对子接口产生影响，特别是只有主接口处于连通状态时子接口才能正常工作。
子接口产生的原因
在VLAN虚拟局域网中，通常是一个物理接口对应一个 VLAN。在多个 VLAN 的网络上，无法使用单台路由器的一个物理接口实现 VLAN 间通信，同时路由器有其物理局限性，不可能带有大量的物理接口。



```shell
Router>enable
Router#configure 
Configuring from terminal, memory, or network [terminal]? 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#interface fastEthernet 0/0
Router(config-if)#no shutdown
%LINK-5-CHANGED: Interface FastEthernet0/0, changed state to up
%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/0, changed state to up


Router(config)#interface fastEthernet 0/0.1
%LINK-5-CHANGED: Interface FastEthernet0/0.1, changed state to up
%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/0.1, changed state to up
# 配置封装模式为IEEE802.1Q ,对应VLAN号为80
Router(config-subif)#encapsulation dot1Q 80
Router(config-subif)#ip address 192.168.80.1 255.255.255.0

Router(config)#interface FastEthernet0/0.2
%LINK-5-CHANGED: Interface FastEthernet0/0.2, changed state to up
%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/0.2, changed state to up
Router(config-subif)#encapsulation dot1Q 70
Router(config-subif)#ip address 192.168.70.1 255.255.255.0
```



pc1

```powershell
C:\>ping 192.168.70.2

Pinging 192.168.70.2 with 32 bytes of data:

Request timed out.  # 第一次ping 不通
Reply from 192.168.70.2: bytes=32 time<1ms TTL=127
Reply from 192.168.70.2: bytes=32 time<1ms TTL=127
Reply from 192.168.70.2: bytes=32 time<1ms TTL=127

Ping statistics for 192.168.70.2:
    Packets: Sent = 4, Received = 3, Lost = 1 (25% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms
```





**一层交换机** 只支持物理层协议
**二层交换机** 支持物理层和数据链路层协议,如以太网交换机

二层交换机指的就是传统的工作在OSI参考模型的第二层--数据链路层上交换机，主要功能包括物理编址、错误校验、帧序列以及流控。

**三层交换机** 支持物理层,数据链路层及网络层协议,如某些带路由功能的交换机 比如划分vlan



### 实验6 

通过设计有两个路由器的网络及静态路由的配置理解静态路由原理。

实验2已经做过 就不重复了

1、 静态路由信息在缺省情况下是私有的，不会传递给其他的路由器。

2、在默认情况下，静态路由的出口方式指定优先级会比下一跳地址高，但是我们这里建议网络管理者使用下一跳地址做为静态路由，因为如果出口是在关闭状态下，那么这条静态路由便不会被装载到路由表中。

3、静态路由的另一个作用是作动态路由的备份路由选项，如果我们已经配置了动态路由，可以手动的更改静态路由的优先级，当动态路由出现问题的时候路由器便可以选择这条静态路由来转发数据包。





### 实验7

![image-20201225143812959](C:\Users\yhn\AppData\Roaming\Typora\typora-user-images\image-20201225143812959.png)

多网段网络组建与动态路由配置

#### routerA

```shell
Router>
Router>enable
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#hostname RouterA
RouterA(config)#interface fastEthernet 0/0
RouterA(config-if)#no shutdown
%LINK-5-CHANGED: Interface FastEthernet0/0, changed state to up
%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/0, changed state to up
RouterA(config-if)#ip address 192.168.66.1 255.255.255.0
RouterA(config-if)#exit
RouterA(config)#router rip

RouterA(config-router)#network 192.168.66.0
RouterA(config-router)#network 192.168.88.0


RouterA(config)#interface serial 1/0
RouterA(config-if)#ip address 192.168.88.2 255.255.255.0
RouterA(config-if)#no shutdown 


RouterA>enable
RouterA#show ip route 
Codes: C - connected, S - static, I - IGRP, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route

Gateway of last resort is not set

C    192.168.66.0/24 is directly connected, FastEthernet0/0
C    192.168.88.0/24 is directly connected, Serial1/0
R    192.168.99.0/24 [120/1] via 192.168.88.3, 00:00:24, Serial1/0  # 动态添加的动态路由 rip
```





#### routerB

```shell
Router>enable 
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#hostname RouterB
RouterB(config)#interface serial 1/0
RouterB(config-if)#ip ad
RouterB(config-if)#ip address 192.168.88.3 255.255.255.0
RouterB(config-if)#no shutdown
%LINK-5-CHANGED: Interface Serial1/0, changed state to up

RouterB(config)#router rip 
RouterB(config-router)#network 192.168.88.0
RouterB(config-router)#network 192.168.99.0

RouterB(config)#interface fastEthernet 0/0
RouterB(config-if)#ip address 192.168.99.1 255.255.255.0
RouterB(config-if)#no shutdown
%LINK-5-CHANGED: Interface FastEthernet0/0, changed state to up
%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/0, changed state to up

RouterB#show ip route 
Codes: C - connected, S - static, I - IGRP, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route
Gateway of last resort is not set

R    192.168.66.0/24 [120/1] via 192.168.88.2, 00:00:11, Serial1/0
C    192.168.88.0/24 is directly connected, Serial1/0
C    192.168.99.0/24 is directly connected, FastEthernet0/0

```



debug

```shell
RouterA#debug ip rip 
RIP protocol debugging is on
RouterA#RIP: sending  v1 update to 255.255.255.255 via FastEthernet0/0 (192.168.66.1)
RIP: build update entries
      network 192.168.88.0 metric 1
      network 192.168.99.0 metric 2
RIP: sending  v1 update to 255.255.255.255 via Serial1/0 (192.168.88.2)
RIP: build update entries
      network 192.168.66.0 metric 1
RIP: received v1 update from 192.168.88.3 on Serial1/0
      192.168.99.0 in 1 hops  # 路由器从接口192.168.88.3接收到更新信息：到达192.168.99.0网络只需要一跳。
# 路由器 
RIP: sending  v1 update to 255.255.255.255 via FastEthernet0/0 (192.168.66.1) 
RIP: build update entries
      network 192.168.88.0 metric 1
      network 192.168.99.0 metric 2  # 路由的跳数+1 得到 本路由向192.168.99.0 网段的距离
RIP: sending  v1 update to 255.255.255.255 via Serial1/0 (192.168.88.2)
RIP: build update entries
      network 192.168.66.0 metric 1
```





pc0

```powershell
C:\>ping 192.168.99.2

Pinging 192.168.99.2 with 32 bytes of data:

Request timed out.
Reply from 192.168.99.2: bytes=32 time=12ms TTL=126
Reply from 192.168.99.2: bytes=32 time=5ms TTL=126
Reply from 192.168.99.2: bytes=32 time=1ms TTL=126

Ping statistics for 192.168.99.2:
    Packets: Sent = 4, Received = 3, Lost = 1 (25% loss),
Approximate round trip times in milli-seconds:
    Minimum = 1ms, Maximum = 12ms, Average = 6ms
```

### 实验8

#### 两个相关命令

```shell
1. Access-list命令
Access-list access-list-number {deny | permit} source [source-wildcard] [log] 
access-list命令参数的含义如下: 
(1) access-list-number:访问控制列表号,标准访问控制列表的号码范围是1~99。
(2) deny:如果满足条件,数据包被拒绝从该入口通过。
(3) permit:如果满足条件,数据包允许从该入口通过。
(4) source:数据包的源网络地址,源网络地址可以是具体的地址或any(任意),如果源地址是单个IP地址时,将"source"改成"host",后再写IP地址即可。
(5) Source-wildcard:源地址通配符掩码，可选项。通配符掩码是一个32比特位的数字字符串,使用1或0来表示,它被用"."分成4组,每组8位。在通配符掩码位中,0表示"检查相应的位",而1表示不检查相应位。通配符掩码相当于子网掩码的反码。
(6) Log:可选项,生成日志信息,记录匹配permit或deny语句的包。
可以通过在"access-list"命令前加"no"的形式,来删除一个已经建立的标准ACL。
关键字any和host的用法 
(1)any指定对允许所有的IP地址作为源地址。这样,当某环境下允许访问任何目的地址时,我们就不用输入source位为"0.0.0.0",再输入通配符掩码为"255.255.255.255"了,直接使用"any"就可以了。下面两行指令是等价的。
access-list 1 permit 0.0.0.0 255.255.255.255 
access-list 1 permit any 
(2)host用在访问表中指定通配符掩码是0.0.0.0这样某环境下要输入单个的地址,如172.16.8.1,就不用输入172.16.8.1和通配符掩码0.0.0.0了,直接在地址前加host就可以了。

2. ip access-group 命令
ACL的使用 
在创建了一个访问控制列表并分配了表号之后,为了让该访问控制列表起作用,用户必须把它配置到一个接口上且指明数据流方向。
其语法是：ip access-group access-list-number {in|out} 
(1) Ip：定义所用的协议。
(2) access-list-number：访问控制列表的号码。
(3) in |out：定义ACL是被应用到接口的流入方向(in),还是接口的流出方向(out)。
如果是外网ping入 需要用in!   内网出去用OUT
```

#### 图示与解释

![image-20201225160612226](C:\Users\yhn\AppData\Roaming\Typora\typora-user-images\image-20201225160612226.png)

**其实下边两个设置 一个是开放ethernet 1/1  out 也就是允许 所有的从DMZ区域向192.168.98.0这个网段的数据包而且还允许 192.168.99.3 (pc1)这个计算机向192.168.98.0送信息 ，其他应该是默认拒绝了**

`每一个访问控制列表最后一项都有一个默认的deny any语句。因此如果其他的控制语句都是deny ×××最后一定不要忘记添加permit any。`

数据包一旦匹配控制语句中的某一条则不会继续匹配之后的控制语句，因此控制语句的顺序对实验的结果有直接的影响，请注意。

这个实验只在ethernet 1/1 这个网卡加上了控制，其他网卡不收影响，因为设置的是out所以最终的影响在别人访问pc2的时候途经网卡ethernet 1/1 会 检查条件。 

#### insiderRouter的部分配置(网络控制)

```shell
Router>enable
Router#configure terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#hostname insiderRouter
insiderRouter(config)#access-list 1 permit 192.168.95.0 0.0.0.255
insiderRouter(config)#access-list 1 permit host 192.168.99.3
insiderRouter(config)#exit
%SYS-5-CONFIG_I: Configured from console by console
insiderRouter#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
#  ethernet 1/1对应 pc2 所在的网段 192.168.98.0
insiderRouter(config)#interface ethernet 1/1    
insiderRouter(config-if)#ip access-group 1 out
insiderRouter(config-if)#end
%SYS-5-CONFIG_I: Configured from console by console
insiderRouter#show access-lists 
Standard IP access list 1
    10 permit 192.168.95.0 0.0.0.255
    20 permit host 192.168.99.3
```

#### 测试 

下边文字是 教学网段的192.168.97.1 (PC3)向行政网段的192.168.98.2(PC2)发送包测试时，包传送到路由器的说明信息，这里明确地指示出来 **The outgoing port has an outbound traffic access-list with an ID of 1. The device checks the packet against the access-list.**  也就是说他会检查所有的在这个网卡上的规则，发现有个规则1在上边， 下一句说**The packet does not match the criteria of any statement in the access-list. The packet is denied and dropped by default. **也就是这个包违反了access-list 的规则1所以这个包被丢弃了

```shell
1. The routing table finds a routing entry to the destination IP address.
2. The destination network is directly connected. The device sets destination as the next-hop.
3. The device decrements the TTL on the packet.
4. The outgoing port has an outbound traffic access-list with an ID of 1. The device checks the packet against the access-list.
5. The packet does not match the criteria of any statement in the access-list. The packet is denied and dropped by default.
# 以下是翻译
1路由表查找到目标IP地址的路由条目。
2目的地网络是直接连接的。设备将目标设置为下一个跃点。
3设备减少包上的TTL。
4传出端口有一个ID为1的出站流量访问列表。设备根据访问列表检查数据包。
5数据包与访问列表中任何语句的条件都不匹配。默认情况下，数据包被拒绝并丢弃。
```

同理  教学网段的192.168.99.3 (PC1)向行政网段的192.168.98.2(PC2)发送包测试时，包传送到路由器的说明信息， 最后一句说 **The packet matches the criteria of the following statement: permit host 192.168.99.3. The packet is permitted.**也就是这个包符合规则 `permit host 192.168.99.3` 所以成功访问

```shell
1. The routing table finds a routing entry to the destination IP address.
2. The destination network is directly connected. The device sets destination as the next-hop.
3. The device decrements the TTL on the packet.
4. The outgoing port has an outbound traffic access-list with an ID of 1. The device checks the packet against the access-list.
5. The packet matches the criteria of the following statement: permit host 192.168.99.3. The packet is permitted.
# 以下是翻译
1.路由表查找到目标IP地址的路由条目。
2.目的地网络是直接连接的。设备将目标设置为下一个跃点。
3.设备减少包上的TTL。
4.传出端口有一个ID为1的出站流量访问列表。设备根据访问列表检查数据包。
5.该数据包符合以下语句的条件：permit host 192.168.99.3。包是允许的。
```



#### 理解in和out

![image-20201225194907026](C:\Users\yhn\AppData\Roaming\Typora\typora-user-images\image-20201225194907026.png)

1、如果在路由器R1上配置标准的访问控制列表，阻止PC1访问PC2，如配置的ACL为access-list 1 deny 192.168.1.254 0.0.0.0 access-list 1 permit any。如果将此访问列表应用到f0/1接口int f0/1

 ip access-group 1 (in/out)不管此处是in还是out PC1都将无法访问PC2，但是这两种情况下，数据包被阻止的情况不一样,如果应用的是 ip access-group 1 out，那么从PC1传送出来的数据包，只能传到f0/1接口，但不能通过此接口，因为此时访问列表将PC1发送的数据包给阻止了。

但是如果应用的是 ip access-group 1 in应用到f0/1接口的，那么从PC1传输的数据包可以通过f0/1接口到达PC2，但是，此时从PC2返回给PC1的流量将无法通过f0/1，因为此时f0/1的的访问列表应用的是in（即入口访问方式）,所以进入该接口的数据包将会被阻止。

2、但是如果此处用的不是标准的访问控制列表（即使用的是扩展的访问控制列表），情况将会有有所不同。

如 access-list 100 deny ip host 192.168.1.254 host 192.168.2.254 access-list 100 permit ip any any 如果将此访问控制列表应用在f0/1接口下,如 int f0/1  ip access-group 100 （out/in）此处只有是使用的是out时，才能阻止PC1访问PC2，因为但PC1发送的数据包到达f0/1接口时，就被访问控制列表所阻止了，所以无法到达目的主机PC2.

但是如果使用的是ip access-group in应用在f0/1接口下，PC1 的数据包将能通过f0/1的接口到达PC2,也许此有人会认为，PC1的数据包能通过f0/1，但是PC2返回给PC1 的数据包通不过f0/1的，因为f0/1应用的是in，可以阻止进入的流量，但是你有没有考虑到此时，从PC2返回的给PC1的数据包的源地址和目的地址是什么,`（此时返回的源地址是PC2的IP地址，目的地址是PC1的IP地址）`，而应用在**F0/1的ACL的阻止的源地址是PC1的IP地址**，目的地址是PC2的IP地址，所以当将返回给PC1的数据包的源地址和目的地址与ACL中阻止的地址相比较的时候，根本就没有匹配的，所以数据包就可以通过f0/1了。



### 实验9

#### 扩展访问控制列表的格式：

```shell
扩展访问控制列表的格式：
access-list access-list number {permit/deny} protocol +源地址+反码 +目标地址+反码+operator operan(It小于，gt大于，eq等于，neq不等于.具体可？)+端口号
1、扩展访问控制列表号的范围是100-199或者2000-2699。
2、因为默认情况下，每个访问控制列表的末尾隐含deny all，所以在每个扩展访问控制列表里面必须有：access-list 110 permit ip any any 。
3、不同的服务要使用不同的协议，比如TFTP使用的是UDP协议。
总结：扩展ACL功能很强大，他可以控制源IP，目的IP，源端口，目的端口等，能实现相当精细的控制，扩展ACL不仅读取IP包头的源地址/目的地址，还要读取第四层包头中的源端口和目的端口的IP。不过他存在一个缺点，那就是在没有硬件ACL加速的情况下，扩展ACL会消耗大量的路由器CPU资源。所以当使用中低档路由器时应尽量减少扩展ACL的条目数，将其简化为标准ACL或将多条扩展ACL合一是最有效的方法。

--------------------------------------------------------------------
扩展ACL也是在全局配置模式下进行设计的,其命令"access-list"的语法格式为：
access-list access-list-number {deny|permit} protocol source[source-wildmask] destination [destination-wildmask] [operator operand] [established] 
命令参数的含义如下：
(1) access-list-number：访问控制列表号,范围为100-199。
(2) deny：如果满足条件,数据包被拒绝通过。
(3) permit：如果满足条件,数据包允许通过。
(4) protocol：指定协议类型,如 IP/TCP/UDP/ICMP等。
(5) source：源地址。
(6) destination：目的地址。
(7) source-mask：源通配符掩码。
(8) destination-mask：目的通配符掩码。
(9) operator operand：可为it|gt|eq|neq,分别表示"小于|大于|等于|不等于"端口号。
(10) established：可选项，表示连接的状态。
可以使用扩展ACL来做到针对协议及其参数的更精细的包过滤,如TCP,UDP,ICMP和IP。在扩展ACL中,要指定上层TCP或UDP端口号,从而选择允许或拒绝的协议。常见的端口号及其对应协议为：FTP 20/21；Telnet  23；SMTP  25；TFTP  69；DNS  53；Http  80等。
```





![image-20201225191746317](C:\Users\yhn\AppData\Roaming\Typora\typora-user-images\image-20201225191746317.png)

```shell
 3.2.2 编辑修改标准ACL
  1）删除ACL  # 在全局配置中
删除编号即可删除ACL。
命令格式：R3 (config) #no access-list 1

  2）取消ACL在接口的应用
命令格式：R3 (config) #int s1/0
　　　　　R3 (config-if) #no ip access-group 1 in

  3）编辑ACL
标准ACL不支持插入或删除一行操作，可以将现有ACL拷贝到记事本里修改，然后粘贴到路由器的命令行中。

  4）查看ACL
命令格式：R3#show access-lists
　　　　　R3#show access-lists 1
```



#### 自己的测试 (禁掉了所有的端口 ，因为简单acl不能做到精细化控制 需要扩展)

```shell
insiderRouter#
%SYS-5-CONFIG_I: Configured from console by console
insiderRouter#show access-lists   # 这里只有实验8上次设置的一个 
Standard IP access list 1
    10 permit 192.168.95.0 0.0.0.255
    20 permit host 192.168.99.3

insiderRouter#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
insiderRouter(config)#access-list 2 deny host 192.168.95.4
insiderRouter(config)#access-list 2 permit any 

insiderRouter(config)#interface ethernet 1/3  
# 配置   宿舍网段 (192.168.96.0) 所在的网卡 (ethernet 1/3) 从ftp服务器传回来的数据  host(源地址) 为192.168.95.4(FTP服务器) 不得访问   宿舍网段(192.168.96.0) 
# 其实这样一点都不好  因为浪费大量资源    注意还有一点 如果 ip access-group 2 in 不会起到拦截效果
# 原因在实验8的 in 与 out 理解
insiderRouter(config-if)#ip access-group 2 out  
```



#### 老师的配置1

老师的配置比较科学 (禁止宿舍整个网段 对于192.168.95.4(ftp服务器) 21端口的访问)

```shell
insiderRouter#
%SYS-5-CONFIG_I: Configured from console by console
insiderRouter#show access-lists   # 这里只有实验8上次设置的一个 
Standard IP access list 1
    10 permit 192.168.95.0 0.0.0.255
    20 permit host 192.168.99.3

insiderRouter#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
#扩展访问控制列表的格式：
# access-list access-list number {permit/deny} protocol +源地址+反码 +目标地址+反码+operator operan(It小于，gt大于，eq等于，neq不等于.具体可？)+端口号
insiderRouter(config)#access-list 100 deny tcp 192.168.96.0 0.0.0.255 host 192.168.95.4 eq 21
insiderRouter(config)#access-list 100 permit ip any any 
insiderRouter(config)#interface fastEthernet 0/0
insiderRouter(config-if)#ip access-group 100 out
insiderRouter#show access-lists 
Standard IP access list 1
    10 permit 192.168.95.0 0.0.0.255 (2 match(es))
    20 permit host 192.168.99.3
Extended IP access list 100  # 扩展访问控制列表
    10 deny tcp 192.168.96.0 0.0.0.255 host 192.168.95.4 eq ftp (81 match(es))
    20 permit ip any any (9 match(es))

```

验证配置  创建 ftp的complex PDU 测试  从源地址 192.168.96.2(宿舍网段PC4) 到目标地址 192.168.95.4 (DMZ区域ftp服务器)

![image-20201225204239741](C:\Users\yhn\AppData\Roaming\Typora\typora-user-images\image-20201225204239741.png)



#### 老师的配置2

```shell
RageRouter>enable 
RageRouter#conf
RageRouter#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
# 1. 配置拒绝 外界218.1.2.0网段不能通过21端口访问本地的FTP服务器 192.168.95.4
RageRouter(config)#access-list 101 deny tcp 218.1.2.0 0.0.0.255 host 192.168.95.4 eq 21
# 2. 配置允许 外界218.1.2.0网段可以通过80端口访问本地的www服务器   192.168.95.3
RageRouter(config)#access-list 101 permit tcp 218.1.2.0 0.0.0.255 host 192.168.95.3 eq 80
# 3. 配置允许 外界218.1.2.0网段可以通过25端口访问本地的SMTP服务器   192.168.95.5
RageRouter(config)#access-list 101 permit tcp 218.1.2.0 0.0.0.255 host 192.168.95.5 eq 25
# 4. 配置允许 源地址为218.1.2.3(外界的www服务器) 和任何内网的设备通信
RageRouter(config)#access-list 101 permit tcp host 218.1.2.3 eq 80 any
# 配置内网中的所有机器可以与外界通信
RageRouter(config)#access-list 101 permit ip 192.168.0.0 0.0.255.255 any
RageRouter#show access-lists 
Extended IP access list 101
    10 deny tcp 218.1.2.0 0.0.0.255 host 192.168.95.4 eq ftp
    20 permit tcp 218.1.2.0 0.0.0.255 host 192.168.95.3 eq www
    30 permit tcp 218.1.2.0 0.0.0.255 host 192.168.95.5 eq smtp
    40 permit tcp host 218.1.2.3 eq www any
    50 permit ip 192.168.0.0 0.0.255.255 any
```



测试

```shell
1.  外界的pc5(218.1.2.2) 访问 内部ftp服务器 (192.168.95.4)
C:\>ftp 192.168.95.4
Trying to connect...192.168.95.4

%Error opening ftp://192.168.95.4/ (Timed out)
.
(Disconnecting from ftp server)
# 结果是报错  所以ftp服务不通
2.外界的pc5 (218.1.2.2)访问内部的 www服务 (192.168.95.3) 
打开浏览器 图在下边 图1
3. 创建复杂PDU 测试 smtp
图在下边 图2  图3 测试为success
4. 创建复杂PDU 测试 www
图4 和 图5  测试为success
除了教学网段的pc3 (192.168.97.2) 访问不了以外 其他的都能访问
```

图1

![image-20201225211828545](C:\Users\yhn\AppData\Roaming\Typora\typora-user-images\image-20201225211828545.png)

图2

![sp20201225_212122_080](F:\onedrive\桌面\sp20201225_212122_080.png)

图3

![image-20201225212801694](C:\Users\yhn\AppData\Roaming\Typora\typora-user-images\image-20201225212801694.png)

图4

![Snipaste_2020-12-25_21-30-50](F:\onedrive\桌面\yhn笔记本\网站运维\upload\Snipaste_2020-12-25_21-30-50.png)

图5

![image-20201225213311952](C:\Users\yhn\AppData\Roaming\Typora\typora-user-images\image-20201225213311952.png)









### 实验10



```
1、将192.168.95.3(www服务器) 静态NAT到218.1.1.99。 
2、将192.168.95.5(SMTP服务器) 静态NAT到218.1.1.98。 
3、将管理网段、行政网段的内部私有IP动态NAT到218.1.1.97和218.1.1.96。 
4、将教学网段、宿舍网段的内部私有IP动态PAT到218.1.1.95。 
```



#### 1、将192.168.95.3(www服务器) 静态NAT到218.1.1.99。 

```shell
# www服务器的配置
RageRouter>enable 
RageRouter#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
# 1、将192.168.95.3(www服务器) 静态NAT到218.1.1.99。
RageRouter(config)#ip nat inside source static 192.168.95.3 218.1.1.99
# 配置以太网口是对内的接口
RageRouter(config)#interface fastEthernet 0/0
RageRouter(config-if)#ip nat inside
RageRouter(config-if)#exit
# 配置串口是对外的接口 
RageRouter(config)#interface serial 1/0
RageRouter(config-if)#ip nat outside 
RageRouter#show ip nat translations 
Pro  Inside global     Inside local       Outside local      Outside global
---  218.1.1.99        192.168.95.3       ---                ---
```

![image-20210108111748708](C:\Users\yhn\AppData\Roaming\Typora\typora-user-images\image-20210108111748708.png)

![image-20210108112738183](C:\Users\yhn\AppData\Roaming\Typora\typora-user-images\image-20210108112738183.png)

![image-20210108112752558](C:\Users\yhn\AppData\Roaming\Typora\typora-user-images\image-20210108112752558.png)

#### 2、将192.168.95.5(SMTP服务器) 静态NAT到218.1.1.98。

```shell
RageRouter(config)#ip nat inside source static 192.168.95.5 218.1.1.98
RageRouter(config)#interface fastEthernet 0/0
RageRouter(config-if)#ip nat inside 
RageRouter(config)#interface serial 1/0
RageRouter(config-if)#ip nat outside
RageRouter#show ip nat translations 
Pro  Inside global     Inside local       Outside local      Outside global
---  218.1.1.98        192.168.95.5       ---                ---
---  218.1.1.99        192.168.95.3       ---                ---
tcp 218.1.1.98:25      192.168.95.5:25    218.1.2.2:1000     218.1.2.2:1000
tcp 218.1.1.99:80      192.168.95.3:80    218.1.2.2:1000     218.1.2.2:1000
tcp 218.1.1.99:80      192.168.95.3:80    218.1.2.2:1026     218.1.2.2:1026
tcp 218.1.1.99:80      192.168.95.3:80    218.1.2.2:1042     218.1.2.2:1042
tcp 218.1.1.99:80      192.168.95.3:80    218.1.2.2:1043     218.1.2.2:1043
tcp 218.1.1.99:80      192.168.95.3:80    218.1.2.2:1044     218.1.2.2:1044
```



#### 3、将管理网段(192.168.99.0)、行政网段(192.168.98.0)的内部私有IP动态NAT到218.1.1.97和218.1.1.96。

```shell
RageRouter(config)#access-list 1 permit 192.168.99.0 0.0.0.255
RageRouter(config)#access-list 1 permit 192.168.98.0 0.0.0.255
RageRouter(config)#ip nat pool mypool 218.1.1.97 218.1.1.96 netmask 255.255.255.0
RageRouter(config)#ip nat inside source list 1 pool mypool
RageRouter(config)#interface fastEthernet 0/0
RageRouter(config-if)#ip nat inside 
RageRouter(config-if)#exit
RageRouter(config)#interface serial 1/0
RageRouter(config-if)#ip nat outside 
RageRouter(config-if)#exit
```









大作业

outerRouter的配置

```shell
# rip的配置
Router>enable
Router#configure terminal 
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#hostname outerRouter
outerRouter(config)#router rip
outerRouter(config-router)#network 218.1.1.0
outerRouter(config-router)#network 218.1.99.0
```





```shell
Router(config)#hostname edgeRouter
# rip 的配置
edgeRouter(config)#router rip
edgeRouter(config-router)# network 192.168.43.0
edgeRouter(config-router)# network 218.1.1.0
# 只允许访问 80端口的服务 其他不允许
edgeRouter(config)#access-list 101 permit tcp 218.1.99.0 0.0.0.255 host 192.168.43.50 eq 80
edgeRouter(config)#interface fastEthernet 0/0
edgeRouter(config-if)#ip access-group 101 out
# 静态NAT的配置
edgeRouter(config)#ip nat inside source static 192.168.43.50 218.1.1.99
edgeRouter(config)#interface fastEthernet 0/0
edgeRouter(config-if)#ip nat inside 
edgeRouter(config-if)#exit
edgeRouter(config)#interface serial 1/0
edgeRouter(config-if)#ip nat outside 
```

