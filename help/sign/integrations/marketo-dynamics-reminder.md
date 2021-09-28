---
title: 使用Adobe Sign for Microsoft Dynamics 365和Marketo发送提醒
description: 了解如何在协议在一段时间后仍未签名时发送电子邮件提醒
role: Admin
product: adobe sign
solution: Adobe Sign, Marketo, Document Cloud
level: Intermediate
topic-revisit: Integrations
thumbnail: KT-7250.jpg
exl-id: 5a97fade-18a3-448a-8504-efb9e38e9187
source-git-commit: bcddb0ee106239f2786debaed908b2a2ec5ce792
workflow-type: tm+mt
source-wordcount: '911'
ht-degree: 2%

---

# 使用Adobe Sign for Microsoft Dynamics 365和Marketo发送提醒

了解如何在协议在一段时间后仍未签名时发送电子邮件提醒。 此集成使用Adobe Sign、Adobe Sign for Microsoft Dynamics、Marketo和Marketo Microsoft Dynamics Sync。

## 先决条件

1. 安装Marketo Microsoft Dynamics同步。

   [此处提供了Microsoft Dynamics Sync的信息和最新插件。](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/microsoft-dynamics/marketo-plugin-releases-for-microsoft-dynamics.html)

1. 安装[Adobe Sign for Microsoft Dynamics](https://appsource.microsoft.com/zh-cn/product/dynamics-365/adobesign.f3b856fc-a427-4d47-ad4b-d5d1baba6f86)。

   [此处提供了有关此插件的信息。](https://helpx.adobe.com/ca/sign/using/microsoft-dynamics-integration-installation-guide.html)

## 查找自定义对象

完成Marketo Microsoft Dynamics Sync和Adobe Sign for Dynamics配置后，Marketo管理终端中会显示两个新选项。

![管理员](assets/adminTerminal.png)

1. 单击&#x200B;**[!UICONTROL 动态实体同步]**。

   同步自定义实体之前必须禁用同步。 如果这是您的首次，请单击“同步架构”**。**&#x200B;否则，单击&#x200B;**刷新架构**。

   ![刷新](assets/refreshSchema.png)

## 同步自定义对象

1. 在右侧，找到[!UICONTROL Lead]、[!UICONTROL Contact]和[!UICONTROL 基于帐户]的自定义对象。

   * **对“** Lead”下的对 **** 象启用“同步”，如果Lead未在Dynamics中  签署协议时要发送提醒。

   * **为“** 联系人”下的对 **** 象启用“同步”，如果您希望在Dynamics中联系  人未签署协议时发送提醒。

   * **为“** 帐户”下的对 **** 象启用“同步”，如果您希望在帐户未在Dynamics  中签署协议时发送提醒。

   * **为** 所需的父级(Lead、Contact **** 、Contact或Account下的协  议对象启用Sync。

   ![自定义对象](assets/enableSyncDynamics.png)

1. 在新窗口中，选择您希望在“协议”下的属性，然后启用&#x200B;**约束**&#x200B;和&#x200B;**触发器**&#x200B;下的框，以将其显示给您的营销活动。

   ![自定义同步1](assets/entitySync1.png)

   ![自定义同步2](assets/entitySync2.png)

1. 在对自定义对象启用同步后重新激活同步。

   返回管理终端，单击&#x200B;**Microsoft Dynamics**，然后单击&#x200B;**启用同步**。

   ![Microsoft Dynamics](assets/microsoftDynamics.png)

   ![启用全局](assets/enableGlobalDynamics.png)

## 创建程序和令牌

1. 在Marketo的“营销活动”部分，右键单击左栏上的“营销活动&#x200B;**”。**

   选择&#x200B;**新建促销活动文件夹**，并为其指定名称。

   ![新建文件夹](assets/newFolder.png)

1. 右键单击创建的文件夹，选择&#x200B;**新建程序**，并为其指定名称。

   将所有其他内容保留为默认值，然后单击&#x200B;**创建**。

   ![新计划1](assets/newProgram1.png)

   ![新计划2](assets/newProgram2.png)

1. 单击&#x200B;**我的令牌**，然后将&#x200B;**电子邮件脚本**&#x200B;拖到画布上。

   ![电子邮件脚本](assets/emailScript.png)

1. 为其命名，然后单击&#x200B;**单击以编辑**。

   ![名称和编辑](assets/nameAndSave.png)

1. 展开右侧的&#x200B;**[!UICONTROL 自定义对象]**，然后展开&#x200B;**[!UICONTROL 协议]**&#x200B;对象。

   查找[!UICONTROL 名称]、协议状态、发送位置和当前签名者URL并将其拖动到画布上。

1. 使用这些令牌编写一个Velocity脚本，以显示一周内未签名的协议的协议URL。 下面是一个将当前日期与发送日期进行比较的示例：

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

个性化示例包括：签名者的姓名、协议的名称、协议链接等。

1. 右键单击您创建的程序，单击&#x200B;**[!UICONTROL 新建本地资源]**，然后选择&#x200B;**[!UICONTROL 电子邮件]**。

   ![新电子邮件](assets/createNewEmail.png)

1. 在新选项卡中，输入电子邮件的&#x200B;**[!UICONTROL 名称]**&#x200B;和&#x200B;**[!UICONTROL 说明]**，然后从模板选取器中选择模板。

   ![模板选取器](assets/templatePicker.png)

1. 单击“**[!UICONTROL 创建]**”。

1. 设置“**[!UICONTROL From Name]**”和“**[!UICONTROL From Address]**”。

   ![提醒电子邮件](assets/reminderEmail.png)

1. 单击消息正文以激活编辑器。

   单击&#x200B;**[!UICONTROL 插入令牌]**&#x200B;按钮，找到您创建的自定义协议URL令牌，然后单击&#x200B;**[!UICONTROL 插入]**。 完成对电子邮件的自定义，然后单击&#x200B;**[!UICONTROL 保存]**。

   ![插入令牌](assets/insertToken.png)

1. 使用已分配协议的配置文件进行预览。

   您应看到一个指向URL的链接，其中“协议名称”为标签。

   ![电子邮件链接](assets/emailLink.png)

## 设置智能营销活动过滤器

1. 右键单击您创建的程序，然后单击&#x200B;**[!UICONTROL 新建智能营销活动]**。

   ![Smart Campaign 1](assets/smartCampaign1.png)

1. 为其指定您选择的名称，然后单击&#x200B;**[!UICONTROL 创建]**。

   ![Smart Campaign 2](assets/smartCampaign2.png)

1. 搜索，然后单击&#x200B;**[!UICONTROL 有协议]**&#x200B;并拖动到智能列表。

   ![有协议](assets/hasAgreementDynamics1.png)

   在&#x200B;**[!UICONTROL 添加约束]**&#x200B;中，应提供向触发器公开的字段。

1. 选择&#x200B;**[!UICONTROL 协议状态]**&#x200B;以及要筛选的任何其他字段。

   对于添加的每个字段，定义要筛选依据的值。 在这种情况下，仅当&#x200B;**[!UICONTROL 协议状态]**&#x200B;为&#x200B;*发出进行签名*&#x200B;且&#x200B;**[!UICONTROL 发送于]**&#x200B;在过去的1周之前为&#x200B;*时，才会触发该事件。*

   ![协议状态](assets/hasAgreementDynaSentOn.png)

   >[!NOTE]
   >
   > 如果希望此营销活动仅针对某些协议运行，请向约束添加唯一标识符，如&#x200B;**名称**。

1. 确认营销活动受众，并在“计划”选项卡中查看哪些人将获得资格。

   ![限定词](assets/qualifiers.png)

## 设置智能营销活动流

由于使用了促销活动过滤器&#x200B;**截止到过期天数**，因此您可以对促销活动使用计划的重复周期。

1. 单击[!UICONTROL Smart Campaign]中的&#x200B;**[!UICONTROL 流量]**&#x200B;选项卡。

   搜索&#x200B;**发送电子邮件**&#x200B;流并将其拖动到画布上，然后选择您在上一部分中创建的提醒电子邮件。

   ![发送电子邮件](assets/sendEmail.png)

1. 单击Smart Campaign中的&#x200B;**[!UICONTROL 计划]**&#x200B;选项卡。 确保在&#x200B;**智能营销活动设置**&#x200B;中，营销活动流仅限于每人运行一次。 然后，单击&#x200B;**计划重复**&#x200B;选项卡。

   ![“计划”选项卡](assets/scheduleTab.png)

1. 将&#x200B;**Schedule**&#x200B;设置为&#x200B;_Daily_。 根据需要，为营销活动选择开始日期、时间和结束日期。

   ![计划设置](assets/scheduleSettings.png)

>[!TIP]
>
>本教程是[通过Adobe Sign for Microsoft Dynamics和Marketo](https://experienceleague.adobe.com/?recommended=Sign-U-1-2021.1)加速销售周期课程的一部分，该课程在Experience League上免费提供！