---
title: 使用GigaSign收集大量文档
description: Gigasign允许您同时向数千人发送、收集和跟踪文档以供签名
role: User, Admin
product: adobe sign
level: Intermediate
jira: KT-6626
topic-revisit: Integrations
thumbnail: 328113.jpg
exl-id: a59eab61-fe61-45c6-8137-f074e1f2b3ed
source-git-commit: aa8fd589d214879f2bfcb6bc54576c707532fd6f
workflow-type: tm+mt
source-wordcount: '264'
ht-degree: 5%

---

# 使用GigaSign收集大量文档

Gigasign允许您同时向数千人发送、收集和跟踪文档以供签名。 它旨在与您的员工和客户进行大量通信，每次批量发送可支持多达2,500个收件人。 GigaSign使用Acrobat Sign API提供与MegaSign相同的功能，并支持多个签名者、收件人组、收件人角色、协议名称、抄送等。

>[!VIDEO](https://video.tv.adobe.com/v/328113?quality=12&learn=on&hidetitle=true)

## 下载并安装GigaSign应用程序

[下载GigaSign Zip文件](https://documentcloud.adobe.com/link/track?uri=urn:aaid:scds:US:8975dbca-98d5-4e66-9164-d21163c91c7f)

[Java 1.8下载链接（仅在需要时可用）](https://www.oracle.com/java/technologies/javase/javase8-archive-downloads.html) {target="_blank"}

[白名单中的IP地址（仅在需要时）](https://helpx.adobe.com/cn/sign/system-requirements.html#IPs){target="_blank"}

## 基本设置说明

1. 登录到您的 Acrobat Sign 帐户.

1. 单击 **[!UICONTROL 编组]** 或 **[!UICONTROL 帐户]**，以您在上边看到的内容为准。

1. 在屏幕左侧的搜索字段中键入“访问令牌”。

1. 按右侧的“+”图标。

1. 使用所需的范围(User_Read、Agreement_Read、Agreement_Write、Agreement_Send、Library_Read)创建密钥。

1. 双击您创建的密钥并复制全文（它从屏幕向右移动，因此请确保您已获得全部内容）。

1. 打开GigaSign。

1. 单击 **[!UICONTROL 设置]** 图标。

1. 将集成密钥粘贴到第一行。

1. 在第二行输入用于创建该密钥的帐户的电子邮件地址。

1. 单击&#x200B;**[!UICONTROL 提交]**。
