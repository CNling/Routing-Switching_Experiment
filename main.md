# 目录：

[TOC]

---

---

## 1、[实验目标](https://github.com/lihouwen83/Routing-Switching_Experiment/blob/master/Obj%20expt.md)

---

## 2、实验步骤：

---

##### 2.1、终端的基本配置

> PC10：

![PC10_BasicCfg](.\images\PC10_BasicCfg.png)

> Client11：

![Client11_BasicCfg](.\images\Client11_BasicCfg.png)

> PC20：

![PC20_BasicCfg](.\images\PC20_BasicCfg.png)

> Client21：

![Client21_BasicCfg](.\images\Client21_BasicCfg.png)

> Server30：

![Server30_BasicCfg](.\images\Server30_BasicCfg.png)

> Server40：

![Server40_BasicCfg](.\images\Server40_BasicCfg.png)

> Server100：

![Server100_BasicCfg](.\images\Server100_BasicCfg.png)

> Server200：

![Server200_BasicCfg](.\images\Server200_BasicCfg.png)

---

##### 2.2、实现 PC10 与 Client11、PC20 与 Client21 互通

**SW1 配置：**

```
//重命名
<Huawei>system-view
[Huawei]sysname SW1

//创建 vlan
[SW1]vlan 10
[SW1-vlan10]q

//配置端口 E0/0/10、E0/0/11、G0/0/1
[SW1]int e0/0/10
[SW1-Ethernet0/0/10]port link-type access
[SW1-Ethernet0/0/10]port default vlan 10
[SW1-Ethernet0/0/10]int e0/0/11
[SW1-Ethernet0/0/11]port link-type access
[SW1-Ethernet0/0/11]port default vlan 10
[SW1-Ethernet0/0/10]int g0/0/1
[SW1-GigabitEthernet0/0/1]port link-type trunk
[SW1-GigabitEthernet0/0/1]port trunk allow-pass vlan 10
[SW1-GigabitEthernet0/0/1]q
```

**SW3 配置：**

```
//重命名
<Huawei>system-view
[Huawei]sysname SW3

//创建 vlan
[SW3]vlan 10
[SW1-vlan10]q

//创建 Vlanif
[SW3]int Vlanif 10
[SW3-Vlanif10]ip address 192.168.10.254 24
[SW3-Vlanif10]q

//配置端口 G0/0/1
[SW3]int g0/0/1
[SW3-GigabitEthernet0/0/1]port link-type trunk
[SW3-GigabitEthernet0/0/1]port trunk allow-pass vlan 10
[SW3-GigabitEthernet0/0/1]q
```

---

**测试连通性：**PC10 ping Client11

![PC10-Client11](D:\Desktop\images\PC10-Client11.png)

> Pass（通过）

---

**SW2 配置：**

```
//重命名
<Huawei>sys
[Huawei]sysname SW2

//创建 vlan
[SW2]vlan 20
[SW2-vlan20]q

//配置端口 E0/0/20、E0/0/21、G0/0/2 全部隶属于 vlan 20
[SW2]int e0/0/20
[SW2-Ethernet0/0/20]port link-type access
[SW2-Ethernet0/0/20]port default vlan 20
[SW2-Ethernet0/0/20]int e0/0/21
[SW2-Ethernet0/0/21]port link-type access
[SW2-Ethernet0/0/21]port default vlan 20
[SW2-Ethernet0/0/21]int g0/0/2
[SW2-GigabitEthernet0/0/2]port link-type trunk
[SW2-GigabitEthernet0/0/2]port trunk allow-pass vlan 20
[SW2-GigabitEthernet0/0/2]q
```

**SW4 配置：**

```
//重命名
<Huawei>sys
[Huawei]sysname SW4

//创建 vlan
[SW4]vlan 20
[SW4-vlan20]q

//创建 Vlanif
[SW4]int Vlanif 20
[SW4-Vlanif20]ip add 192.168.20.254 24
[SW4-Vlanif20]q

//配置端口 G0/0/2
[SW4]int g0/0/2
[SW4-GigabitEthernet0/0/2]port link-type trunk
[SW4-GigabitEthernet0/0/2]port trunk allow-pass vlan 20
[SW4-GigabitEthernet0/0/2]q
```

---

**测试连通性：**PC20 ping Client21

![pc20-client21](D:\Desktop\images\pc20-client21.png)

> Pass

---

---

##### 2.3、\*实现工程部与财务部、研发部、人事部互通 \*实现研发部与财务部、工程部、人事部互通

