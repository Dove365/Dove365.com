# Licensing and Prerequisites

## Licensing

- Power Apps Premium is required.
- No Dove365 subscription is currently required.
- Microsoft 365, Outlook, and Exchange capabilities are required for email tracking scenarios.
- Licensing should be confirmed against the customer tenant and current Microsoft licensing guidance before production deployment.

Do not rely on this document as a complete Microsoft licensing statement.

## Environment Requirements

- Dataverse environment.
- Dynamics 365-enabled environment recommended, especially for Outlook for Dynamics 365.
- Power Apps Component Framework enabled.
- Power Apps Code Apps enabled.
- Dataverse Intelligence enabled.
- Dataverse Model Context Protocol enabled.
- Server-side sync enabled for incoming email, outgoing email, and appointments.
- Mailboxes approved and enabled.
- Email tracking enabled.

## SharePoint Requirements

- SharePoint site available.
- Configure server-based SharePoint Integration.
- Document Management Settings configured using the SharePoint site.
- Contacts selected in Document Management Settings to enable files for contacts in CRM.

## Service Account Requirements

- Service account mailbox available.
- Mailbox approved and enabled.
- Office 365 Outlook connection available for `dove365_OutlookConnectServiceAccount`.
- Service account email value configured in `dove365_ServiceAccountEmail`.
