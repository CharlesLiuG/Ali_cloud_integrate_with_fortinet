# Slide 1

# Slide 2

![Image](images_dir1/slide_2_7.jpg)

业务绑定主网卡

业务绑定主网卡辅助IP

CEN-TR引流统一gateway

业务绑定辅助网卡辅助IP

# Slide 3

业务拓扑

客户环境：原来是多VPC直接绑定EIP在server通过阿里云源生防火墙进行安全防护；

现在想要做独立的安全出口和替换阿里云防火墙，需要Fortigate支持多网卡绑定EIP，对现网改变最小；

目前我们推荐实例类型C6e.xlarge，支持4块网卡，我们可以通过3块网卡实现HA部署，Port4作为除了主网卡业务外额外一块业务网卡。

目前阿里云c6e.xlarge，支持每块网卡配置15个IP

![Image](images_dir1/slide_3_340.png)

# Slide 4

![Image](images_dir1/slide_4_4.png)

业务发布部署

# Slide 5

EIP绑定主网卡辅助IP方式

通过alicloud shell命令行进行配置，不支持console业务配置管理

aliyun vpc AssociateEipAddress --region 'cn-beijing' --RegionId 'cn-beijing' --PrivateIpAddress '10.0.0.10' --InstanceType NetworkInterface --AllocationId 'eip-2zefdc6f0azncb9udtuba' --VpcId 'vpc-2ze77es17ue641djas06h' --InstanceId 'eni-2ze46nbb45cmecqveige’

region / RegionId  可用区ID   PrivateIpAddress: 需要绑定主网卡私网IP InstanceType:网卡类型 AllocationId: EIP标识 VpcId:VPC标识  
InstanceId:需要绑定ENI网卡标识

![Image](images_dir1/slide_5_5.png)

![Image](images_dir1/slide_5_6.wmf)

# Slide 6

绑定后EIP状态

![Image](images_dir1/slide_6_5.png)

# Slide 7

![Image](images_dir1/slide_7_3.png)

业务访问测试-1

![Image](images_dir1/slide_7_6.png)

# Slide 8

![Image](images_dir1/slide_8_5.png)

绑定辅助网卡辅助IP-FGT预配置

![Image](images_dir1/slide_8_8.png)

![Image](images_dir1/slide_8_10.png)

# Slide 9

绑定辅助网卡辅助IP-Alicloud配置

![Image](images_dir1/slide_9_8.png)

# Slide 10

![Image](images_dir1/slide_10_5.png)

业务访问测试-1

![Image](images_dir1/slide_10_12.png)

![Image](images_dir1/slide_10_18.png)

![Image](images_dir1/slide_10_14.png)

# Slide 11

HA切换测试

![Image](images_dir1/slide_11_4.png)

![Image](images_dir1/slide_11_6.png)

执行主设备重启操作，HA进行切换

绑定在主网卡的业务丢8个包后可以网络回复正常业务也正常访问。

绑定在主网卡辅助IP业务在FGT切换后无法正常访问

7.2.6，7.4.4之后版本已支持主网卡辅助IP，辅助网卡EIP 切换

# Slide 12

网卡状态

![Image](images_dir1/slide_12_4.png)

# Slide 13

![Image](images_dir1/slide_13_2.png)

FGT正常切换

![Image](images_dir1/slide_13_5.png)

# Slide 14

辅助网卡EIP切换-1

![Image](images_dir1/slide_14_7.png)

# Slide 15

辅助网卡EIP切换-2

![Image](images_dir1/slide_15_5.png)

# Slide 16

辅助网卡EIP切换-3

![Image](images_dir1/slide_16_4.png)

# Slide 17

辅助网卡EIP切换-4

![Image](images_dir1/slide_17_10.png)

![Image](images_dir1/slide_17_12.png)

# Slide 18

辅助网卡EIP切换-5

![Image](images_dir1/slide_18_4.png)

![Image](images_dir1/slide_18_9.png)

网络层面丢3个包

# Slide 19

CEN-TR 配置

![Image](images_dir1/slide_19_4.png)

![Image](images_dir1/slide_19_6.png)

# Slide 20

CEN-TR 配置-attachment

![Image](images_dir1/slide_20_4.png)

手工关闭route sync ，否则会影响sec vpc Internet路由

# Slide 21

CEN-TR 配置-Rtb1

App TR RTB :  Internet RTB 关联 APP VPC路由

![Image](images_dir1/slide_21_11.png)

![Image](images_dir1/slide_21_13.png)

# Slide 22

CEN-TR 配置-Rtb2

SEC TR RTB : Sec RTB 关联 Sec VPC路由

![Image](images_dir1/slide_22_17.png)

![Image](images_dir1/slide_22_19.png)

# Slide 23

Security Vpc Fgt Rtb

![Image](images_dir1/slide_23_10.png)

Sec FGT RTB : 关联 Sec VPC路由 ,回指server路由,隐式Internet出局路由

![Image](images_dir1/slide_23_14.png)

![Image](images_dir1/slide_23_17.png)

# Slide 24

Security Vpc Server Rtb

![Image](images_dir1/slide_24_10.png)

Sec Server RTB : 关联 Sec VPC Server,回指server路由和本VPC指向Internet外网路由

![Image](images_dir1/slide_24_13.png)

![Image](images_dir1/slide_24_15.png)

# Slide 25

Sec VPC Attach Transit Rtb

![Image](images_dir1/slide_25_8.png)

![Image](images_dir1/slide_25_10.png)

![Image](images_dir1/slide_25_12.png)

Sec attach RTB : 关联 Sec VPC attach,指向FGT inside ENI解决APP VPC Server Internet route

# Slide 26

App Vpc Server Rtb

![Image](images_dir1/slide_26_6.png)

App Server RTB : 关联 App Server attach,指向internet route

![Image](images_dir1/slide_26_11.png)

![Image](images_dir1/slide_26_13.png)

# Slide 27

App VPC Attach transit rtb

![Image](images_dir1/slide_27_3.png)

![Image](images_dir1/slide_27_6.png)

![Image](images_dir1/slide_27_8.png)

App attach RTB : 关联 App VPC attach,指向internet route

# Slide 28

![Image](images_dir1/slide_28_3.png)

业务访问测试：App vpc - internet

![Image](images_dir1/slide_28_6.png)

![Image](images_dir1/slide_28_10.png)

![Image](images_dir1/slide_28_8.png)

# Slide 29

![Image](images_dir1/slide_29_3.png)

业务访问测试：App vpc – internet   Anti-virus

![Image](images_dir1/slide_29_6.png)

# Slide 30

业务访问测试：sec vpc- app vpc

![Image](images_dir1/slide_30_4.png)

![Image](images_dir1/slide_30_8.png)

# Slide 31

# Slide 32

通过DNAT将对端Server IP映射成inside接口IP或者其他子网IP

![Image](images_dir1/slide_32_10.png)

![Image](images_dir1/slide_32_11.png)

# Slide 33

![Image](images_dir1/slide_33_4.png)

![Image](images_dir1/slide_33_7.png)

![Image](images_dir1/slide_33_12.png)

![Image](images_dir1/slide_33_14.png)

# Slide 34

