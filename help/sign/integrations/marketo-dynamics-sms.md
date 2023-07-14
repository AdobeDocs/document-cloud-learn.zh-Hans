---
title: 使用Acrobat Sign for Microsoft Dynamics 365和Marketo发送通知
description: 了解如何发送文本消息、电子邮件或推送通知，以便让签名者知道协议即将送达
role: Admin
solution: Acrobat Sign, Marketo, Document Cloud
level: Intermediate
topic-revisit: Integrations
jira: KT-7249
thumbnail: KT-7249.jpg
exl-id: 2e0de48c-70bf-4dc5-8251-88e7399f588a
source-git-commit: aa8fd589d214879f2bfcb6bc54576c707532fd6f
workflow-type: tm+mt
source-wordcount: '690'
ht-degree: 1%

---

# 使用Acrobat Sign for Microsoft Dynamics 365和Marketo发送通知

了解如何使用Acrobat Sign、Acrobat Sign for Microsoft Dynamic、Marketo和Marketo Microsoft Dynamics Sync发送文本消息、电子邮件或推送通知，以便让签名者了解即将签署协议。 要从Marketo发送通知，您首先需要购买或配置Marketo SMS管理功能。 此演练使用 [Twilio SMS](https://launchpoint.marketo.com/twilio/twilio-sms-for-marketo/)，但是其他Marketo SMS解决方案可用。

## 先决条件

1. 安装Marketo Microsoft Dynamics Sync。

   Microsoft Dynamics Sync的信息和最新插件现已可用 [此处。](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/marketo-plugin-releases-for-microsoft-dynamics.html)

1. 安装Acrobat Sign for Microsoft Dynamics。

   有关此增效工具的信息 [此处。](https://helpx.adobe.com/ca/sign/using/microsoft-dynamics-integration-installation-guide.html)

## 查找自定义对象

完成Marketo Microsoft Dynamics Sync和Acrobat Sign for Dynamics配置后，Marketo管理终端中会显示两个新选项。

![管理员](assets/adminTerminal.png)

* 单击 **[!UICONTROL Dynamics实体同步]**&#x200B;的

  在同步自定义实体之前，必须禁用同步。 单击 **[!UICONTROL 同步架构]** 如果是第一次了。 否则，请单击 **[!UICONTROL 刷新架构]**&#x200B;的

  ![刷新](assets/refreshSchema.png)

## 同步自定义对象

1. 在右侧，找到 [!UICONTROL 潜在客户], [!UICONTROL 联系人]和 [!UICONTROL 帐户]基于的自定义对象。

   * **[!UICONTROL 启用同步]** 对于“潜在客户”下的对象（如果您想要在Dynamics中将潜在客户添加到协议时触发）。

   * **[!UICONTROL 启用同步]** 对象（如果您希望在Dynamics中将联系人添加到协议时触发）。

   * **[!UICONTROL 启用同步]** 在Dynamics中将帐户添加到协议时触发“帐户”下的对象。

   * **启用同步** 对于所需父级（潜在客户、联系人或帐户）下的协议对象。

   ![自定义对象](assets/enableSyncDynamics.png)

1. 在新窗口中，选择协议下所需的属性。

   启用下面的 **[!UICONTROL 约束]** 和 **[!UICONTROL 触发器]** ，让他们了解您的营销活动。

   ![Custom Sync 1](assets/entitySync1.png)

   ![自定义同步2](assets/entitySync2.png)

1. 在自定义对象上启用同步后，重新激活同步。

   返回 [!UICONTROL 管理员终端]，然后单击。 **[!UICONTROL Microsoft Dynamics]**，然后单击 **[!UICONTROL 启用同步]**&#x200B;的

   ![Microsoft Dynamics](assets/microsoftDynamics.png)

   ![启用全局](assets/enableGlobalDynamics.png)

## 创建程序

1. 在 [!UICONTROL 营销活动]，右键单击 **[!UICONTROL 营销活动]** 在左侧栏中，选择 **[!UICONTROL 新的Campaign文件夹]**，并为其命名。

   ![新建文件夹](assets/newFolder.png)

1. 右键单击创建的文件夹，选择 **[!UICONTROL 新计划]**，并为其命名。

   将所有其他内容保留为默认值，然后单击 **[!UICONTROL 创建]**&#x200B;的

   ![新计划1](assets/newProgram1.png)

   ![新计划2](assets/newProgram2.png)

## 设置 [!DNL Twilio] 短信

首先确保您拥有一个 [!DNL Twilio] 购买了所需的短信功能。

设置Marketo - [!DNL Twilio] SMS Webhook需要三个 [!DNL Twilio] 来自您帐户的参数。

* 帐户SID
* 帐户令牌
* Twilio电话号码

从您的帐户检索这些参数，现在打开您的Marketo实例。

1. 单击 **[!UICONTROL 管理员]** 右上角。

   ![管理员](assets/adminTab.png)

1. 单击 **[!UICONTROL Webhook]**，然后单击。 **[!UICONTROL 新建Webhook]**&#x200B;的

   ![Webhook](assets/webhooks.png)

1. 输入 **[!UICONTROL Webhook名称]** 和 **[!UICONTROL 说明]**&#x200B;的

1. 输入以下URL，并确保替换 `ACCOUNT_SID` 和 `AUTH_TOKEN` 的 [!DNL Twilio] 凭据。

   ```
   https://[ACCOUNT_SID]:[AUTH_TOKEN]@API.TWILIO.COM/2010-04-01/ACCOUNTS/[ACCOUNT_SID]/Messages.json
   ```

1. 选择 **[!UICONTROL POST]** 作为请求类型。

1. 输入以下内容 **模板** 一定要替换 `MY_TWILIO_NUMBER` 的 [!DNL Twilio] 电话号码和 `YOUR_MESSAGE` 并附上您选择的消息。

   ```
   From=%2B1[MY_TWILIO_NUMBER]&To=%2B1{{lead.Mobile Phone Number:default=edit me}}&Body=[YOUR_MESSAGE]
   ```

1. 设置 **[!UICONTROL 请求令牌编码]** 来 *表单/URL*&#x200B;的

1. 将“响应类型”设置为 *JSON* 然后单击 **[!UICONTROL 保存]**&#x200B;的

## 设置智能营销活动触发器

1. 在“市场营销活动”部分，右键单击您创建的计划，然后选择 **[!UICONTROL 新的智能营销活动]**&#x200B;的

   ![Smart Campaign 1](assets/smartCampaign1.png)

1. 为其命名，然后单击 **[!UICONTROL 创建]**&#x200B;的

   ![Smart Campaign 2](assets/smartCampaign3.png)

   您应该可以在Microsoft文件夹下看到多个可用的触发器。

1. 单击并拖动 **[!UICONTROL 添加到协议]** 到 **[!UICONTROL 智能列表]**，然后添加您希望对触发器具有的任何约束。

   ![添加到协议](assets/addedToAgreementDynamics.png)

## 设置智能活动流程

1. 单击 **[!UICONTROL 流量]** 选项卡 [!UICONTROL Smart Campaign]的

   搜索并拖动 **调用Webhook** 排列到画布上，并选择您在上一部分中创建的Webhook。

   ![调用Webhook](assets/callWebhook.png)

1. 现已设置您针对添加到协议中的潜在客户的短信通知营销活动。
>[!TIP]
>
>本教程是课程的一部分 [借助Acrobat Sign for Microsoft Dynamics和Marketo加快销售周期](https://experienceleague.adobe.com/?recommended=Sign-U-1-2021.1) 免费下载Experience League!