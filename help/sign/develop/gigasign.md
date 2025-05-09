---
title: 使用GigaSign收集大量文档
description: 借助Gigasign，您可以同时向数千人发送、收集和跟踪文档，以供其签名
feature: Workflow
role: Developer
level: Intermediate
jira: KT-6626
topic-revisit: Integrations
thumbnail: 328113.jpg
exl-id: a59eab61-fe61-45c6-8137-f074e1f2b3ed
source-git-commit: ba9931920ab3bfb6ea38a92cac4a35da1d0295cd
workflow-type: tm+mt
source-wordcount: '298'
ht-degree: 1%

---

# 使用GigaSign收集大量文档

借助Gigasign，您可以同时向数千人发送、收集和跟踪文档，以供其签名。 它旨在与您的员工和客户进行高容量通信 — 每次批量发送最多支持2,500个收件人。 GigaSign使用Acrobat Sign API提供与MegaSign相同的功能，并且包括支持多个签名者、接收者组、接收者角色、协议名称、Carbon Copy等。

>[!IMPORTANT]
>
>GigaSign将不再更新到最新版本的Java或Acrobat Sign，并且仅提供有限的支持。 正在将GigaSign的功能添加到产品的[批量发送](https://experienceleague.adobe.com/docs/document-cloud-learn/sign-learning-hub/admin-set-up/getting-started-admin/megasign.html?lang=zh-Hans&)功能下。 对于所有没有明确要求使用GigaSign的使用案例，请使用批量发送。

>[!VIDEO](https://video.tv.adobe.com/v/3453520?quality=12&learn=on&hidetitle=true&captions=chi_hans)

## 下载并安装GigaSign应用程序

[下载GigaSign Zip文件](https://acrobat.adobe.com/id/urn:aaid:sc:US:001cf62d-1cab-46c7-aa96-661ac8680206)

[Java 1.8下载链接（仅在需要时）](https://www.oracle.com/java/technologies/javase/javase8-archive-downloads.html){target="_blank"} 

[要列入白名单的IP地址（仅在需要时）](https://helpx.adobe.com/cn/sign/system-requirements.html#IPs){target="_blank"}

## 基本设置说明

1. 登录到您的Acrobat Sign帐户。

1. 单击&#x200B;**[!UICONTROL 组]**&#x200B;或&#x200B;**[!UICONTROL 帐户]**，具体取决于您在顶部看到的内容。

1. 在屏幕左侧的搜索字段中键入“访问令牌”。

1. 按右侧的“+”图标。

1. 创建具有所需范围(User_Read、Agreement_Read、Agreement_Write、Agreement_Send、Library_Read)的密钥。

1. 双击您创建的密钥并复制全文（它将在屏幕右侧消失，因此请确保全部获得）。

1. 打开GigaSign。

1. 单击右上角的&#x200B;**[!UICONTROL 设置]**&#x200B;图标。

1. 将集成密钥粘贴到第一行。

1. 在第二行中输入用于创建该键的帐户的电子邮件地址。

1. 单击&#x200B;**[!UICONTROL “提交”]**。
