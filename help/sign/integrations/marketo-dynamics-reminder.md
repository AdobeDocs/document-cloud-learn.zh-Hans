---
title: 使用Acrobat Sign for Microsoft Dynamics 365和Marketo发送提醒
description: 了解当协议在一段时间后仍未签名时，如何发送电子邮件提醒
role: Admin
solution: Acrobat Sign, Marketo, Document Cloud
level: Intermediate
topic: Integrations
topic-revisit: Integrations
jira: KT-7250
thumbnail: KT-7250.jpg
exl-id: 5a97fade-18a3-448a-8504-efb9e38e9187
source-git-commit: ad54f7afa78b0fbb31eccf455723a8890cb92355
workflow-type: tm+mt
source-wordcount: '911'
ht-degree: 2%

---

# 使用Acrobat Sign for Microsoft Dynamics 365和Marketo发送提醒

了解当协议在一段时间后仍未签名时，如何发送电子邮件提醒。 此集成使用Acrobat Sign、Acrobat Sign for Microsoft Dynamics、Marketo和Marketo Microsoft Dynamics Sync。

## 先决条件

1. 安装Marketo Microsoft Dynamics Sync。

   Microsoft Dynamics Sync的信息和最新插件现已可用 [此处。](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/marketo-plugin-releases-for-microsoft-dynamics.html)

