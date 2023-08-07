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
source-git-commit: 452299b2b786beab9df7a5019da4f3840d9cdec9
workflow-type: tm+mt
source-wordcount: '688'
ht-degree: 1%

---

# 使用Acrobat Sign发送通知 [!DNL Salesforce] 和 [!DNL Marketo]

了解如何发送短信、电子邮件或推送通知，让签名者知道即将使用Acrobat Sign、适用于Salesforce的Acrobat Sign、Marketo和Marketo Salesforce Sync签署协议。 要从Marketo发送通知，您首先需要购买或配置Marketo短信管理功能。 此演练使用 [Twilio SMS](https://launchpoint.marketo.com/twilio/twilio-sms-for-marketo/)，但其他Marketo短信解决方案可用。

## 先决条件

1. 安装Marketo Salesforce Sync。

   已提供适用于Salesforce Sync的信息和最新插件 [这里。](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/salesforce-sync/understanding-the-salesforce-sync.html)

1. 安装适用于Salesforce的Acrobat Sign。

   有关此插件的信息，请参阅 [这里。](https://helpx.adobe.com/ca/sign/using/salesforce-integration-installation-guide.html)

## 查找自定义对象

Marketo Salesforce Sync和适用于Salesforce的Acrobat Sign配置完成后，Marketo管理员终端中会显示多个新选项。

![管理员](assets/adminTab.png)

![对象同步](assets/salesforceAdmin.png)

1. 点击 **同步架构** 如果这是你第一次。 否则，请单击 **刷新架构**.

   ![刷新](assets/refreshSchema1.png)

1. 如果全局同步正在运行，请单击 **禁用全局同步**.

   ![禁用](assets/disableGlobal.png)

1. 点击 **刷新架构**.

   ![刷新2](assets/refreshSchema2.png)

## 同步自定义对象

在右侧，请参阅基于潜在客户、联系人和客户的自定义对象。

**启用同步** 如果您希望在将潜在客户添加到Salesforce中的协议时触发，请为潜在客户下的对象执行此操作。

**启用同步** 用于联系人下的对象，如果您希望在将联系人添加到Salesforce中的协议时触发。

**启用同步** 用于帐户下的对象（如果您要在将帐户添加到Salesforce中的协议时触发）。

1. **启用同步** 显示在所需父级（潜在客户、联系人或帐户）下的自定义对象。

   ![自定义对象](assets/customObjects.png)

1. 以下资源演示了如何 **启用同步**.

   ![自定义同步1](assets/customObjectSync1.png)

   ![自定义同步2](assets/customObjectSync2.png)

1. 对自定义对象启用同步后，请重新激活同步。

   ![启用全局](assets/enableGlobal.png)

## 创建程序

1. 在Marketo的Marketing Activities部分，右键单击 **营销活动** 在左栏中，选择 **新市场活动文件夹**，并为其命名。

   ![新建文件夹](assets/newFolder.png)

1. 右键单击已创建的文件夹，选择 **新计划**，并为其命名。 将其余所有内容保留为默认值，然后单击 **创建**.

   ![新计划1](assets/newProgram1.png)

   ![新计划2](assets/newProgram2.png)

## 设置Twilio SMS

首先，确保您拥有有效的Twilio帐户并购买了您需要的短信功能。

设置Marketo - Twilio SMS Webhook需要从您的帐户中使用三个Twilio参数。

- 帐户SID
- 帐户令牌
- Twilio电话号码

从您的帐户检索这些参数，现在即可打开您的Marketo实例。

1. 单击 **管理员** 右上角的。

   ![管理员](assets/adminTab.png)

1. 单击 **Webhook**，然后 **新Webhook**.

   ![Webhook](assets/webhooks.png)

1. 输入 **Webhook名称** 和 **描述**.

1. 输入以下URL，并确保将 **[ACCOUNT_SID]** 和 **[AUTH_TOKEN]** 使用您的Twilio凭据。

   ```
   https://[ACCOUNT_SID]:[AUTH_TOKEN]@API.TWILIO.COM/2010-04-01/ACCOUNTS/[ACCOUNT_SID]/Messages.json
   ```

1. 选择 **POST** 作为您的请求类型。

1. 输入以下内容 **模板** 并确保将 **[MY_TWILIO_NUMBER]** 用你的Twilio电话号码和 **[您的消息(_M)]** 你选了什么口信。

   ```
   From=%2B1[MY_TWILIO_NUMBER]&To=%2B1{{lead.Mobile Phone Number:default=edit me}}&Body=[YOUR_MESSAGE]
   ```

1. 将请求令牌编码设置为表单/URL。

1. 将“Response type”（响应类型）设置为JSON，然后单击 **保存**.

## 设置Smart Campaign触发器

1. 在Marketing Activities部分，右键单击您创建的计划，然后选择 **新的Smart Campaign**.

   ![Smart Campaign 1](assets/smartCampaign1.png)

1. 为其命名，然后单击 **创建**.

   ![Smart Campaign 2](assets/smartCampaign3.png)

   如果正确配置了自定义对象同步，您应该会在Salesforce文件夹下看到以下可供使用的触发器。

1. 单击添加到协议并将其拖动到智能列表。 添加您希望对触发器具有的任何约束。

   ![已添加到协议2](assets/addedToAgreement2.png)

## 设置智能营销活动流

1. 单击 **流量** 选项卡。 搜索并拖动 **调用Webhook** 流到画布上，并选择您在上一节中创建的Webhook。

   ![调用Webhook](assets/callWebhook.png)

1. 现已设置您针对添加到协议中的潜在顾客的短信通知营销活动。

>[!TIP]
>
>本教程是本课程的一部分 [使用适用于Salesforce的Acrobat Sign和Marketo加快销售周期](https://experienceleague.adobe.com/?recommended=Sign-U-1-2021.1) 可在Experience League上免费获得！