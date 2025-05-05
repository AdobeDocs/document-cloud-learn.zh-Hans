---
title: 使用适用于Salesforce的Acrobat Sign和Marketo发送通知
description: 了解如何发送短信、电子邮件或推送通知，让签名者知道协议正在执行中
feature: Integrations
role: Admin
solution: Acrobat Sign, Marketo, Document Cloud
level: Intermediate
jira: KT-7247
topic: Integrations
topic-revisit: Integrations
thumbnail: KT-7248.jpg
exl-id: ac3334ec-b65f-4ce4-b323-884948f5e0a6
source-git-commit: 51d1a59999a7132cb6e47351cc39a93d9a38eaeb
workflow-type: tm+mt
source-wordcount: '646'
ht-degree: 0%

---

# 使用Acrobat Sign为[!DNL Salesforce]和[!DNL Marketo]发送通知

了解如何发送短信、电子邮件或推送通知，让签名者知道即将使用Acrobat Sign、适用于Salesforce的Acrobat Sign、Marketo和Marketo Salesforce Sync签署协议。 要从Marketo发送通知，您首先需要购买或配置Marketo短信管理功能。 此演练使用[Twilio SMS](https://launchpoint.marketo.com/twilio/twilio-sms-for-marketo/)，但其他Marketo SMS解决方案可用。

## 先决条件

1. 安装Marketo Salesforce Sync。

   [此处](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/salesforce-sync/understanding-the-salesforce-sync.html?lang=zh-Hans)提供了有关Salesforce Sync的信息和最新插件。

1. 安装适用于Salesforce的Acrobat Sign。

   [此处](https://helpx.adobe.com/ca/sign/using/salesforce-integration-installation-guide.html)提供了有关此插件的信息。

## 查找自定义对象

Marketo Salesforce Sync和适用于Salesforce的Acrobat Sign配置完成后，Marketo管理员终端中会显示多个新选项。

![管理员](assets/adminTab.png)

![对象同步](assets/salesforceAdmin.png)

1. 如果这是您第一次这样做，请单击&#x200B;**同步架构**。 否则，请单击&#x200B;**刷新架构**。

   ![刷新](assets/refreshSchema1.png)

1. 如果全局同步正在运行，请单击&#x200B;**禁用全局同步**&#x200B;将其禁用。

   ![禁用](assets/disableGlobal.png)

1. 单击&#x200B;**刷新架构**。

   ![刷新2](assets/refreshSchema2.png)

## 同步自定义对象

在右侧，请参阅基于潜在客户、联系人和客户的自定义对象。

如果您希望在将潜在客户添加到Salesforce中的协议时触发，请为潜在客户下的对象&#x200B;**启用同步**。

如果要在将联系人添加到Salesforce中的协议时触发，请为联系人下的对象&#x200B;**启用同步**。

如果要在Salesforce中将帐户添加到协议时触发，请为帐户下的对象启用&#x200B;**同步**。

1. 为所需父级（潜在客户、联系人或帐户）下显示的自定义对象&#x200B;**启用同步**。

   ![自定义对象](assets/customObjects.png)

1. 以下资源显示如何&#x200B;**启用同步**。

   ![自定义同步1](assets/customObjectSync1.png)

   ![自定义同步2](assets/customObjectSync2.png)

1. 对自定义对象启用同步后，请重新激活同步。

   ![启用全局](assets/enableGlobal.png)

## 创建程序

1. 在Marketo的“营销活动”部分，右键单击左栏上的&#x200B;**“营销活动”**，选择&#x200B;**“新营销活动文件夹”**，然后为其命名。

   ![新建文件夹](assets/newFolder.png)

1. 右键单击已创建的文件夹，选择&#x200B;**新建程序**，然后为其命名。 将其余所有内容保留为默认值，然后单击“**创建**”。

   ![新程序1](assets/newProgram1.png)

   ![新程序2](assets/newProgram2.png)

## 设置Twilio SMS

首先，确保您拥有有效的Twilio帐户并购买了您需要的短信功能。

设置Marketo - Twilio SMS Webhook需要从您的帐户中使用三个Twilio参数。

- 帐户SID
- 帐户令牌
- Twilio电话号码

从您的帐户检索这些参数，现在即可打开您的Marketo实例。

1. 单击右上角的&#x200B;**管理员**。

   ![管理员](assets/adminTab.png)

1. 单击&#x200B;**Webhook**，然后单击&#x200B;**新建Webhook**。

   ![Webhook](assets/webhooks.png)

1. 输入&#x200B;**Webhook名称**&#x200B;和&#x200B;**描述**。

1. 输入以下URL，并确保将&#x200B;**[ACCOUNT_SID]**&#x200B;和&#x200B;**[AUTH_TOKEN]**&#x200B;替换为您的Twilio凭据。

   ```
   https://[ACCOUNT_SID]:[AUTH_TOKEN]@API.TWILIO.COM/2010-04-01/ACCOUNTS/[ACCOUNT_SID]/Messages.json
   ```

1. 选择&#x200B;**POST**&#x200B;作为请求类型。

1. 输入以下&#x200B;**模板**，并确保将&#x200B;**[MY_TWILIO_NUMBER]**&#x200B;替换为您的Twilio电话号码，将&#x200B;**[YOUR_MESSAGE]**&#x200B;替换为您选择的消息。

   ```
   From=%2B1[MY_TWILIO_NUMBER]&To=%2B1{{lead.Mobile Phone Number:default=edit me}}&Body=[YOUR_MESSAGE]
   ```

1. 将请求令牌编码设置为表单/URL。

1. 将响应类型设置为JSON，然后单击“**保存**”。

## 设置Smart Campaign触发器

1. 在“营销活动”部分，右键单击您创建的计划，然后选择&#x200B;**新建Smart Campaign**。

   ![Smart Campaign 1](assets/smartCampaign1.png)

1. 为其命名，然后单击&#x200B;**创建**。

   ![Smart Campaign 2](assets/smartCampaign3.png)

   如果正确配置了自定义对象同步，您应该会在Salesforce文件夹下看到以下可供使用的触发器。

1. 单击添加到协议并将其拖动到智能列表。 添加您希望对触发器具有的任何约束。

   ![已添加到协议2](assets/addedToAgreement2.png)

## 设置智能营销活动流

1. 单击“智能营销活动”中的&#x200B;**流**&#x200B;选项卡。 搜索&#x200B;**调用Webhook**&#x200B;流并将其拖动到画布上，并选择您在上一节中创建的Webhook。

   ![调用Webhook](assets/callWebhook.png)

1. 现已设置您针对添加到协议中的潜在顾客的短信通知营销活动。
