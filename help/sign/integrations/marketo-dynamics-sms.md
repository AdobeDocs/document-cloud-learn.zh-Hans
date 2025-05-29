---
title: 使用Acrobat Sign for Microsoft Dynamics 365和Marketo发送通知
description: 了解如何发送短信、电子邮件或推送通知，让签名者知道协议正在执行中
feature: Integrations
role: Admin
solution: Acrobat Sign, Marketo Engage, Document Cloud
level: Intermediate
topic: Integrations
topic-revisit: Integrations
jira: KT-7249
thumbnail: KT-7249.jpg
exl-id: 2e0de48c-70bf-4dc5-8251-88e7399f588a
source-git-commit: a88ec5a68aa2a02ec2f118332ec31f47d3d5d300
workflow-type: tm+mt
source-wordcount: '652'
ht-degree: 0%

---

# 使用Acrobat Sign for Microsoft Dynamics 365和Marketo发送通知

了解如何使用Acrobat Sign、Acrobat Sign for Microsoft Dynamic、Marketo和Marketo Microsoft Dynamics Sync发送文本消息、电子邮件或推送通知，让签名者知道协议正在执行中。 要从Marketo发送通知，您首先需要购买或配置Marketo短信管理功能。 此演练使用[Twilio SMS](https://launchpoint.marketo.com/twilio/twilio-sms-for-marketo/)，但其他Marketo SMS解决方案可用。

## 先决条件

1. 安装Marketo Microsoft Dynamics Sync。

   [此处](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/marketo-plugin-releases-for-microsoft-dynamics.html)提供了Microsoft Dynamics Sync的信息和最新插件。

1. 安装适用于Microsoft Dynamics的Acrobat Sign。

   [此处](https://helpx.adobe.com/ca/sign/using/microsoft-dynamics-integration-installation-guide.html)提供了有关此插件的信息。

## 查找自定义对象

完成Marketo Microsoft Dynamics Sync和Acrobat Sign for Dynamics配置后，Marketo管理终端中会显示两个新选项。

![管理员](assets/adminTerminal.png)

* 单击&#x200B;**[!UICONTROL Dynamics实体同步]**。

  在同步自定义实体之前，必须禁用同步。 如果这是您第一次这样做，请单击&#x200B;**[!UICONTROL 同步架构]**。 否则，请单击&#x200B;**[!UICONTROL 刷新架构]**。

  ![刷新](assets/refreshSchema.png)

## 同步自定义对象

1. 在右侧，找到基于[!UICONTROL 潜在客户]、[!UICONTROL 联系人]和[!UICONTROL 帐户]的自定义对象。

   * 如果要在Dynamics中将潜在客户添加到协议时触发，请为潜在客户下的对象&#x200B;**[!UICONTROL 启用同步]**。

   * 如果要在Dynamics中将联系人添加到协议时触发，请为联系人下的对象启用&#x200B;**[!UICONTROL 同步]**。

   * 如果要在Dynamics中将帐户添加到协议时触发，请为帐户下的对象启用&#x200B;**[!UICONTROL 同步]**。

   * 为所需父级（潜在客户、联系人或帐户）下的协议对象&#x200B;**启用同步**。

   ![自定义对象](assets/enableSyncDynamics.png)

1. 在新窗口中，在协议下选择所需的属性。

   启用&#x200B;**[!UICONTROL 约束]**&#x200B;和&#x200B;**[!UICONTROL 触发器]**&#x200B;下的框以使其公开参与营销活动。

   ![自定义同步1](assets/entitySync1.png)

   ![自定义同步2](assets/entitySync2.png)

1. 在自定义对象上启用同步后重新激活同步。

   返回[!UICONTROL 管理员终端]，单击&#x200B;**[!UICONTROL Microsoft Dynamics]**，然后单击&#x200B;**[!UICONTROL 启用同步]**。

   ![Microsoft Dynamics](assets/microsoftDynamics.png)

   ![启用全局](assets/enableGlobalDynamics.png)

## 创建程序

1. 在[!UICONTROL 营销活动]中，右键单击左侧栏上的&#x200B;**[!UICONTROL 营销活动]**，选择&#x200B;**[!UICONTROL 新建营销活动文件夹]**，然后为其命名。

   ![新建文件夹](assets/newFolder.png)

1. 右键单击已创建的文件夹，选择&#x200B;**[!UICONTROL 新建程序]**，然后为其命名。

   将其余所有内容保留为默认值，然后单击“**[!UICONTROL 创建]**”。

   ![新程序1](assets/newProgram1.png)

   ![新程序2](assets/newProgram2.png)

## 设置[!DNL Twilio]短信

首先，请确保您拥有有效的[!DNL Twilio]帐户并购买了您需要的短信功能。

设置Marketo - [!DNL Twilio] SMS Webhook需要从您的帐户中使用三个[!DNL Twilio]参数。

* 帐户SID
* 帐户令牌
* Twilio电话号码

从您的帐户检索这些参数，现在打开您的Marketo实例。

1. 单击右上角的&#x200B;**[!UICONTROL 管理员]**。

   ![管理员](assets/adminTab.png)

1. 单击&#x200B;**[!UICONTROL Webhook]**，然后单击&#x200B;**[!UICONTROL 新建Webhook]**。

   ![Webhook](assets/webhooks.png)

1. 输入&#x200B;**[!UICONTROL Webhook名称]**&#x200B;和&#x200B;**[!UICONTROL 描述]**。

1. 输入以下URL，并确保将`ACCOUNT_SID`和`AUTH_TOKEN`替换为您的[!DNL Twilio]凭据。

   ```
   https://[ACCOUNT_SID]:[AUTH_TOKEN]@API.TWILIO.COM/2010-04-01/ACCOUNTS/[ACCOUNT_SID]/Messages.json
   ```

1. 选择&#x200B;**[!UICONTROL POST]**&#x200B;作为请求类型。

1. 输入以下&#x200B;**模板**，并确保将`MY_TWILIO_NUMBER`替换为您的[!DNL Twilio]电话号码，将`YOUR_MESSAGE`替换为您选择的消息。

   ```
   From=%2B1[MY_TWILIO_NUMBER]&To=%2B1{{lead.Mobile Phone Number:default=edit me}}&Body=[YOUR_MESSAGE]
   ```

1. 将&#x200B;**[!UICONTROL 请求令牌编码]**&#x200B;设置为&#x200B;*表单/URL*。

1. 将响应类型设置为&#x200B;*JSON*，然后单击&#x200B;**[!UICONTROL 保存]**。

## 设置Smart Campaign触发器

1. 在“营销活动”部分，右键单击您创建的计划，然后选择&#x200B;**[!UICONTROL 新建Smart Campaign]**。

   ![Smart Campaign 1](assets/smartCampaign1.png)

1. 为其命名，然后单击&#x200B;**[!UICONTROL 创建]**。

   ![Smart Campaign 2](assets/smartCampaign3.png)

   您应该会在Microsoft文件夹下看到多个可用的触发器。

1. 单击&#x200B;**[!UICONTROL “已添加到协议”]**&#x200B;并将其拖动到&#x200B;**[!UICONTROL “智能列表”]**，然后添加任何要对该触发器设置的约束。

   ![已添加到协议](assets/addedToAgreementDynamics.png)

## 设置智能营销活动流

1. 单击[!UICONTROL 智能营销活动]中的&#x200B;**[!UICONTROL 流]**&#x200B;选项卡。

   搜索&#x200B;**调用Webhook**&#x200B;流并将其拖动到画布上，并选择您在上一节中创建的Webhook。

   ![调用Webhook](assets/callWebhook.png)

1. 现已设置您针对添加到协议中的潜在顾客的短信通知营销活动。
