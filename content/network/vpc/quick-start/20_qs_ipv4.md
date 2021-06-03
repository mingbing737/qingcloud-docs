---
title: "搭建 IPv4 VPC 网络"
linkTitle: "搭建 IPv4 VPC 网络"
date: 2021-05-18T10:08:56+09:00
description:
draft: false
weight: 20
---

## 操作场景

本文旨在指引您快速搭建一个具有 IPv4 CIDR 的 VPC 网络，并为 VPC 网络中的云服务器绑定一个公网 IP，实现公网访问的目的。

## 前提条件

- 已注册青云账号并通过实名认证。
- 确保账户有足够金额。若需要充值，请前往[充值页面](https://console.qingcloud.com/finance/wallet)。

## 操作步骤

### 步骤1：创建 VPC 网络及私有网络

按照以下操作，创建一个 VPC 网络并在 VPC 网络中添加一个私有网络。

1. 登录[管理控制台](https://console.qingcloud.com/pek3)。

2. 在控制台导航栏中，选择**产品与服务** > **网络服务** > **VPC 网络**，进入**VPC 网络**页面。

3. 点击**创建 VPC 网络**，弹出**创建 VPC 网络**页面。

   <img src="/network/vpc/_images/501010_创建VPC1.0.png" alt="501010_创建VPC1.0" style="zoom:60%;" />

4. 配置VPC网络信息。

   > **说明**：
   >
   > 以下配置为本操作中使用的示例，若您有其他特别需求，可参见[创建 VPC 网络](/network/vpc/manual/vpcnet/10_create_vpc/)查看参数详细配置说明。

   - 名称：输入 VPC 网络的名称。如：vpc-test。

   - IPv4 地址范围：选择 VPC 网络的 IPv4 网段。

   - IPv6 网络地址：选择**关闭 IPv6**。

   - 管理路由器属性

     - 类型：不同类型可支持的管理流量转发能力不同，根据需要选择。

     - 安全组：使用**缺省安全组**。

5. 点击**创建**，等待创建完成。

6. 点击已创建好的 VPC，进入 VPC 详情页面。默认展示**私有网络**页签。

7. 在**私有网络**页签区域，点击<img src="../../_images/4020_vpc详情_添加Vxnet图标.png" alt="4020_vpc详情_添加Vxnet图标" style="zoom:50%;" />，弹出**创建/连接私有网络**页面。

   <img src="/network/vpc/_images/501030_bind_vxnet_3.png" alt="501030_bind_vxnet_3" style="zoom:50%;" />

8. 设置私有网络网络基本信息。

   > **说明**：
   >
   > 以下配置为本操作中使用的示例，若您有其他特别需求，可参见[为 VPC 添加私有网络](/network/vpc/manual/vpcnet/30_bind_vxnet/) 查看参数详细配置说明。

   - 私有网络名称：如：Vxnet-test。
   - 部署方式：选择**单可用区部署**。
   - 网络ACL：选择**无**。
   - 高级选项：使用默认配置即可。

9. 点击**提交**。

### 步骤2：在私有网络中创建云服务器

按照以下操作，在[步骤1](#步骤1创建vpc网络及私有网络)中创建的私有网络中创建云服务器。

1. 登录[管理控制台](https://console.qingcloud.com/pek3)。

2. 在顶部菜单栏中，选择**产品与服务** > **网络服务** > **VPC 网络**，进入**VPC 网络**页面。

3. 点击在[步骤1](#步骤1创建vpc网络及私有网络)中已创建好的 VPC 网络，进入 VPC 详情页面。

4. 在**私有网络** > **资源列表**区，点击**创建资源** > **云服务器**。

5. 配置云服务器信息。

   本操作中，云服务器的网络配置使用默认配置，即云服务器使用我们在[步骤1](../#步骤1：创建VPC网络及私有网络)中所创建的私有网络“Vxnet-test”进行通信。其他配置说明请参见[创建云服务器](/compute/vm/manual/vm_instance/#创建云服务器)。

6. 点击**创建**。

   创建完成后，资源列表中将显示云服务信息。

### 步骤3：申请公网IP并绑定到云服务器

按照以下操作，申请一个公网IP并绑定到[步骤2](#步骤2在私有网络中创建云服务器)中所创建的云服务器。

1. 登录[管理控制台](https://console.qingcloud.com/pek3)。

2. 在顶部菜单栏中，选择**产品与服务** > **网络服务** > **公网IP**，进入**公网IP**页面。

3. 点击**申请公网IP**，弹出申请公网IP页面。

   <img src="../../_images/4020_申请公网IP_按需.png" alt="4020_申请公网IP_按需" style="zoom:50%;" />

4. 设置公网 IP 参数，点击**提交**。

   > **说明**：
   >
   > 以下配置为本操作中使用的示例，若您有其他特别需求，可参见 [外部绑定公网 IP](/network/eip/manual/ipv4/outband_ipv4/) 查看详细配置说明。

   - 计费方式：选择**按需计费**。
   - 名称：公网IP名称。如：IPv4-test。
   - 地址：公网IP地址。如：192.168.128.5。
   - 模式：选择**按带宽计费**。
   - 带宽上限：设置为“10Mbps”。
   - IP组：使用默认的**同城专线互联IP组**。
   - 绑定方式：选择**外部绑定**。
   - 数量：设置为“1”。

5. 在**公网IP**管理页面，找到已申请的公网IP“IPv4-test”，右键单击，选择**分配到云服务器**。

6. 选择在[步骤2](#步骤2在私有网络中创建云服务器)中所创建的云服务器，点击**提交**。

7. 在提示框中，点击**确认**。

### 步骤4：启动实例验证公网连通性

按照以下操作，测试云服务器的网络连通性。

1. 启动并登录已绑定公网IP的云服务器。

2. 执行`ping`命令验证是否可公网通信。 如 `ping www.qingcloud.com` 测试公网连通性。

   <img src="../../_images/4020_ping.png" alt="4020_ping" style="zoom:50%;" />


