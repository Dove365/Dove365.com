# Deployment Guide

## Pre-Deployment Checklist

- Confirm target environment is Dataverse-enabled.
- Prefer a Dynamics 365-enabled environment to support Outlook for Dynamics 365.
- Confirm Power Apps Component Framework is enabled.
- Confirm Power Apps Code Apps is enabled.
- Confirm Dataverse Intelligence is enabled.
- Confirm Dataverse Model Context Protocol is enabled.
- Confirm target users and service account are available.
- Confirm SharePoint site is available for document management.
- Confirm mailboxes can be approved and tested.
- Confirm managed solution packages are available for both solutions.

## Platform Settings

Before production import, configure or plan:

- Server-side sync for incoming email, outgoing email, and appointments.
- Dataverse Intelligence enabled.
- Dataverse Model Context Protocol enabled.
- Mailboxes approved and enabled.
- Email tracking enabled.
- Configure server-based SharePoint Integration.
- Document Management Settings using the SharePoint site.
- Contacts selected in Document Management Settings to enable contact files in CRM.

## Import Order

1. Import Dove365 Common.
2. Import Dove365 CRM Starter.

Do not import CRM Starter before Common.

## Environment Variables

During or after Common import, set:

- `Flow Failure Notification Account`: recipient mailbox/account for failed flow notifications.
- `Service Account Email`: sender/service account used for notification emails, reminders, and system-generated communications.

## Connection References

Common includes two connection references that must both be signed in with the dedicated service account during or immediately after import.

| Logical name | Display name | Connector |
|---|---|---|
| `dove365_OutlookConnectServiceAccount` | Outlook Connect (Service Account) | Office 365 Outlook |
| `dove365_DataverseConnectionServiceAccount` | Dataverse Connection (Service Account) | Microsoft Dataverse |

Both connections must be authenticated before turning on the Power Automate flows in CRM Starter. Leaving either connection unauthenticated will prevent those flows from running.

## Post-Import Validation

Validate:

- App `Dove365 CRM Starter` opens.
- Sitemap shows Dashboard, Relationships, Pipeline, and Service.
- Roles `Dove365 CRM User` and `Dove365 CRM Admin` exist.
- Lead, Opportunity, and Case BPFs show on relevant records.
- LinkedIn Badge PCF renders on contact form where LinkedIn URL is populated.
- Flow `Notify when flows fail` is present and has a valid Outlook connection.
- Flow `Opportunity - Close` is present and has a valid Dataverse connection.
- Flow `Case - Resolve` (Power Automate) is present and has a valid Dataverse connection.
- Flow `Lead - Convert` (Power Automate) is present and has a valid Dataverse connection.
- Dataverse Intelligence remains enabled in the target environment.
- Dataverse Model Context Protocol remains enabled in the target environment.
- SharePoint Documents grid is visible for contacts once document management is configured.
- Email tracking and timeline activities work for enabled users.

## Smoke Test Scenarios

- Create account and contact; link contact to account.
- Create lead, progress through New, Research, Engage, Qualify, and Convert/Close.
- Convert a lead to contact and check converted-from-lead reference.
- Create opportunity, link account/contact, add service offering, progress BPF, close won/lost.
- Create case, link account/contact/service offering, escalate, resolve, or cancel.
- Upload/view contact document through SharePoint integration.
- Send and track an Outlook email to a CRM contact.

## Rollback Considerations

- Managed solution rollback should follow Microsoft solution upgrade/uninstall guidance.
- Do not delete Common while CRM Starter is installed.
- If an import fails, review dependency errors, then confirm Common version and platform prerequisites.
- Back up production before first install and before major upgrades.