1. 安装 [Acrobat Sign for Microsoft Dynamics](https://appsource.microsoft.com/zh-cn/product/dynamics-365/adobesign.f3b856fc-a427-4d47-ad4b-d5d1baba6f86)的

   有关此增效工具的信息 [此处。](https://helpx.adobe.com/ca/sign/using/microsoft-dynamics-integration-installation-guide.html)

## 查找自定义对象

完成Marketo Microsoft Dynamics Sync和Acrobat Sign for Dynamics配置后，Marketo管理终端中会显示两个新选项。

![管理员](assets/adminTerminal.png)

1. 单击 **[!UICONTROL Dynamics实体同步]**&#x200B;的

   在同步自定义实体之前，必须禁用同步。 单击 **同步架构** 如果是第一次了。 否则，请单击 **刷新架构**&#x200B;的

   ![刷新](assets/refreshSchema.png)

## 同步自定义对象

1. 在右侧，找到 [!UICONTROL 潜在客户], [!UICONTROL 联系人]和 [!UICONTROL 帐户]基于的自定义对象。

   * **启用同步** 对于对象的 **[!UICONTROL 潜在客户]** 如果您想在以下情况发送提醒： [!UICONTROL 潜在客户] 尚未在Dynamics中签署协议。

   * **启用同步** 对于对象的 **[!UICONTROL 联系人]** 如果您想在以下情况发送提醒： [!UICONTROL 联系人] 尚未在Dynamics中签署协议。

   * **启用同步** 对于对象的 **[!UICONTROL 帐户]** 如果您希望在发送提醒时 [!UICONTROL 帐户] 尚未在Dynamics中签署协议。

   * **启用同步** 对于所需协议对象的 **[!UICONTROL 父级]** ([!UICONTROL 潜在客户], [!UICONTROL 联系人]或 [!UICONTROL 帐户])。

   ![自定义对象](assets/enableSyncDynamics.png)

1. 在新窗口中，选择“协议”下要使用的属性，然后启用下方的框 **约束** 和 **触发器** ，让他们了解您的营销活动。

   ![Custom Sync 1](assets/entitySync1.png)

   ![自定义同步2](assets/entitySync2.png)

1. 在自定义对象上启用同步后，重新激活同步。

   返回到管理员终端，单击 **Microsoft Dynamics**，然后单击 **启用同步**&#x200B;的

   ![Microsoft Dynamics](assets/microsoftDynamics.png)

   ![启用全局](assets/enableGlobalDynamics.png)

## 创建程序和令牌

1. 在Marketo的“营销活动”部分，右键单击 **营销活动** 图标。

   选择 **新的Campaign文件夹**，并为其命名。

   ![新建文件夹](assets/newFolder.png)

1. 右键单击创建的文件夹，选择 **新计划**，并为其命名。

   将所有其他内容保留为默认值，然后单击 **创建**&#x200B;的

   ![新计划1](assets/newProgram1.png)

   ![新计划2](assets/newProgram2.png)

1. 单击 **我的令牌**，然后拖动 **电子邮件脚本** 到画布上。

   ![电子邮件脚本](assets/emailScript.png)

1. 为其命名，然后单击 **单击以编辑**&#x200B;的

   ![命名和编辑](assets/nameAndSave.png)

1. 扩展 **[!UICONTROL 自定义对象]** ，然后将 **[!UICONTROL 协议]** 对象。

   查找和拖动 [!UICONTROL 名称]、协议状态、发送时间和当前签名者URL。

1. 使用这些令牌编写Velocity脚本，以显示一周内未签名的协议的协议URL。 下面的示例将当前日期与发送日期进行比较：

   ```
   #foreach($agreement in $adobe_agreementList)
       #if($agreement.adobe_esagreementstatus == "Out for Signature")
           #set($todayCalObj = $date.toCalendar($date.toDate("yyyy-MM-dd",$date.get('yyyy-MM-dd'))) )
           #set($dateSentCalObj = $date.toCalendar($date.toDate("yyyy-MM-dd",$agreement.adobe_datesent)) )
           #set($dateDiff = ($todayCalObj.getTimeInMillis() - $dateSentCalObj.getTimeInMillis()) / 86400000 )
   
           #if($dateDiff >= 7)
               #set($agreementName = $agreement.adobe_name)
               #set($agreementURL = $agreement.adobe_currentsignerurl.substring(8))
               #break
           #else
           #end
       #else
       #end
   #end
   
   #if(${agreementName})
       <a href="https://${agreementURL}">${agreementName}</a>
   #else
       Please contact us. 
   #end
   ```

1. 单击&#x200B;**[!UICONTROL 保存]**。

## 创建提醒并添加个性化

个性化示例包括：签名者的姓名、协议名称、协议链接等。

1. 右键单击您创建的程序，然后单击 **[!UICONTROL 新建本地资源]**，然后选择。 **[!UICONTROL 电子邮件]**&#x200B;的

   ![新电子邮件](assets/createNewEmail.png)

1. 在新选项卡中，输入 **[!UICONTROL 名称]** 和 **[!UICONTROL 说明]** ，然后从模板选取器中选择一个模板。

   ![模板选取器](assets/templatePicker.png)

1. 单击&#x200B;**[!UICONTROL 创建]**.

1. 设置 **[!UICONTROL 源名称]** 和 **[!UICONTROL 发件人地址]**&#x200B;的

   ![提醒电子邮件](assets/reminderEmail.png)

1. 单击消息正文以激活编辑器。

   单击 **[!UICONTROL 插入令牌]** 按钮，找到您创建的自定义协议URL令牌，然后单击 **[!UICONTROL 插入]**&#x200B;的 完成自定义电子邮件，然后单击 **[!UICONTROL 保存]**&#x200B;的

   ![插入令牌](assets/insertToken.png)

1. 使用分配了协议的配置文件进行预览。

   您应看到一个指向URL的链接，其中“协议名称”为标签。

   ![电子邮件链接](assets/emailLink.png)

## 设置智能营销活动过滤器

1. 右键单击您创建的程序，然后单击 **[!UICONTROL 新的智能营销活动]**&#x200B;的

   ![Smart Campaign 1](assets/smartCampaign1.png)

1. 为其指定您选择的名称，然后单击 **[!UICONTROL 创建]**&#x200B;的

   ![Smart Campaign 2](assets/smartCampaign2.png)

1. 搜索，然后单击并拖动 **[!UICONTROL 有协议]** 到“智能列表”。

   ![有协议](assets/hasAgreementDynamics1.png)

   您向触发器显示的字段应在中 **[!UICONTROL 添加约束]**&#x200B;的

1. 选择 **[!UICONTROL 协议状态]** 以及要作为筛选依据的任何其他字段。

   对于添加的每个字段，定义筛选依据的值。 在这种情况下，它只在 **[!UICONTROL 协议状态]** 是 *发出进行签名* 和 **[!UICONTROL 发送时间]** 是 *1周前的过去*&#x200B;的

   ![协议状态](assets/hasAgreementDynaSentOn.png)

   >[!NOTE]
   >
   > 向约束添加唯一标识符，例如 **名称**，如果您希望仅针对某些协议运行此营销活动。

1. 确认营销活动受众，并查看谁将有资格访问“计划”选项卡。

   ![限定符](assets/qualifiers.png)

## 设置智能活动流程

因为营销活动筛选器 **距到期日天数** 已使用，您可以为市场活动使用预设的重复项。

1. 单击 **[!UICONTROL 流量]** 选项卡 [!UICONTROL Smart Campaign]的

   搜索并拖动 **发送电子邮件** 排列到画布上，并选择您在上一部分中创建的提醒电子邮件。

   ![发送电子邮件](assets/sendEmail.png)

1. 单击 **[!UICONTROL 计划]** 选项卡。 确保在 **智能营销活动设置**&#x200B;的 然后，单击 **计划重复次数** 选项卡。

   ![“计划”选项卡](assets/scheduleTab.png)

1. 设置 **计划** 来 _每天_&#x200B;的 如有必要，为市场活动选择起始日期和时间以及终止日期。

   ![计划设置](assets/scheduleSettings.png)

>[!TIP]
>
>本教程是课程的一部分 [借助Acrobat Sign for Microsoft Dynamics和Marketo加快销售周期](https://experienceleague.adobe.com/?recommended=Sign-U-1-2021.1) 免费下载Experience League!