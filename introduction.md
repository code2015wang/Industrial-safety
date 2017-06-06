# 0x01专业术语

------

`SCADA`：数据采集与监视控制系统

`ICS`：工业控制系统

`DCS`：分布式控制系统/集散控制系统

`PCS`：过程控制系统

`ESD`：应急停车系统

`PLC`：可编程序控制器(Programmable Logic Controller)

`RTU`：远程终端控制系统

`IED`：智能监测单元

`HMI`：人机界面(Human Machine Interface)

`MIS`：管理信息系统(Management Information System)

`SIS`： 生产过程自动化监控和管理系统（Supervisory Information System）

`MES`：制造执行管理系统

PLC: 一个典型的PLC通常包括电源、网络通信（串口或以太网）、输入、输出。这些通常封装在独立的模块中以方便扩展。PLC的运行是通过扫描输入信号，然后根据逻辑运算输入，最后将运算结果写入输出。PLC通常是控制网络中的传感器。PLC的输出通常是连接到控制网络中的物理元件，例如电机、泵或阀门。PLC的计算架构相当简单，处理器并没有太高的`运行速度并且没有高级功能。值得注意的是，没有内存管理，运行在PLC上的所有进程都是直接访问内存的。

SCADA vs DCS

SCADA系统通常应用于不需要实时控制的场合，其设备通常会分布在一个很大的地理区域，并且为了操作方便需要远程访问。集散控制系统（DCS）通常应用于实时控制（例如工厂、组装线），其设备通常部署在一个区域。标准的SCADA网络由多层构成，层与层之间形成关键系统间的隔离。SCADA设备最初的设计是用来部署在已经隔离的网络中。



# 0x02协议端口及测试脚本

------

- **协议科普**

  - **Modbus**

    MODBUS协议定义了一个与基础通信层无关的简单协议数据单元（PDU）。特定总线或网络上的MODBUS协议映射能够在应用数据单元（ADU）上引入一些附加域。

    ![img](http://static.wooyun.org//drops/20150911/2015091107544572757pic22.png)

    安全问题：

    - 缺乏认证：仅需要使用一个合法的Modbus地址和合法的功能码即可以建立一个Modbus会话
    - 缺乏授权：没有基于角色的访问控制机制， 任意用户可以执行任意的功能。
    - 缺乏加密：地址和命令明文传输， 可以很容易地捕获和解析

  - **PROFIBUS**

    一种用于工厂自动化车间级监控和现场设备层数据通信与控制的现场总线技术，可实现现场设备层到车间级监控的分散式数字控制和现场通信网络

  - **DNP3**

    DNP(Distributed Network Protocol,分布式网络协议)是一种应用于自动化组件之间的通讯协议，常见于电力、水处理等行业。

    简化OSI模型，只包含了物理层，数据层与应用层的体系结构（EPA）。

    SCADA可以使用DNP协议与主站、RTU、及IED进行通讯。

  - **ICCP**

    [电力控制中心](http://www.doc88.com/p-0127254402976.html)通讯协议。

  - **OPC**

    过程控制的OLE （OLE for Process Control）。

    OPC包括一整套接口、属性和方法的标准集，用于过程控制和制造业自动化系统。

  - **BACnet**

    楼宇自动控制网络数据通讯协议（A Data Communication Protocol for Building Automation and Control Networks）。

    BACnet 协议是为计算机控制采暖、制冷、空调HVAC系统和其他建筑物设备系统定义服务和协议

  - **CIP**

    通用工业协议，被deviceNet、ControINet、EtherNet/IP三种网络所采用。

  - **Siemens S7**

    属于第7层的协议，用于西门子设备之间进行交换数据，通过TSAP，可加载MPI,DP,以太网等不同物理结构总线或网络上，PLC一般可以通过封装好的通讯功能块实现。

  - **其他工控协议**

    IEC 60870-5-104、EtherNet/IP、Tridium Niagara Fox、Crimson V3、OMRON FINS、PCWorx、ProConOs、MELSEC-Q。按需求自行查阅资料。

- **信息探测**

  - **协议测试脚本**

    ![img](http://static.wooyun.org//drops/20150911/2015091107544851182pic32.png)

    *PS：简要测试，大量脚本自行测试。*

  - **相关搜索引擎**

    Shodan搜索

    *PS：Shodan搜索引擎介绍 http://drops.wooyun.org/tips/2469*

    Zoomeye搜索

    *PS：敏感信息，你懂得。*

  - **Ethernet/IP 44818**

    `nmap -p 44818 --script enip-enumerate.nse  85.132.179.*`

  - **Modbus 502**

    `nmap --script modicon-info.nse -Pn -p 502 -sV 91.83.43.*`

    ​

  - **IEC 61870-5-101/104 2404**

    `nmap -Pn -n -d --script iec-identify.nse  --script-args=iec-identify -p 2404 80.34.253.*`

    ​

  - **Siemens S7 102**

    `nmap -p 102 --script s7-enumerate -sV 140.207.152.*`

    `nmap -d --script mms-identify.nse  --script-args='mms-identify.timeout=500' -p 102  IP`

  - **Tridium Niagara Fox 1911**

    `nmap -p 1911 --script fox-info 99.55.238.*`

  - **意义何在**

    上述NSE脚本意义：

    1. 定位工控系统及协议模块。
    2. 收集目标工控的信息，如版本、内网IP、模块、硬件信息等。
    3. 结合对应的NSE脚本进一步拓展，例如自定义空间搜素引擎。

- **脚本资源**

  - **Github测试脚本**

    [https://github.com/atimorin/scada-tools](https://github.com/atimorin/scada-tools) [https://github.com/atimorin/PoC2013](https://github.com/atimorin/PoC2013) [https://github.com/drainware/scada-tools](https://github.com/drainware/scada-tools) [https://github.com/drainware/nmap-scada](https://github.com/drainware/nmap-scada)

  - **Exploit-db测试脚本**

    [https://www.exploit-db.com/exploits/19833/](https://www.exploit-db.com/exploits/19833/) [https://www.exploit-db.com/exploits/19832/](https://www.exploit-db.com/exploits/19832/) [https://www.exploit-db.com/exploits/19831/](https://www.exploit-db.com/exploits/19831/) [https://www.exploit-db.com/search/?action=search&description=scada&e_author=](https://www.exploit-db.com/search/?action=search&description=scada&e_author=)

  ## 使用wireshark分析常见工控协议

1. Siemens S7 https://github.com/wireshark/wireshark/tree/master/epan/dissectors/packet-s7comm.c 西门子PLC支持的通讯协议

2 MMS(IEC61850) https://github.com/wireshark/wireshark/tree/master/asn1/mms 输配电通讯协议

3 GOOSE(IEC61850) https://github.com/wireshark/wireshark/tree/master/asn1/goose 输配电通讯协议

4 SV(IEC61850) https://github.com/wireshark/wireshark/tree/master/asn1/sv 输配电通讯协议

5 Modbus https://github.com/wireshark/wireshark/tree/master/epan/dissectors/packet-mbtcp.c 工控标准协议

6 OPC DA https://github.com/wireshark/wireshark/tree/master/epan/dissectors/packet-dcom.c 工控标准协议

7 FF HSE https://github.com/wireshark/wireshark/tree/master/epan/dissectors/packet-ff.c 基金会现场总线以太网通信协定

8 IEC 104 https://github.com/wireshark/wireshark/tree/master/epan/dissectors/packet-iec104.c 输配电通讯协议

9 Ethernet POWERLINK https://github.com/wireshark/wireshark/tree/master/epan/dissectors/packet-epl.c 开放式实时以太网通信

10 OPC UA https://github.com/wireshark/wireshark/tree/master/plugins/opcua/opcua.c OPC新一代标准

11 HART-IP https://github.com/wireshark/wireshark/tree/master/epan/dissectors/packet-hartip.c 高速可寻址远程传感器协议

12 CoAP https://github.com/wireshark/wireshark/tree/master/epan/dissectors/packet-coap.c 轻量应用层协议

13 Omron FINS https://github.com/wireshark/wireshark/tree/master/epan/dissectors/packet-omron-fins.c 欧姆龙PLC支持的通讯协定

14 openSAFETY https://github.com/wireshark/wireshark/tree/master/epan/dissectors/packet-opensafety.c 开源安全应用协议

15 EGD(Ethernet Global Data) https://github.com/wireshark/wireshark/tree/master/epan/dissectors/packet-egd.c GE Fanuc为PLC开发的通讯协定

16 DNP3 https://github.com/wireshark/wireshark/tree/master/epan/dissectors/packet-dnp.c 分布式网络协议，主要用于电力行业

17 Sinec H1 https://github.com/wireshark/wireshark/tree/master/epan/dissectors/packet-h1.c 西门子PLC支持的通讯协议

18 Profinet https://github.com/wireshark/wireshark/tree/master/plugins/profinet/ 开放式的工业以太网通讯协定

19 EtherCAT https://github.com/wireshark/wireshark/tree/master/plugins/ethercat/ 德国Beckhoff公司推动的开放式实时以太网通讯协定

20 SERCOS III https://github.com/wireshark/wireshark/tree/master/epan/dissectors/packet-sercosiii.c 实时以太网通讯协定

21 RTPS https://github.com/wireshark/wireshark/tree/master/epan/dissectors/packet-rtps.c 实时流传输协议

22 TTEthernet https://github.com/wireshark/wireshark/tree/master/epan/dissectors/packet-tte.c 实时以太网通讯协定

23 CDT https://github.com/wireshark/wireshark/tree/master/epan/dissectors/packet-cdt.c 远动规约

24 EtherNet/IP https://github.com/wireshark/wireshark/tree/master/epan/dissectors/packet-etherip.c 工业通讯协定（Industrial Protocol），是一种CIP的实现方式，由罗克韦尔自动化公司所设计

25 CIP https://github.com/wireshark/wireshark/tree/master/epan/dissectors/packet-cip.c 通用工业协定

26 CIP Safety https://github.com/wireshark/wireshark/tree/master/epan/dissectors/packet-cipsafety.c 安全通用工业协定

27 DeviceNet https://github.com/wireshark/wireshark/tree/master/epan/dissectors/ packet-devicenet.c 一种CIP的实现方式，由Allen-Bradley公司所设计

28 BACnet https://github.com/wireshark/wireshark/tree/master/epan/dissectors/packet-bacnet.c 楼宇自动控制网络数据通讯协议

29 KNXnet/IP https://github.com/wireshark/wireshark/tree/master/epan/dissectors/packet-knxnetip.c 住宅和楼宇控制标准

30 Lontalk https://github.com/wireshark/wireshark/tree/master/epan/dissectors/packet-lon.c 埃施朗公司的LonWorks技术所使用的通讯协议

31 CANopen https://github.com/wireshark/wireshark/tree/master/epan/dissectors/packet-canopen.c 控制局域网通讯协定

32 SAE J1939 https://github.com/wireshark/wireshark/tree/master/epan/dissectors/packet-j1939.c 一种CAN的变种，适用在农业车辆及商用车辆

33 USITT DMX512-A https://github.com/wireshark/wireshark/tree/master/epan/dissectors/packet-dmx.c 灯光控制数据传输协议

34 BSSAP/BSAP https://github.com/wireshark/wireshark/tree/master/epan/dissectors/packet-bssap.c 由Bristol Babcock Inc发展的通讯协定

35 Gryphon https://github.com/wireshark/wireshark/tree/master/plugins/gryphon 车用通讯协定

36 ZigBee https://github.com/wireshark/wireshark/tree/master/epan/dissectors/packet-zbee.h 开放式的无线通讯协定

+ 如下工具可以快速帮你熟悉、调试、仿真、测试DNP3协议：

### opendnp3

opendnp3是automatak开源的基于IEEE-1815 (DNP3)的开源协议栈。

### Aegis™

Aegis™是automatak开源的一个针对工控协议进行模糊测试(Fuzz)的框架，其中包含对DNP3协议模糊测试的模块，官方的Project Robus项目曾经发布过多个应用在DNP3协议健壮性上的漏洞，官方在发布MODBUS模块后貌似没有再继续开源了。

### Protocol Test Harness

Protocol Test Harness是Triangle MicroWorks公司开发的一款协议仿真、调试软件，软件可以仿真多种工控协议包括DNP3，可以方便你完成调试、仿真。

## 利用标准且公开的工控协议对工控系统及设备进行识别

| 协议名         | 默认端口  | 请求功能      | 响应内容            |
| ----------- | ----- | --------- | --------------- |
| MMS         | 102   | 请求厂商和模块信息 | 厂商和模块信息         |
| Modbus      | 502   | 43功能码     | 设备厂商和产品模块信息     |
| IEC104      | 2404  | 启动连接      | 启动确认            |
| DNP3        | 20000 | 请求链路状态    | 连接确认            |
| OPCUA       | 4840  | 查找服务器请求   | 应用名称            |
| EtherNet/IP | 44818 | 枚举设备信息    | 制造厂商、模块信息、串号等信息 |
| BACnet      | 47808 | 枚举设备信息    | 制造厂商、模块信息等信息    |

## 利用私有工控协议对工控系统及设备进行识别

| 协议名               | 默认端口  | 请求功能     | 相应内容            |
| ----------------- | ----- | -------- | --------------- |
| Siemens S7        | 102   | 读SZL     | PLC的模块信息、版本、串号等 |
| Codesys           | 1200  | 读系统信息    | 系统信息            |
| Mitsubishi MELSEC | 5007  | 读CPU信息   | PLC模块信息         |
| Omron  FINS       | 9600  | 读CPU单元信息 | PLC的模块信息        |
| GE SRTP           | 18245 | 读CPU单元信息 | PLC模块信息         |

## 利用传统服务特征对工控系统即设备进行识别

| 厂商         | 设备        | 服务（端口）    | 特征                            |
| ---------- | --------- | --------- | ----------------------------- |
| Siemens    | s7  1200  | HTTP（80）  | Location：/Default.mwsl        |
| Siemens    | s7 300    | HTTP（80）  | Location： /Portal0000.html    |
| Siemens    | s7  1200  | SNMP(161) | Siemens, SIMATIC  S7 CPU-1200 |
| Siemens    | s7 300    | SNMP(161) | Siemens，SIMATIC  NET          |
| Mitsubishi | Q  Series | FTP（21）   | QnUDE(H)CPU FTP server ready  |
| Moxa       | NPort     | HTTP(80)  | Server: MoxaHttp              |

