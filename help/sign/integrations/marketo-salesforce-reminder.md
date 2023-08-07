---
title: 使用适用于Salesforce的Acrobat Sign和Marketo配置指南发送提醒
description: 了解当协议在一段时间后仍未签名时，如何从Marketo发送电子邮件提醒
feature: Integrations
role: Admin
solution: Acrobat Sign, Marketo, Document Cloud
level: Intermediate
topic: Integrations
jira: KT-7248
topic-revisit: Integrations
thumbnail: KT-7248.jpg
exl-id: 33aca2e0-2f27-4100-a16f-85ba652c17a3
source-git-commit: 452299b2b786beab9df7a5019da4f3840d9cdec9
workflow-type: tm+mt
source-wordcount: '953'
ht-degree: 1%

---

# 使用适用于Salesforce的Acrobat Sign和Marketo配置指南发送提醒

了解当协议在一段时间后仍未签名时，如何从Marketo发送电子邮件提醒。 此集成使用Acrobat Sign、适用于Salesforce的Acrobat Sign、Marketo以及Marketo和Salesforce Sync。

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

**启用同步** 如果您希望在潜在客户尚未在Salesforce中签署协议时发送提醒，请为潜在客户下的对象执行此操作。

**启用同步** 如果要在联系人尚未在Salesforce中签署协议时发送提醒，请为联系人下的对象执行此操作。

**启用同步** 如果要在帐户尚未在Salesforce中签署协议时发送提醒，请为帐户下的对象执行此操作。

1. **启用同步** 用于 **协议** 在所需父项（潜在客户、联系人或帐户）下显示的对象。 对要同步的任何其他自定义对象执行此操作。

   ![协议对象](assets/agreementObject.png)

1. 以下资源演示了如何 **启用同步**.

   ![自定义同步1](assets/customObjectSync1.png)

   ![自定义同步2](assets/customObjectSync2.png)

## 向触发器公开自定义对象字段

1. 在停用全局同步时，选择您为其启用同步的协议自定义对象，然后 **编辑可见字段**.

1. 选中触发器列中的“协议名称”字段，使其向您的市场活动操作触发器显示。 选中要作为筛选依据的任何其他字段，然后 **保存**.

   ![编辑可见字段1](assets/editVisible1.png)

   ![编辑可见字段2](assets/editVisible2.png)

1. 在自定义对象上启用同步并显示触发器值后，请记住重新激活同步：

   ![启用全局](assets/enableGlobal.png)

## 创建程序和令牌

1. 在Marketo的Marketing Activities部分，右键单击 **营销活动** 在左栏中，选择 **新市场活动文件夹**，并为其命名。

   ![新建文件夹](assets/newFolder.png)

1. 右键单击已创建的文件夹，选择 **新计划**，并为其命名。 将其余所有内容保留为默认值，然后单击 **创建**.

   ![新计划1](assets/newProgram1.png)

   ![新计划2](assets/newProgram2.png)

1. 单击 **我的令牌**，然后拖动  **电子邮件脚本** 到画布上。

   ![电子邮件脚本](assets/emailScript.png)

1. 为其命名，然后单击 **单击以编辑**.

   ![命名和编辑](assets/nameAndSave.png)

1. 展开 **自定义对象** 在右侧，展开 **协议** 对象。 查找协议名称、协议状态、签名日期和签名URL并将其拖动到画布上。

1. 使用这些令牌编写Velocity脚本以显示为期一周未签名的协议的协议URL。 以下示例将当前日期与“发送日期”进行比较：

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

## 创建提醒并添加个性化设置

个性化设置示例包括：签名者的姓名、协议名称、协议链接等。

1. 右键单击您创建的程序，然后单击 **新建本地资源**，然后选择 **电子邮件**.

   ![新电子邮件](assets/createNewEmail.png)

1. 在新选项卡中，输入 **名称** 和 **描述** ，然后从模板选择器中选择一个模板。 单击&#x200B;**创建**.

   ![模板选择器](assets/templatePicker.png)

1. 设置 **发件人姓名** 和 **发件人地址**.

   ![提醒电子邮件](assets/reminderEmail.png)

1. 单击消息正文以激活编辑器。 单击 **插入令牌** 按钮，查找您创建的自定义协议URL令牌，然后单击 **插入**. 完成自定义电子邮件，然后单击 **保存**.

   ![插入令牌](assets/insertToken.png)

1. 使用分配了协议的配置文件进行预览。 您应该会看到一个指向以协议名称作为标签的URL的链接。

   ![电子邮件链接](assets/emailLink.png)

## 设置Smart Campaign筛选器

1. 右键单击您创建的程序，然后单击 **新的Smart Campaign**.

   ![Smart Campaign 1](assets/smartCampaign1.png)

1. 为您选择的名称命名，然后单击 **创建**.

   ![Smart Campaign 2](assets/smartCampaign2.png)

1. 搜索，然后单击并拖动 **有协议** 添加到智能列表。

   ![有协议](assets/hasAgreement.png)

1. 现在应可在以下位置找到您向触发器显示的字段 **添加约束**. 选择 **协议状态** 以及您希望作为筛选依据的任何其他字段。 对于添加的每个字段，定义作为筛选依据的值。 在这种情况下，仅当 **协议状态** 已发出进行签名，且 **发送日期** 在7天前已过。

   ![协议状态](assets/agreementStatus.png)

   >[!NOTE]
   >
   > 为约束设置唯一标识符，如 **协议名称**，如果您希望此营销活动仅针对某些协议运行。

1. 确认市场活动受众，并在“计划”选项卡中查看符合条件的受众。

   ![限定词](assets/qualifiers.png)

## 设置智能营销活动流

因为市场活动过滤器 **未签名天数** 已使用，您可以对市场活动使用计划的重复。

1. 单击 **流量** 选项卡。 搜索并拖动 **发送电子邮件** 排到画布上，然后选择您在上一部分中创建的提醒电子邮件。

   ![发送电子邮件](assets/sendEmail.png)

1. 单击 **计划** 选项卡。 确保市场活动流仅限在 **Smart Campaign设置**. 然后，单击 **计划重复周期** 选项卡。

   ![“计划”选项卡](assets/scheduleTab.png)

1. 设置 **计划** 对于每日，根据需要选择市场活动的起始日期和时间以及终止日期。

   ![计划设置](assets/scheduleSettings.png)

>[!TIP]
>
>本教程是本课程的一部分 [使用适用于Salesforce的Acrobat Sign和Marketo加快销售周期](https://experienceleague.adobe.com/?recommended=Sign-U-1-2021.1) 可在Experience League上免费获得！
