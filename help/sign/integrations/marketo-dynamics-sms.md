---
title: 使用Adobe Sign for Microsoft Dynamics 365和Marketo发送通知
description: 了解如何发送文本消息、电子邮件或推送通知，以让签名者知道协议即将签署
role: Admin
product: adobe sign
solution: Adobe Sign, Marketo, Document Cloud
level: Intermediate
topic-revisit: Integrations
thumbnail: KT-7249.jpg
exl-id: 2e0de48c-70bf-4dc5-8251-88e7399f588a
source-git-commit: bcddb0ee106239f2786debaed908b2a2ec5ce792
workflow-type: tm+mt
source-wordcount: '690'
ht-degree: 1%

---

# 使用Adobe Sign for Microsoft Dynamics 365和Marketo发送通知

了解如何使用Adobe Sign、Adobe Sign for Microsoft Dynamic、Marketo和Marketo Microsoft Dynamics Sync发送文本消息、电子邮件或推送通知，以让签名者知道协议正在进行中。 要从Marketo发送通知，您首先需要购买或配置Marketo SMS管理功能。 本演练使用[Twilio SMS](https://launchpoint.marketo.com/twilio/twilio-sms-for-marketo/)，但提供其他Marketo SMS解决方案。

## 先决条件

1. 安装Marketo Microsoft Dynamics同步。

   [此处提供了Microsoft Dynamics Sync的信息和最新插件。](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/marketo-plugin-releases-for-microsoft-dynamics.html)

1. 安装Adobe Sign for Microsoft Dynamics。

   [此处提供了有关此插件的信息。](https://helpx.adobe.com/ca/sign/using/microsoft-dynamics-integration-installation-guide.html)

## 查找自定义对象

完成Marketo Microsoft Dynamics Sync和Adobe Sign for Dynamics配置后，Marketo管理终端中会显示两个新选项。

![管理员](assets/adminTerminal.png)

* 单击&#x200B;**[!UICONTROL 动态实体同步]**。

   同步自定义实体之前必须禁用同步。 如果这是您的首次，请单击“同步架构”**[!UICONTROL 。]**&#x200B;否则，单击&#x200B;**[!UICONTROL 刷新架构]**。

   ![刷新](assets/refreshSchema.png)

## 同步自定义对象

1. 在右侧，找到[!UICONTROL Lead]、[!UICONTROL Contact]和[!UICONTROL 基于帐户]的自定义对象。

   * **[!UICONTROL 如果]** 您希望在Dynamics中向协议添加潜在客户时触发，请为潜在客户下的对象启用同步。

   * **[!UICONTROL 如果]** 要在Dynamics中将联系人添加到协议时触发，请为“联系人”下的对象启用同步。

   * **[!UICONTROL 如果]** 要在Dynamics中将帐户添加到协议时触发，请为帐户下的对象启用同步。

   * **为所** 需父级（潜在客户、联系人或帐户）下的协议对象启用同步。

   ![自定义对象](assets/enableSyncDynamics.png)

1. 在新窗口中，选择您要在“协议”下选择的属性。

   启用&#x200B;**[!UICONTROL Constraint]**&#x200B;和&#x200B;**[!UICONTROL Trigger]**&#x200B;下的框，将它们显示给您的营销活动。

   ![自定义同步1](assets/entitySync1.png)

   ![自定义同步2](assets/entitySync2.png)

1. 在对自定义对象启用同步后重新激活同步。

   返回到[!UICONTROL 管理终端]，单击&#x200B;**[!UICONTROL Microsoft Dynamics]**，然后单击&#x200B;**[!UICONTROL 启用同步]**。

   ![Microsoft Dynamics](assets/microsoftDynamics.png)

   ![启用全局](assets/enableGlobalDynamics.png)

## 创建程序

1. 在[!UICONTROL 营销活动]中，右键单击左栏中的&#x200B;**[!UICONTROL 营销活动]**，选择&#x200B;**[!UICONTROL 新建营销活动文件夹]**&#x200B;并命名它。

   ![新建文件夹](assets/newFolder.png)

1. 右键单击创建的文件夹，选择&#x200B;**[!UICONTROL 新建程序]**，并为其指定名称。

   将所有其他内容保留为默认值，然后单击&#x200B;**[!UICONTROL 创建]**。

   ![新计划1](assets/newProgram1.png)

   ![新计划2](assets/newProgram2.png)

## 设置[!DNL Twilio] SMS

首先确保您有一个有效的[!DNL Twilio]帐户，并购买了所需的SMS功能。

设置Marketo - [!DNL Twilio] SMS webhook需要帐户中的三个[!DNL Twilio]参数。

* 帐户SID
* 帐户令牌
* Twilio电话号码

从您的帐户检索这些参数，现在打开Marketo实例。

1. 单击右上角的&#x200B;**[!UICONTROL 管理员]**。

   ![管理员](assets/adminTab.png)

1. 单击&#x200B;**[!UICONTROL Webhooks]**，然后单击&#x200B;**[!UICONTROL 新建Webhook]**。

   ![Webhook](assets/webhooks.png)

1. 输入&#x200B;**[!UICONTROL Webhook名称]**&#x200B;和&#x200B;**[!UICONTROL 说明]**。

1. 输入以下URL，确保用您的[!DNL Twilio]凭据替换`ACCOUNT_SID`和`AUTH_TOKEN`。

   ```
   https://[ACCOUNT_SID]:[AUTH_TOKEN]@API.TWILIO.COM/2010-04-01/ACCOUNTS/[ACCOUNT_SID]/Messages.json
   ```

1. 选择&#x200B;**[!UICONTROL POST]**&#x200B;作为请求类型。

1. 输入以下&#x200B;**Template**，确保用您的[!DNL Twilio]电话号码替换`MY_TWILIO_NUMBER`，用您选择的消息替换`YOUR_MESSAGE`。

   ```
   From=%2B1[MY_TWILIO_NUMBER]&To=%2B1{{lead.Mobile Phone Number:default=edit me}}&Body=[YOUR_MESSAGE]
   ```

1. 将&#x200B;**[!UICONTROL 请求令牌编码]**&#x200B;设置为&#x200B;*表单/URL*。

1. 将响应类型设置为&#x200B;*JSON*，然后单击&#x200B;**[!UICONTROL 保存]**。

## 设置智能营销活动触发器

1. 在“营销活动”部分，右键单击您创建的程序，然后选择&#x200B;**[!UICONTROL 新建智能营销活动]**。

   ![Smart Campaign 1](assets/smartCampaign1.png)

1. 命名它，然后单击&#x200B;**[!UICONTROL 创建]**。

   ![Smart Campaign 2](assets/smartCampaign3.png)

   在Microsoft文件夹下，您应看到几个可用的触发器。

1. 单击并将&#x200B;**[!UICONTROL 添加到协议]**&#x200B;拖到&#x200B;**[!UICONTROL 智能列表]**&#x200B;中，然后添加您希望在触发器上具有的任何约束。

   ![已添加到协议](assets/addedToAgreementDynamics.png)

## 设置智能营销活动流

1. 单击[!UICONTROL Smart Campaign]中的&#x200B;**[!UICONTROL 流量]**&#x200B;选项卡。

   搜索&#x200B;**调用Webhook**&#x200B;流并将其拖动到画布上，然后选择您在上一节中创建的Webhook。

   ![调用Webhook](assets/callWebhook.png)

1. 您针对已添加到协议中的潜在客户的短信通知营销活动现已设置。
>[!TIP]
>
>本教程是[通过Adobe Sign for Microsoft Dynamics和Marketo](https://experienceleague.adobe.com/?recommended=Sign-U-1-2021.1)加速销售周期课程的一部分，该课程在Experience League上免费提供！