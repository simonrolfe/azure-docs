---
title: Assigning administrator roles in Azure Active Directory | Microsoft Docs
description: An admin role can add users, assign administrative roles, reset user passwords, manage user licenses, or manage domains. A user who is assigned an admin role has the same permissions across all cloud services to which your organization has subscribed.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''

ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 06/07/2018
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro

---
# Assigning administrator roles in Azure Active Directory

Using Azure Active Directory (Azure AD), you can designate separate administrators to serve different functions. Administrators have access to various features in the Azure portal and, depending on their role, can create or edit users, assign administrative roles to others, reset user passwords, manage user licenses, and manage domains, among other things. A user who is assigned an admin role will have the same permissions across all of the cloud services to which your organization has subscribed to, regardless of whether you assign the role in the Office 365 portal, or in the Azure portal, or by using the Azure AD module for Windows PowerShell.

## Details about the Global Administrator role
The Global Administrator has access to all administrative features. By default, the person who signs up for an Azure subscription is assigned the global administrator role for the directory. Only global administrators can assign other administrator roles.

## Assign or remove administrator roles
To learn how to assign administrative roles to a user in Azure Active Directory, see [Assign a user to administrator roles in Azure Active Directory](fundamentals/active-directory-users-assign-role-azure-portal.md).

## Available roles
The following administrator roles are available:

* **Application Administrator**: Users in this role can create and manage all aspects of enterprise applications, application registrations, and application proxy settings. This role also grants the ability to consent to delegated permissions, and application permissions excluding Microsoft Graph and Azure AD Graph. Members of this role are not added as owners when creating new application registrations or enterprise applications.

* **Application Developer**: Users in this role can create application registrations when the “Users can register applications” setting is set to No. This role also allows members to consent on their own behalf when the “Users can consent to apps accessing company data on their behalf” setting is set to No. Members of this role are added as owners when creating new application registrations or enterprise applications.

* **Billing Administrator**: Makes purchases, manages subscriptions, manages support tickets, and monitors service health.

* **Cloud Application Administrator**: Users in this role have the same permissions as the Application Administrator role, excluding the ability to manage application proxy. This role grants the ability to create and manage all aspects of enterprise applications and application registrations. This role also grants the ability to consent to delegated permissions, and application permissions excluding Microsoft Graph and Azure AD Graph. Members of this role are not added as owners when creating new application registrations or enterprise applications.

* **Compliance Administrator**: Users with this role have management permissions within in the Office 365 Security & Compliance Center and Exchange Admin Center. More information at “[About Office 365 admin roles](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).”

* **Conditional Access Administrator**: Users with this role have the ability to manage Azure Active Directory conditional access settings.
  > [!NOTE]
  > To deploy Exchange ActiveSync conditional access policy in Azure, the user must also be Global Administrator.
  
* **Device Administrators**:  This role is available for assignment only as an additional local administrator in [Device settings](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/DevicesMenuBlade/DeviceSettings/menuId/). Users with this role become local machine administrators on all Windows 10 devices that are joined to Azure Active Directory. They do not have the ability to manage devices objects in Azure Active Directory.

* **Directory Readers**: This is a legacy role that is to be assigned to applications that do not support the [Consent Framework](active-directory-integrating-applications.md). It should not be assigned to any users.

* **Directory Synchronization Accounts**: Do not use. This role is automatically assigned to the Azure AD Connect service, and is not intended or supported for any other use.

* **Directory Writers**: This is a legacy role that is to be assigned to applications that do not support the [Consent Framework](active-directory-integrating-applications.md). It should not be assigned to any users.

