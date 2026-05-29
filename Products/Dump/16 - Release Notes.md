# Release Notes

## Release

- Product: Dove365 CRM Starter
- Version: TODO: confirm release version
- Release date: TODO: confirm release date
- Solution versions analysed: Common `1.0.0.23`, CRM Starter `1.0.0.20`

## Summary

Initial CRM Starter release for small to medium-sized businesses needing lightweight CRM on Microsoft Power Platform and Dataverse.

## New Features

- Model-driven app `Dove365 CRM Starter`.
- Navigation for Dashboard, Relationships, Pipeline, and Service.
- Account and contact forms/views.
- Lead table, lead process, conversion and deactivation processes.
- Opportunity process and service offering relationship.
- Case process with escalation and resolution fields.
- Service Offering catalogue.
- CRM User and CRM Admin security roles.
- Flow failure notification child flow in Common.
- Outlook service account connection reference (`dove365_OutlookConnectServiceAccount`).
- Dataverse service account connection reference (`dove365_DataverseConnectionServiceAccount`).
- Power Automate flow `Opportunity - Close` replacing classic on-exit Opportunity Won/Lost workflows.
- Power Automate flow `Case - Resolve` replacing classic on-exit Case Resolve workflow.
- Power Automate flow `Lead - Convert` replacing classic on-exit Lead Convert workflow.
- LinkedIn Badge PCF control.
- Dove365 theme and app icons.

## Deployment Notes

Install Dove365 Common first, then Dove365 CRM Starter.

## Known Limitations

- Generative page may need further work or removal if dependent configuration files cause deployment issues.
- AI features are not included in the base CRM Starter release.
- Service Offering pricing fields are not a full product catalogue, quote, order, or invoicing module.
- SLA/entitlement automation is not included in the base support module.

## Post-Deployment Checks

- App opens.
- Roles exist.
- BPFs render.
- Environment variables are populated.
- Outlook connection reference is connected.
- Dataverse Intelligence is enabled.
- Dataverse Model Context Protocol is enabled.
- SharePoint document management works for Contacts.
- Mailboxes and email tracking work for pilot users.
