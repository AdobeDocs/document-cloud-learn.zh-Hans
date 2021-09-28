---
title: 使用Adobe Sign for Salesforce和Marketo配置指南发送提醒
description: 了解如何在协议在一段时间后仍未签名时从Marketo发送电子邮件提醒
role: Admin
product: adobe sign
solution: Adobe Sign, Marketo, Document Cloud
level: Intermediate
topic-revisit: Integrations
thumbnail: KT-7248.jpg
exl-id: 33aca2e0-2f27-4100-a16f-85ba652c17a3
source-git-commit: bc79bbde966c99bdf32231ff073b49cfc759d928
workflow-type: tm+mt
source-wordcount: '953'
ht-degree: 1%

---

# 使用Adobe Sign for Salesforce和Marketo配置指南发送提醒

了解如何在一段时间后协议仍未签名时从Marketo发送电子邮件提醒。 此集成使用Adobe Sign、Adobe Sign for Salesforce、Marketo以及Marketo和Salesforce同步。

## 先决条件

1. 安装Marketo Salesforce同步。

   [此处提供了用于Salesforce同步的信息和最新插件。](https://experienceleague.adobe.com/docs/marketo/using/product-docs/crm-sync/salesforce-sync/understanding-the-salesforce-sync.html)

1. 安装Adobe Sign for Salesforce。

   [此处提供了有关此插件的信息。](https://helpx.adobe.com/ca/sign/using/salesforce-integration-installation-guide.html)

## 查找自定义对象

当Marketo Salesforce同步和Adobe Sign for Salesforce配置完成时，Marketo管理终端中会显示多个新选项。

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

**如果** 希望在潜在客户未在Salesforce中签署协议时发送提醒，请为潜在客户下的对象启用同步。

**如果** 希望在联系人未在Salesforce中签署协议时发送提醒，请为“联系人”下的对象启用同步。

**如果** 您希望在帐户未在Salesforce中签署协议时发送提醒，请为帐户下的对象启用同步。

1. **为所** 需的父 **** 代（潜在客户、联系人或帐户）下显示的协议对象启用同步。对要同步的任何其他自定义对象执行此操作。

   ![协议对象](assets/agreementObject.png)

1. 以下资源说明如何&#x200B;**启用同步**。

   ![自定义同步1](assets/customObjectSync1.png)

   ![自定义同步2](assets/customObjectSync2.png)

## 向触发器公开自定义对象字段

1. 在禁用全局同步时，选择您为其启用同步的协议自定义对象，然后选择&#x200B;**编辑可见字段**。

1. 选中触发器列中的“协议名称”字段，将其公开给促销活动操作触发器。 选中要筛选的任何其他字段，然后&#x200B;**保存**。

   ![编辑可见字段1](assets/editVisible1.png)

   ![编辑可见字段2](assets/editVisible2.png)

1. 在对自定义对象启用同步并显示触发器值后，请记住重新激活同步：

   ![启用全局](assets/enableGlobal.png)

## 创建程序和令牌

1. 在Marketo的“营销活动”部分，右键单击左栏的“营销活动&#x200B;**”，选择“新建营销活动文件夹**”，并为其命名。****

   ![新建文件夹](assets/newFolder.png)

1. 右键单击创建的文件夹，选择&#x200B;**新建程序**，并为其指定名称。 将所有其他内容保留为默认值，然后单击&#x200B;**创建**。

   ![新计划1](assets/newProgram1.png)

   ![新计划2](assets/newProgram2.png)

1. 单击&#x200B;**我的令牌**，然后将&#x200B;**电子邮件脚本**&#x200B;拖到画布上。

   ![电子邮件脚本](assets/emailScript.png)

1. 为其命名，然后单击&#x200B;**单击以编辑**。

   ![名称和编辑](assets/nameAndSave.png)

1. 展开右侧的&#x200B;**自定义对象**，然后展开&#x200B;**协议**&#x200B;对象。 查找协议名称、协议状态、签名日期和签名URL并将其拖动到画布上。

1. 使用这些令牌编写一个Velocity脚本，以显示一周内未签名的协议的协议URL。 下面是一个将当前日期与发送日期进行比较的示例：

   ```
   #foreach($agreement in $echosign_dev1__SIGN_Agreement__cList)
       #if($agreement.echosign_dev1__Status__c == "Out for Signature")
           #set($todayCalObj = $date.toCalendar($date.toDate("yyyy-MM-dd",$date.get('yyyy-MM-dd'))) )
           #set($dateSentCalObj = $date.toCalendar($date.toDate("yyyy-MM-dd",$agreement.echosign_dev1__DateSent__c)) )
           #set($dateDiff = ($todayCalObj.getTimeInMillis() - $dateSentCalObj.getTimeInMillis()) / 86400000 )
   
           #if($dateDiff >= 7)
               #set($agreementName = $agreement.Name)
               #set($agreementURL = $agreement.echosign_dev1__Signing_URL__c.substring(8))
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

1. 单击&#x200B;**保存**。

## 创建提醒并添加个性化

个性化示例包括：签名者的姓名、协议的名称、协议链接等。

1. 右键单击您创建的程序，单击&#x200B;**新建本地资源**，然后选择&#x200B;**电子邮件**。

   ![新电子邮件](assets/createNewEmail.png)

1. 在新选项卡中，输入电子邮件的&#x200B;**名称**&#x200B;和&#x200B;**说明**，然后从模板选取器中选择模板。 单击“**创建**”。

   ![模板选取器](assets/templatePicker.png)

1. 设置“**From Name**”和“**From Address**”。

   ![提醒电子邮件](assets/reminderEmail.png)

1. 单击消息正文以激活编辑器。 单击&#x200B;**插入令牌**&#x200B;按钮，找到您创建的自定义协议URL令牌，然后单击&#x200B;**插入**。 完成对电子邮件的自定义，然后单击&#x200B;**保存**。

   ![插入令牌](assets/insertToken.png)

1. 使用已分配协议的配置文件进行预览。 您应看到一个指向URL的链接，其中“协议名称”为标签。

   ![电子邮件链接](assets/emailLink.png)

## 设置智能营销活动过滤器

1. 右键单击您创建的程序，然后单击&#x200B;**新建智能营销活动**。

   ![Smart Campaign 1](assets/smartCampaign1.png)

1. 为其指定您选择的名称，然后单击&#x200B;**创建**。

   ![Smart Campaign 2](assets/smartCampaign2.png)

1. 搜索，然后单击&#x200B;**有协议**&#x200B;并拖动到智能列表。

   ![有协议](assets/hasAgreement.png)

1. 现在，在&#x200B;**添加约束**&#x200B;中，您向触发器公开的字段应可用。 选择&#x200B;**协议状态**&#x200B;以及要筛选的任何其他字段。 对于添加的每个字段，定义要筛选依据的值。 在这种情况下，仅当&#x200B;**协议状态**&#x200B;为“发出进行签名”且&#x200B;**发送日期**&#x200B;在7天前已过时时，才会触发该事件。

   ![协议状态](assets/agreementStatus.png)

   >[!NOTE]
   >
   > d约束的唯一标识符，如&#x200B;**协议名称**，如果您希望此营销活动仅针对某些协议运行。

1. 确认营销活动受众，并在“计划”选项卡中查看哪些人将获得资格。

   ![限定词](assets/qualifiers.png)

## 设置智能营销活动流

由于使用了营销活动过滤器&#x200B;**未签名天数**，因此您可以对营销活动使用计划的重复周期。

1. 单击Smart Campaign中的&#x200B;**流量**&#x200B;选项卡。 搜索&#x200B;**发送电子邮件**&#x200B;流并将其拖动到画布上，然后选择您在上一部分中创建的提醒电子邮件。

   ![发送电子邮件](assets/sendEmail.png)

1. 单击Smart Campaign中的&#x200B;**计划**&#x200B;选项卡。 确保在&#x200B;**智能营销活动设置**&#x200B;中，营销活动流仅限于每人运行一次。 然后，单击&#x200B;**计划重复**&#x200B;选项卡。

   ![“计划”选项卡](assets/scheduleTab.png)

1. 将&#x200B;**计划**&#x200B;设置为“每日”，根据需要选择营销活动的开始日期和时间以及结束日期。

   ![计划设置](assets/scheduleSettings.png)

>[!TIP]
>
>本教程是[通过Adobe Sign for Salesforce和Marketo](https://experienceleague.adobe.com/?recommended=Sign-U-1-2021.1)加速销售周期课程的一部分，该课程在Experience League上免费提供！