* **Dynamics 365 Administrator**: Users with this role have global permissions within Microsoft Dynamics 365, when the service is present, as well as the ability to manage support tickets and monitor service health. More information at [About Office 365 admin roles](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **Exchange Service Administrator**: Users with this role have global permissions within Microsoft Exchange Online, when the service is present. More information at [About Office 365 admin roles](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **Global Administrator / Company Administrator / Tenant Administrator**: Users with this role have access to all administrative features in Azure Active Directory, as well as services that federate to Azure Active Directory like Exchange Online, SharePoint Online, and Skype for Business Online. The person who signs up for the Azure Active Directory tenant becomes a global administrator. Only global administrators can assign other administrator roles. There can be more than one global administrator at your company. Global admins can reset the password for any user and all other administrators.

  > [!NOTE]
  > In Microsoft Graph API, Azure AD Graph API, and Azure AD PowerShell, this role is identified as "Company Administrator". It is "Global Administrator" in the [Azure portal](https://portal.azure.com).
  >
  >

* **Guest Inviter**: Users in this role can manage Azure Active Directory B2B guest user invitations when the “Members can invite” user setting is set to No. More information about B2B collaboration at [About Azure AD B2B collaboration](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b). It does not include any other permissions.

* **Information Protection Administrator**: Users with this role have user rights only on the Azure Information Protection service. They are not granted user rights on Identity Protection Center, Privileged Identity Management, Monitor Office 365 Service Health, or Office 365 Security & Compliance Center. They can configure labels for the Azure Information Protection policy, manage protection templates, and activate protection.

* **Intune Service Administrator**: Users with this role have global permissions within Microsoft Intune Online, when the service is present. Additionally, this role contains the ability to manage users and devices in order to associate policy, as well as create and manage groups.

* **Mailbox Administrator**: This role is only used as part of Exchange Online email support for RIM Blackberry devices. If your organization does not use Exchange Online email on RIM Blackberry devices, do not use this role.

* **Message Center Reader**: Users in this role can monitor notifications and advisory health updates in [Office 365 Message center](https://support.office.com/article/Message-center-in-Office-365-38FB3333-BFCC-4340-A37B-DEDA509C2093) for their organization on configured services such as Exchange, Intune and Microsoft Teams. Message Center Readers receive weekly email digests of posts, updates, and can share message center posts in Office 365. In Azure AD, users assigned to this role will only have read-only access on Azure AD services such as users and groups. 

* **Partner Tier 1 Support**: Do not use. This role has been deprecated and will be removed from Azure AD in the future. This role is intended for use by a small number of Microsoft resale partners, and is not intended for general use.

* **Partner Tier 2 Support**: Do not use. This role has been deprecated and will be removed from Azure AD in the future. This role is intended for use by a small number of Microsoft resale partners, and is not intended for general use.

* **Password Administrator / Helpdesk Administrator**: Users with this role can change passwords, manage service requests, and monitor service health. Helpdesk administrators can change passwords only for users and other Helpdesk administrators. 

  > [!NOTE]
  > In Microsoft Graph API, Azure AD Graph API and Azure AD PowerShell, this role is identified as "Helpdesk Administrator". It is "Password Administrator" in the [Azure portal](https://portal.azure.com/).
  >
  >
  
* **Power BI Service Administrator**: Users with this role have global permissions within Microsoft Power BI, when the service is present, as well as the ability to manage support tickets and monitor service health. More information at [About Office 365 admin roles](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d?ui=en-US&rs=en-001&ad=US).

* **Privileged Role Administrator**: Users with this role can manage role assignments in Azure Active Directory, as well as within Azure AD Privileged Identity Management. In addition, this role allows management of all aspects of Privileged Identity Management.

* **Reports Reader**: Users with this role can view usage reporting data and the reports dashboard in Office 365 admin center and the adoption context pack in PowerBI. Additionally, the role provides access to sign-on reports and activity in Azure AD and data returned by the Microsoft Graph reporting API. A user assigned to the Reports Reader role can access only relevant usage and adoption metrics. They don't have any admin permissions to configure settings or access the product specific admin centers like Exchange. 

* **Security Administrator**: Users with this role have all of the read-only permissions of the Security reader role, plus the ability to manage configuration for security-related services: Azure Active Directory Identity Protection, Azure Information Protection, Privileged Identity Management, and Office 365 Security & Compliance Center. More information about Office 365 permissions is available at [Permissions in the Office 365 Security & Compliance Center](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).

* **Security Reader**: Users with this role have global read-only access, including all information in Azure Active Directory, Identity Protection, Privileged Identity Management, as well as the ability to read Azure Active Directory sign-in reports and audit logs. The role also grants read-only permission in Office 365 Security & Compliance Center. More information about Office 365 permissions is available at [Permissions in the Office 365 Security & Compliance Center](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1).

* **Service Support Administrator**: Users with this role can open support requests with Microsoft for Azure and Office 365 services, and views the service dashboard and message center in the Azure portal and Office 365 admin portal. More information at [About Office 365 admin roles](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **SharePoint Service Administrator**: Users with this role have global permissions within Microsoft SharePoint Online, when the service is present, as well as the ability to manage support tickets and monitor service health. More information at [About Office 365 admin roles](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

* **Skype for Business / Lync Service Administrator**: Users with this role have global permissions within Microsoft Skype for Business, when the service is present, as well as manage Skype-specific user attributes in Azure Active Directory. Additionally, this role grants the ability to manage support tickets and monitor service health. More information at [About Office 365 admin roles](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d).

  > [!NOTE]
  > In Microsoft Graph API, Azure AD Graph API and Azure AD PowerShell, this role is identified as "Lync Service Administrator". It is "Skype for Business Service Administrator" in the [Azure portal](https://portal.azure.com/).
  >
  >

* **User Account Administrator**: Users with this role can create and manage all aspects of users and groups. Additionally, this role includes the ability to manage support tickets and monitors service health. Some restrictions apply. For example, this role does not allow deleting a global administrator. User Account administrators can change passwords for users, Helpdesk administrators, and other User Account administrators only.

## Administrator permissions

### Application Administrator

| Can do | Cannot do |
| --- | --- |
| Read all directory information<br>Create application registrations<br>Update application registration properties<br>Acquire enterprise applications<br>Manage application registration permissions<br>Delete application registrations<br>Manage enterprise application single sign-on settings<br>Manage enterprise application provisioning settings<br>Manage enterprise application self-service settings<br>Manage enterprise application permission settings<br>Manage application access<br>Manage provisioning settings<br>Delete enterprise applications<br>Consent on behalf of everyone for all delegated permission requests<br>Consent on behalf of everyone for all application permission requests except Azure AD Graph or Microsoft Graph<br>Manage application proxy settings<br>Access services settings<br>Monitor service health<br>Manage support tickets<br>Read hidden group membership | Create, edit, and delete groups<br>Manage user licenses<br>Use directory synchronization<br>View sign-in reports and audit logs | 

### Application Developer

| Can do | Cannot do |
| --- | --- |
| Read all directory information<br>Create application registrations<br>Consent on behalf of self | View sign-in and audit logs<br>Read hidden group membership |

### Billing Administrator

| Can do | Cannot do |
| --- | --- |
|<p>View company and user information</p><p>Manage Office support tickets</p><p>Perform billing and purchasing operations for Office products</p> |<p>Reset user passwords</p><p>Create and manage user views</p><p>Create, edit, and delete users and groups, and manage user licenses</p><p>Manage domains</p><p>Manage company information</p><p>Delegate administrative roles to others</p><p>Use directory synchronization</p><p>View audit logs</p> |

### Cloud Application Administrator

| Can do | Cannot do |
| --- | --- |
| Read all directory information<br>Create application registrations<br>Update application registration properties<br>Acquire enterprise applications<br>Manage application registration permissions<br>Delete application registrations<br>Manage enterprise application single sign-on settings<br>Manage enterprise application provisioning settings<br>Manage enterprise application self-service settings<br>Manage enterprise application permission settings<br>Manage application access<br>Manage provisioning settings<br>Delete enterprise applications<br>Consent on behalf of everyone for all delegated permission requests<br>Consent on behalf of everyone for all application permission requests except Azure AD Graph or Microsoft Graph<br>Access services settings<br>Monitor service health<br>Manage support tickets<br>Read hidden group membership | Manage application proxy settings<br>Create, edit, and delete groups<br>Manage user licenses<br>Use directory synchronization<br>View sign-in reports and audit logs |

### Conditional Access Administrator

| Can do | Cannot do |
| --- | --- |
|<p>View company and user information</p><p>Manage conditional access settings</p> |<p>Reset user passwords</p><p>Create and manage user views</p><p>Create, edit, and delete users and groups, and manage user licenses</p><p>Manage domains</p><p>Manage company information</p><p>Delegate administrative roles to others</p><p>Use directory synchronization</p><p>View audit logs</p>|

### Global Administrator
| Can do | Cannot do |
| --- | --- |
|<p>View company and user information</p><p>Manage Office support tickets</p><p>Perform billing and purchasing operations for Office products</p><p>Reset user passwords</p><p>Reset other administrators' passwords</p> <p>Create and manage user views</p><p>Create, edit, and delete users and groups, and manage user licenses</p><p>Manage domains</p><p>Manage company information</p><p>Delegate administrative roles to others</p><p>Use directory synchronization</p><p>Enable or disable multi-factor authentication</p><p>View audit logs</p> |N/A |

### Password Administrator / Helpdesk Administrator
| Can do | Cannot do |
| --- | --- |
| <p>View company and user information</p><p>Manage Office support tickets</p><p>Change passwords for users and other Helpdesk administrators only</p>|<p>Perform billing and purchasing operations for Office products</p><p>Create and manage user views</p><p>Create, edit, and delete users and groups, and manage user licenses</p><p>Manage domains</p><p>Manage company information</p><p>Delegate administrative roles to others</p><p>Use directory synchronization</p><p>View reports</p>|

### Information Protection Administrator
In | Can do
-------- | ---------
Azure Information Protection | <li>Configure labels and settings in global and scoped policies<li>Configure and manage protection templates<li>Activate or deactivate protection--
 
### Reports Reader 
Can do | Cannot do
------ | ----------
View Azure AD sign-in Reports and audit logs<br>View company and user information<br>Access Office 365 usage dashboard | Create and manage user views<br>Create, edit, and delete users and groups, and manage user licenses<br>Delegate administrative roles to others<br>Manage company information

### Security Reader
| In | Can do |
| --- | --- |
| Identity Protection Center |Read all security reports and settings information for security features<ul><li>Anti-spam<li>Encryption<li>Data loss prevention<li>Anti-malware<li>Advanced threat protection<li>Anti-phishing<li>Mailflow rules |
| Privileged Identity Management |<p>Has read-only access to all information surfaced in Azure AD PIM: Policies and reports for Azure AD role assignments, security reviews and in the future read access to policy data and reports for scenarios besides Azure AD role assignment.<p>**Cannot** sign up for Azure AD PIM or make any changes to it. In PIM's portal or via PowerShell, someone in this role can activate additional roles (for example, Global Admin or Privileged Role Administrator), if the user is a candidate for them. |
| <p>Monitor Office 365 Service Health</p><p>Office 365 Security & Compliance Center</p> |<ul><li>Read and manage alerts<li>Read security policies<li>Read threat intelligence, Cloud App Discovery, and Quarantine in Search and Investigate<li>Read all reports |

### Security Administrator
| In | Can do |
| --- | --- |
| Identity Protection Center |<ul><li>All permissions of the Security Reader role.<li>Additionally, the ability to perform all IPC operations except for resetting passwords. |
| Privileged Identity Management |<ul><li>All permissions of the Security Reader role.<li>**Cannot** manage Azure AD role memberships or settings. |
| <p>Monitor Office 365 Service Health</p><p>Office 365 Security & Compliance Center |<ul><li>All permissions of the Security Reader role.<li>Can configure all settings in the Advanced Threat Protection feature (malware & virus protection, malicious URL config, URL tracing, etc.). |

### Service Administrator
| Can do | Cannot do |
| --- | --- |
| <p>View company and user information</p><p>Manage Office support tickets</p> |<p>Reset user passwords</p><p>Perform billing and purchasing operations for Office products</p><p>Create and manage user views</p><p>Create, edit, and delete users and groups, and manage user licenses</p><p>Manage domains</p><p>Manage company information</p><p>Delegate administrative roles to others</p><p>Use directory synchronization</p><p>View audit logs</p> |

### User Account Administrator
| Can do | Cannot do |
| --- | --- |
| <p>View company and user information</p><p>Manage Office support tickets</p><p>Change passwords for users, Helpdesk administrators, and other User Account administrators only</p><p>Create and manage user views</p><p>Create, edit, and delete users and groups, and manage user licenses, with limitations. He or she cannot delete a global administrator or create other administrators.</p> |<p>Perform billing and purchasing operations for Office products</p><p>Manage domains</p><p>Manage company information</p><p>Delegate administrative roles to others</p><p>Use directory synchronization</p><p>Enable or disable multi-factor authentication</p><p>View audit logs</p> |

### To add a user as a global administrator

1. Sign in to the [Azure Active Directory Admin Center](https://aad.portal.azure.com) with an account that's a Global Administrator for the tenant directory.

   ![Opening azure AD admin center](./media/active-directory-assign-admin-roles-azure-portal/active-directory-admin-center.png)

2. Select **Users** > **All users**.

3. Open the page for the user you want to designate as a Global Administrator.

4. On the command bar, select **Directory role**.

5. Select **Add role**.

6. On the directory role page, select the **Global Administrator** role, and then click **Select** to save.

## Deprecated roles

The following roles should not be used. They are deprecated and will be removed from Azure AD in the future.

* AdHoc License Administrator
* Email Verified User Creator
* Device Join
* Device Managers
* Device Users
* Workplace Device Join

## Next steps

* To learn more about how to change administrators for an Azure subscription, see [Add or change Azure subscription administrators](../billing-add-change-azure-subscription-administrator.md)
* To learn more about how resource access is controlled in Microsoft Azure, see [Understanding resource access in Azure](../role-based-access-control/rbac-and-directory-admin-roles.md)
* For more information on how Azure Active Directory relates to your Azure subscription, see [How Azure subscriptions are associated with Azure Active Directory](fundamentals/active-directory-how-subscriptions-associated-directory.md)
* [Manage users](active-directory-create-users.md)
* [Manage passwords](active-directory-manage-passwords.md)
* [Manage groups](fundamentals/active-directory-manage-groups.md)
