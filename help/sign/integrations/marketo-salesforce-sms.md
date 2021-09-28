---
title: 使用Adobe Sign for Salesforce和Marketo发送通知
description: 了解如何发送文本消息、电子邮件或推送通知，以让签名者知道协议即将签署
role: Admin
product: adobe sign
solution: Adobe Sign, Marketo, Document Cloud
level: Intermediate
topic-revisit: Integrations
thumbnail: KT-7248.jpg
exl-id: ac3334ec-b65f-4ce4-b323-884948f5e0a6
source-git-commit: bc79bbde966c99bdf32231ff073b49cfc759d928
workflow-type: tm+mt
source-wordcount: '690'
ht-degree: 1%

---

# 使用Adobe Sign for Salesforce和Marketo发送通知

了解如何使用Adobe Sign、Adobe Sign for Salesforce、Marketo和Marketo Salesforce同步发送文本消息、电子邮件或推送通知，以让签名者知道协议正在进行中。 要从Marketo发送通知，您首先需要购买或配置Marketo SMS管理功能。 本演练使用[Twilio SMS](https://launchpoint.marketo.com/twilio/twilio-sms-for-marketo/)，但提供其他Marketo SMS解决方案。

## 先决条件

1. 安装Marketo Salesforce同步。

   [此处提供了用于Salesforce同步的信息和最新插件。](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/salesforce-sync/understanding-the-salesforce-sync.html)

1. 安装Adobe Sign for Salesforce。

   [此处提供了有关此插件的信息。](https://helpx.adobe.com/ca/sign/using/salesforce-integration-installation-guide.html)

## 查找自定义对象

完成Marketo Salesforce同步和Adobe Sign for Salesforce配置后，Marketo管理终端中会显示多个新选项。

![管理员](assets/adminTab.png)

![对象同步](assets/salesforceAdmin.png)

1. 如果这是您的首次，请单击“同步架构”**。**&#x200B;否则，单击&#x200B;**刷新架构**。

   ![刷新](assets/refreshSchema1.png)

1. 如果全局同步正在运行，请单击&#x200B;**禁用全局同步**&#x200B;禁用。

   ![禁用](assets/disableGlobal.png)

1. 单击&#x200B;**刷新架构**。

   ![刷新2](assets/refreshSchema2.png)

## 同步自定义对象

在右侧，请参阅潜在客户、联系人和基于帐户的自定义对象。

**如果** 您希望在Salesforce中将潜在客户添加到协议时触发，请为潜在客户下的对象启用同步。

**如果** 您希望在Salesforce中将联系人添加到协议时触发，请为“联系人”下的对象启用同步。

**如果** 要在Salesforce中将帐户添加到协议时触发，请为帐户下的对象启用同步。

1. **为所** 需父级（潜在客户、联系人或帐户）下显示的自定义对象启用同步。

   ![自定义对象](assets/customObjects.png)

1. 以下资源说明如何&#x200B;**启用同步**。

   ![自定义同步1](assets/customObjectSync1.png)

   ![自定义同步2](assets/customObjectSync2.png)

1. 在自定义对象上启用同步后，重新激活同步。

   ![启用全局](assets/enableGlobal.png)

## 创建程序

1. 在Marketo的“营销活动”部分，右键单击左栏的“营销活动&#x200B;**”，选择“新建营销活动文件夹**”，并为其命名。****

   ![新建文件夹](assets/newFolder.png)

1. 右键单击创建的文件夹，选择&#x200B;**新建程序**，并为其指定名称。 将所有其他内容保留为默认值，然后单击&#x200B;**创建**。

   ![新计划1](assets/newProgram1.png)

   ![新计划2](assets/newProgram2.png)

## 设置Twilio SMS

首先确保您拥有一个活跃的Twilio帐户，并购买您需要的SMS功能。

设置Marketo - Twilio SMS网络挂接需要您帐户中的三个Twilio参数。

- 帐户SID
- 帐户令牌
- Twilio电话号码

从您的帐户中检索这些参数，现在打开您的Marketo实例。

1. 单击右上角的&#x200B;**管理员**。

   ![管理员](assets/adminTab.png)

1. 单击&#x200B;**Webhooks**，然后单击&#x200B;**新建Webhook**。

   ![Webhook](assets/webhooks.png)

1. 输入&#x200B;**Webhook名称**&#x200B;和&#x200B;**说明**。

1. 输入以下URL，确保用您的Twilio凭据替换&#x200B;**[ACCOUNT_SID]**&#x200B;和&#x200B;**[AUTH_TOKEN]**。

   ```
   https://[ACCOUNT_SID]:[AUTH_TOKEN]@API.TWILIO.COM/2010-04-01/ACCOUNTS/[ACCOUNT_SID]/Messages.json
   ```

1. 选择&#x200B;**POST**&#x200B;作为请求类型。

1. 输入以下&#x200B;**模板**，确保将&#x200B;**[MY_TWILIO_NUMBER]**&#x200B;替换为您的Twilio电话号码，将您选择的消息替换为&#x200B;**[YOUR_MESSAGE]**。

   ```
   From=%2B1[MY_TWILIO_NUMBER]&To=%2B1{{lead.Mobile Phone Number:default=edit me}}&Body=[YOUR_MESSAGE]
   ```

1. 将“请求令牌编码”设置为“表单/URL”。

1. 将响应类型设置为JSON，然后单击&#x200B;**保存**。

## 设置智能营销活动触发器

1. 在“营销活动”部分，右键单击您创建的程序，然后选择&#x200B;**新建智能营销活动**。

   ![Smart Campaign 1](assets/smartCampaign1.png)

1. 命名它，然后单击&#x200B;**创建**。

   ![Smart Campaign 2](assets/smartCampaign3.png)

   如果自定义对象同步的配置正确完成，您应在Salesforce文件夹下看到以下可用触发器。

1. 单击并将“已添加到协议”拖动到智能列表。 添加您希望对触发器具有的任何约束。

   ![已添加到协议2](assets/addedToAgreement2.png)

## 设置智能营销活动流

1. 单击Smart Campaign中的&#x200B;**流量**&#x200B;选项卡。 搜索&#x200B;**调用Webhook**&#x200B;流并将其拖动到画布上，然后选择您在上一节中创建的Webhook。

   ![调用Webhook](assets/callWebhook.png)

1. 您针对已添加到协议中的潜在客户的短信通知营销活动现已设置。

>[!TIP]
>
>本教程是[通过Adobe Sign for Salesforce和Marketo](https://experienceleague.adobe.com/?recommended=Sign-U-1-2021.1)加速销售周期课程的一部分，该课程在Experience League上免费提供！