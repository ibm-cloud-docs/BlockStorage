---

copyright:
  years: 2026
lastupdated: "2026-02-06"

keywords: block storage for classic, missing permission

subcollection: BlockStorage

---

{{site.data.keyword.attribute-definition-list}}

# Why do I get an insufficient permissions error when I try to update my volume?
{: #ts-insufficient-permissions}

As you try to increase your volume's capacity or update your volume's IOPS, you receive a 500 error that states `PermissionDenied: You do not have permission to perform upgrades.` You successfully completed such operations previously and you are certain your permissions were not changed.
{: tsSymptoms}

This error might have occurred because you do not have `viewer` IAM role for Billing. As of 21 January 2026, any user who wants to complete operations that impact billing must also have permissions to view account and billing information. This change aims to help users be fully aware of the impact of their upgrades, and to prevent any disputes about proration and billing.
{: tsCauses}

To resolve, work with your account administrator to confirm that you have the right permissions, and correct them if needed. For more information, see [Managing migrated SoftLayer account permissions](/docs/account?topic=account-migrated_permissions). 
{: tsResolve}
