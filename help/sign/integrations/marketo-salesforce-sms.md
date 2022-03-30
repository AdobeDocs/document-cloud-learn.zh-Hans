---
title: 使用适用于Salesforce的Adobe Sign和Marketo发送通知
description: 了解如何发送文本消息、电子邮件或推送通知，以便让签名者知道协议即将送达
role: Admin
product: adobe sign
solution: Acrobat Sign, Marketo, Document Cloud
level: Intermediate
topic-revisit: Integrations
thumbnail: KT-7248.jpg
exl-id: ac3334ec-b65f-4ce4-b323-884948f5e0a6
source-git-commit: 089b6768cee4e3af8f1a349d5754d84aa3f4f69a
workflow-type: tm+mt
source-wordcount: '690'
ht-degree: 1%

---

# 使用适用于Salesforce的Adobe Sign和Marketo发送通知

了解如何使用Adobe Sign、适用于Salesforce的Adobe Sign、Marketo和Marketo Salesforce Sync来发送短信、电子邮件或推送通知，以便让签名者了解即将签署协议。 要从Marketo发送通知，您首先需要购买或配置Marketo SMS管理功能。 此演练使用 [Twilio SMS](https://launchpoint.marketo.com/twilio/twilio-sms-for-marketo/)，但是其他Marketo SMS解决方案可用。

## 先决条件

1. 安装Marketo Salesforce Sync。

   提供了用于Salesforce同步的信息和最新插件 [此处。](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/salesforce-sync/understanding-the-salesforce-sync.html)

1. 安装适用于Salesforce的Adobe Sign。

   有关此增效工具的信息 [此处。](https://helpx.adobe.com/ca/sign/using/salesforce-integration-installation-guide.html)

## 查找自定义对象

完成Marketo Salesforce同步和适用于Salesforce的Adobe Sign配置后，Marketo管理终端中会显示几个新选项。

![管理员](assets/adminTab.png)

![对象同步](assets/salesforceAdmin.png)

1. 单击 **同步架构** 如果是第一次了。 否则，请单击 **刷新架构**&#x200B;的

   ![刷新](assets/refreshSchema1.png)

1. 如果全局同步正在运行，请单击 **禁用全局同步**&#x200B;的

   ![禁用](assets/disableGlobal.png)

1. 单击 **刷新架构**&#x200B;的

   ![刷新2](assets/refreshSchema2.png)

## 同步自定义对象

在右侧，查看潜在客户、联系人和基于帐户的自定义对象。

**启用同步** 对于“潜在客户”下的对象（如果您希望在Salesforce中将潜在客户添加到协议时触发）。

**启用同步** （如果您希望在Salesforce中将联系人添加到协议时触发）下方的对象。

**启用同步** 对于“帐户”下的对象（如果您希望在Salesforce中将帐户添加到协议时触发）。

1. **启用同步** 对于所需父级（潜在客户、联系人或帐户）下显示的自定义对象。

   ![自定义对象](assets/customObjects.png)

1. 以下资源演示如何 **启用同步**&#x200B;的

   ![Custom Sync 1](assets/customObjectSync1.png)

   ![自定义同步2](assets/customObjectSync2.png)

1. 完成对自定义对象启用同步后，重新激活同步。

   ![启用全局](assets/enableGlobal.png)

## 创建程序

1. 在Marketo的“营销活动”部分，右键单击 **营销活动** 在左侧栏中，选择 **新的Campaign文件夹**，并为其命名。

   ![新建文件夹](assets/newFolder.png)

1. 右键单击创建的文件夹，选择 **新计划**，并为其命名。 将所有其他内容保留为默认值，然后单击 **创建**&#x200B;的

   ![新计划1](assets/newProgram1.png)

   ![新计划2](assets/newProgram2.png)

## 设置Twilio SMS

首先确保您拥有有效的Twilio帐户，并购买了所需的短信功能。

设置Marketo - Twilio SMS Webhook需要从您的帐户中提取三个Twilio参数。

- 帐户SID
- 帐户令牌
- Twilio电话号码

从您的帐户中检索这些参数，现在可以打开Marketo实例。

1. 单击 **管理员** 右上角。

   ![管理员](assets/adminTab.png)

1. 单击 **Webhook**，然后 **新建Webhook**&#x200B;的

   ![Webhook](assets/webhooks.png)

1. 输入 **Webhook名称** 和 **说明**&#x200B;的

1. 输入以下URL，并确保替换 **[ACCOUNT_SID]** 和 **[AUTH_TOKEN]** 还有你的Twilio证书。

   ```
   https://[ACCOUNT_SID]:[AUTH_TOKEN]@API.TWILIO.COM/2010-04-01/ACCOUNTS/[ACCOUNT_SID]/Messages.json
   ```

1. 选择 **POST** 作为请求类型。

1. 输入以下内容 **模板** 一定要替换 **[MY_TWILIO_NUMBER]** 和您的Twilio电话号码 **[YOUR_MESSAGE]** 并附上您选择的消息。

   ```
   From=%2B1[MY_TWILIO_NUMBER]&To=%2B1{{lead.Mobile Phone Number:default=edit me}}&Body=[YOUR_MESSAGE]
   ```

1. 将“请求令牌编码”设置为“表单/URL”。

1. 将响应类型设置为JSON，然后单击 **保存**&#x200B;的

## 设置智能营销活动触发器

1. 在“市场营销活动”部分，右键单击您创建的计划，然后选择 **新的智能营销活动**&#x200B;的

   ![Smart Campaign 1](assets/smartCampaign1.png)

1. 为其命名，然后单击 **创建**&#x200B;的

   ![Smart Campaign 2](assets/smartCampaign3.png)

   如果自定义对象同步的配置正确无误，您将在Salesforce文件夹下看到以下可供使用的触发器。

1. 单击并拖动“添加到协议”到“智能列表”。 添加您希望对触发器具有的任何约束。

   ![添加到协议2](assets/addedToAgreement2.png)

## 设置智能活动流程

1. 单击 **流量** 选项卡。 搜索并拖动 **调用Webhook** 排列到画布上，并选择您在上一部分中创建的Webhook。

   ![调用Webhook](assets/callWebhook.png)

1. 现已设置您针对添加到协议中的潜在客户的短信通知营销活动。

>[!TIP]
>
>本教程是课程的一部分 [利用适用于Salesforce的Adobe Sign和Marketo加快销售周期](https://experienceleague.adobe.com/?recommended=Sign-U-1-2021.1) 免费下载Experience League!