**SW3 配置：**

```
//创建 vlan
[SW3]vlan batch 3 30 34

//创建 Vlanif
[SW3]int Vlanif 30
[SW3-Vlanif30]ip address 192.168.30.254 24
[SW3-Vlanif30]q

//配置端口 G0/0/23
[SW3]int g0/0/23
[SW3-GigabitEthernet0/0/23]port link-type access
[SW3-GigabitEthernet0/0/23]port default vlan 30
[SW3-GigabitEthernet0/0/23]q
```

---

**测试连通性：**PC10 ping Server30

​ ![PC10-Server30](D:\Desktop\images\PC10-Server30.png)

> Pass

---

SW4 配置：

```
//创建 vlan
[SW4]vlan batch 4 34 40

//创建 Vlanif
[SW4]int Vlanif 40
[SW4-Vlanif40]ip add 192.168.40.254 24

//配置端口 G0/0/24
[SW4-GigabitEthernet0/0/24]port link-type access
[SW4-GigabitEthernet0/0/24]port default vlan 40
[SW4-GigabitEthernet0/0/24]q

```

---

**测试连通性：**PC20 ping Server40

![PC20-Server40](D:\Desktop\images\PC20-Server40.png)

> Pass

---

**SW3 配置：**

```
//创建 Vlanif
[SW3]int Vlanif 34
[SW3-Vlanif34]ip add 192.168.34.3 24
[SW3-Vlanif34]q

//配置端口 G0/0/13
[SW3]int g0/0/13
[SW3-GigabitEthernet0/0/13]port link-type trunk
[SW3-GigabitEthernet0/0/13]port trunk allow-pass vlan 34
[SW3-GigabitEthernet0/0/13]q
```

**SW4 配置：**

```
//创建 Vlanif
[SW4]int Vlanif 34
[SW4-Vlanif34]ip add 192.168.34.4 24
[SW4-Vlanif34]q

//配置端口 G0/0/14
[SW4]int g0/0/14
[SW4-GigabitEthernet0/0/14]port link-type trunk
[SW4-GigabitEthernet0/0/14]port trunk allow-pass vlan 34
[SW4-GigabitEthernet0/0/14]q
```

> 接下来配置<u>静态路由协议</u>

**SW3 配置：**

```
//配置静态路由协议
[SW3]ip route-static 192.168.20.0 24 192.168.34.4 preference 9
[SW3]ip route-static 192.168.40.0 24 192.168.34.4 preference 9
```

**SW4 配置：**

```
//配置静态路由协议
[SW4]ip route-static 192.168.10.0 24 192.168.34.3 preference 9
[SW4]ip route-static 192.168.30.0 24 192.168.34.3 preference 9
```

---

**测试连通性：**PC10 ping PC20

![PC10 - PC20](D:\Desktop\images\PC10 - PC20.png)

> Pass

---

PC10 ping Server40

![PC10-Server40](D:\Desktop\images\PC10-Server40.png)

> Pass

---

PC20 ping Server30

![PC20 - Server30](D:\Desktop\images\PC20 - Server30.png)

> Pass

---

---

##### 2.4、\*实现工程部与 Server100 互通 \*实现研发部与 Server200

**SW3 配置：**

```
//创建 Vlanif
[SW3]int Vlanif 3
[SW3-Vlanif3]ip add 192.168.3.3 24
[SW3-Vlanif3]q

//配置端口 G0/0/3
[SW3]int GigabitEthernet 0/0/3
[SW3-GigabitEthernet0/0/3]port link-type access
[SW3-GigabitEthernet0/0/3]port default vlan 3

//配置 OSPF
[SW3]ospf 100 router-id 30.30.30.30
[SW3-ospf-100]area 0
[SW3-ospf-100-area-0.0.0.0]network 192.168.3.3 0.0.0.0
[SW3-ospf-100-area-0.0.0.0]network 30.30.30.30 0.0.0.0
[SW3-ospf-100-area-0.0.0.0]network 192.168.10.254 0.0.0.0
```

**SW4 配置：**

```
//创建 Vlanif 3
[SW4]int Vlanif 4
[SW4-Vlanif4]ip add 192.168.4.4 24
[SW4-Vlanif4]q

//配置端口 G0/0/4
[SW4]int GigabitEthernet 0/0/4
[SW4-GigabitEthernet0/0/4]port link-type access
[SW4-GigabitEthernet0/0/4]port default vlan 4

//配置 OSPF
[SW4]ospf 100 router-id 40.40.40.40
[SW4-ospf-100]area 0
[SW4-ospf-100-area-0.0.0.0]network 192.168.4.4 0.0.0.0
[SW4-ospf-100-area-0.0.0.0]network 40.40.40.40 0.0.0.0
[SW4-ospf-100-area-0.0.0.0]network 192.168.20.254 0.0.0.0
```

