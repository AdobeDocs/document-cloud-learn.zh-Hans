---
title: 使用GigaSign收集大容量文档
description: Gigasign允许您同时向数千人发送、收集和跟踪文档以供签名
role: User, Admin
product: adobe sign
level: Intermediate
topic-revisit: Integrations
thumbnail: 328113.jpg
exl-id: a59eab61-fe61-45c6-8137-f074e1f2b3ed
source-git-commit: 7d82422e442cbbed9420050c30ca70821e9a2cdd
workflow-type: tm+mt
source-wordcount: '268'
ht-degree: 4%

---

# 使用GigaSign收集大容量文档

Gigasign允许您同时向数千人发送、收集和跟踪文档以供签名。 它专为与您的员工和客户进行大批量通信而设计，每次批量发送最多可支持2,500位收件人。 GigaSign使用Adobe Sign API提供与MegaSign相同的功能，并包括对多个签名者、收件人组、收件人角色、协议名称、抄送等的支持。

>[!VIDEO](https://video.tv.adobe.com/v/328113?hidetitle=true)

## 下载并安装GigaSign应用程序

[下载GigaSign Zip文件](https://documentcloud.adobe.com/link/track?uri=urn:aaid:scds:US:8975dbca-98d5-4e66-9164-d21163c91c7f)

[Java 1.8下载链接(仅在需要时](https://www.oracle.com/java/technologies/javase/javase8-archive-downloads.html) ){target=&quot;_blank&quot;}

[IP地址到白名单(仅在需要时](https://helpx.adobe.com/cn/sign/system-requirements.html#IPs)){target=&quot;_blank&quot;}

## 基本设置说明

1. 登录到您的 Adobe Sign 帐户。

1. 单击&#x200B;**[!UICONTROL 组]**&#x200B;或&#x200B;**[!UICONTROL 帐户]**，以您在顶部看到的位置为准。

1. 在屏幕左侧的搜索字段中键入“访问令牌”。

1. 按右侧的“+”图标。

1. 创建具有所需范围(User_Read、Agreement_Read、Agreement_Write、Agreement_Send、Library_Read)的密钥。

1. 双击您创建的键并复制全文（它从屏幕右侧移开，以确保获得全部文本）。

1. 打开GigaSign。

1. 单击右上角的&#x200B;**[!UICONTROL 设置]**&#x200B;图标。

1. 将集成密钥粘贴到第一行中。

1. 在第二行中输入用于创建该密钥的帐户的电子邮件地址。

1. 单击&#x200B;**[!UICONTROL 提交]**。
