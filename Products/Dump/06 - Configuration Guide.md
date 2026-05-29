# Configuration Guide

## Environment Variables

| Display name | Schema name | Configuration |
|---|---|---|
| Flow Failure Notification Account | `dove365_FlowFailureNotificationAccount` | Set to the mailbox/account that should receive failed flow notifications. |
| Service Account Email | `dove365_ServiceAccountEmail` | Set to the mailbox used for service account communications. |

## Connection References

Common includes two connection references. Both must be configured with the dedicated service account before the Power Automate flows in CRM Starter can run.

| Logical name | Display name | Connector |
|---|---|---|
| `dove365_OutlookConnectServiceAccount` | Outlook Connect (Service Account) | Office 365 Outlook |
| `dove365_DataverseConnectionServiceAccount` | Dataverse Connection (Service Account) | Microsoft Dataverse |

Configure each connection reference by signing in with the service account in the Power Apps maker portal under **Solutions → Connection References** after Common import.

## Service Account Setup

1. Create or identify the service account mailbox.
2. License it according to the customer tenant requirements.
3. Approve and enable the mailbox.
4. Test incoming email, outgoing email, and appointments if the account will support those functions.
5. Use this account for the Outlook connection reference.

## Flow Failure Notification Setup

The child flow `Notify when flows fail` accepts parent flow name and message, then sends a high-importance email to the `Flow Failure Notification Account` environment variable value.

## Dataverse Intelligence and MCP

Before go-live, confirm the target environment has the required Dataverse capabilities enabled:

- Dataverse Intelligence enabled.
- Dataverse Model Context Protocol enabled.

These are environment-level capabilities and are not configured by the CRM Starter solution import. Confirm them in the relevant Power Platform admin/configuration experience for the target tenant.

## Theme Configuration

Common includes `dove365_Dove365Theme` and organization setting `OverrideAppHeaderColor`. Confirm the app header and theme render as expected after import.

## SharePoint Document Management

1. Configure server-based SharePoint Integration.
2. Run Document Management Settings.
3. Select the SharePoint site.
4. Select Contacts to enable files for contacts in CRM.
5. Test by opening a contact record and checking the Files/Documents tab.

## Email Tracking and Server-Side Sync

1. Configure email processing method for users.
2. Approve mailboxes.
3. Test and enable mailboxes.
4. Confirm tracked emails appear on the appropriate timeline.

## Security Role Assignment

- Assign `Dove365 CRM User` to standard CRM users.
- Assign `Dove365 CRM Admin` to CRM administrators.
- Also assign any required base Microsoft roles needed for app access, Dataverse use, Outlook integration, or system administration.

### Dynamics 365 for Outlook

If the client requires the Dynamics 365 for Outlook add-in, two additional steps are needed beyond the standard role assignment:

1. **Assign the security role**: In the Dataverse environment, assign the `Dynamics 365 for Outlook` security role to each user who needs the add-in. This is a Microsoft-provided role separate from the Dove365 CRM roles.
2. **Deploy the app via the admin center**: In the Microsoft 365 admin center, go to **Settings → Integrated apps**, find Dynamics 365 for Outlook, and deploy it to the relevant users or groups. Without this step the add-in will not appear in Outlook even if the security role is assigned.

## Service Offering Setup

Admin users should create service offerings before go-live:

- Service Name
- Service Code
- Service Type
- Service Category
- Description
- Billing Type
- Currency
- Default Price
- Default Cost
- Duration Unit
- Default Duration

These fields support CRM pipeline and case categorisation. They are not a complete invoicing or product catalogue module.

## Business Process Flow Activation

Confirm the following BPFs are active:

- Lead conversion process.
- Opportunity conversion process.
- Case resolution process.

TODO: Confirm final BPF display names in target after import, as some process names are represented by backing table/process metadata.

## App Sharing

Share the `Dove365 CRM Starter` app with the Dove365 CRM roles and any additional base roles required by the customer tenant.
