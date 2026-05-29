# Support Documentation

## Common Issues

### Solution Import Dependency Errors

Check that Dove365 Common is installed before Dove365 CRM Starter. Review missing dependency names for tables, relationships, BPFs, web resources, PCF controls, and forms.

### Flow Failure Notifications Not Sending

Confirm:

- `dove365_FlowFailureNotificationAccount` has a valid email value.
- `dove365_OutlookConnectServiceAccount` is connected.
- The service account mailbox is approved, enabled, and can send email.

### Email Tracking Issues

Confirm server-side sync is enabled for incoming email, outgoing email, and appointments. Check mailbox test results and user email tracking settings.

### SharePoint Document Issues

Confirm server-based SharePoint integration, Document Management Settings, SharePoint site access, and that Contacts are enabled for document management.

### Missing Security Role Issues

Confirm the user has `Dove365 CRM User` or `Dove365 CRM Admin`, plus any required base Dataverse/Dynamics roles for app and table access.

### BPF Not Showing

Confirm the relevant process is active, the user has process/table privileges, and the record is using the correct table/form.

### PCF Control Not Rendering

Confirm PCF is enabled in the environment and the LinkedIn profile field contains a valid URL.

### Dashboard or Generative Page Issues

The generative page may rely on dependency/configuration files not fully portable between environments. If imports fail or the page does not render, validate page dependencies or remove the page from the app until it is production-ready.

## Escalation Guidance

Escalate to Dove365 delivery/admin support when:

- Managed solution import fails after Common dependency is confirmed.
- App module or sitemap fails to load.
- Security roles import incorrectly.
- BPF backing tables or process activation cannot be resolved.
- Flow connection references cannot be repaired.