**GW-AR1 配置：**

> 因为路由器不用配置 vlan，所有我们直接配置端口的 IP 地址

```
//重命名
<Huawei>sys
[Huawei]sysname GW-AR1

//配置端口 G0/0/1、G0/0/2、G0/0/0
[GW-AR1]int g0/0/1
[GW-AR1-GigabitEthernet0/0/1]ip add 192.168.3.1 24
[GW-AR1-GigabitEthernet0/0/1]int g0/0/2
[GW-AR1-GigabitEthernet0/0/2]ip add 192.168.4.1 24
[GW-AR1-GigabitEthernet0/0/2]int g0/0/0
[GW-AR1-GigabitEthernet0/0/0]ip add 202.108.12.1 24

//配置OSPF
[GW-AR1]ospf 100 router-id 1.1.1.1
[GW-AR1-ospf-100]area 0
[GW-AR1-ospf-100-area-0.0.0.0]network 192.168.3.1 0.0.0.0
[GW-AR1-ospf-100-area-0.0.0.0]network 192.168.4.1 0.0.0.0
[GW-AR1-ospf-100-area-0.0.0.0]network 202.108.12.1 0.0.0.0
[GW-AR1-ospf-100-area-0.0.0.0]network 1.1.1.1 0.0.0.0
```

**AR2 配置：**

> 注意：设置 OSPF 认证时，务必切换至 S 3/0/0 端口下

```
//重命名
<Huawei>sys
[Huawei]sysname AR2

//配置端口 G0/0/0、S3/0/0
[AR2]int g0/0/0
[AR2-GigabitEthernet0/0/0]ip add 202.108.12.2 24
[AR2-GigabitEthernet0/0/0]int s3/0/0
[AR2-Serial3/0/0]ip add 202.108.23.2 24
[AR2-Serial3/0/0]q

//配置 OSPF
[AR2]ospf 100 router-id 2.2.2.2
[AR2-ospf-100]area 0
[AR2-ospf-100-area-0.0.0.0]network 202.108.12.2 0.0.0.0
[AR2-ospf-100-area-0.0.0.0]network 202.108.23.2 0.0.0.0
[AR2-ospf-100-area-0.0.0.0]network 2.2.2.2 0.0.0.0
[AR2-ospf-100-area-0.0.0.0]q
[AR2-ospf-100]q

//配置 OSPF认证
[AR2]int s3/0/0	//切换至 S 3/0/0 端口下
[AR2-Serial3/0/0]ospf authentication-mode md5 23 plain test
```

**AR3 配置：**

```
//重命名
<Huawei>SYS
[Huawei]sysname AR3

//配置端口 S3/0/0、G0/0/1、G0/0/2
[AR3]int s3/0/0
[AR3-Serial3/0/0]ip add 202.108.23.3 24
[AR3-Serial3/0/0]int g0/0/1
[AR3-GigabitEthernet0/0/1]ip add 202.108.100.3 24
[AR3-GigabitEthernet0/0/1]int g0/0/2
[AR3-GigabitEthernet0/0/2]ip add 202.108.200.3 24
[AR3-GigabitEthernet0/0/2]q

//配置 OSPF
[AR3]ospf 100 router-id 3.3.3.3
[AR3-ospf-100]area 0
[AR3-ospf-100-area-0.0.0.0]network 202.108.23.3 0.0.0.0
[AR3-ospf-100-area-0.0.0.0]network 202.108.100.3 0.0.0.0
[AR3-ospf-100-area-0.0.0.0]network 202.108.200.3 0.0.0.0
[AR3-ospf-100-area-0.0.0.0]network 3.3.3.3 0.0.0.0
[AR3-ospf-100-area-0.0.0.0]q
[AR3-ospf-100]q

//配置 OSPF认证
[AR3]int s3/0/0 //切换至 S 3/0/0 端口下
[AR3-Serial3/0/0]ospf authentication-mode md5 23 plain test
```

---

**测试连通性：**PC10 ping Server100

![PC10-Server100](D:\Desktop\images\PC10-Server100.png)

> Pass

---

PC20 ping Server200

![PC20-Server200](D:\Desktop\images\PC20-Server200.png)

> Pass

---

---

---

> ^v^
