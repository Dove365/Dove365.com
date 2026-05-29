# Technical Design Document

## Solution Inventory

| Solution | Unique name | Version | Managed | Publisher |
|---|---:|---:|---:|---|
| Dove365 Common | `Dove365Common` | `1.0.0.23` | No | Dove365, prefix `dove365` |
| Dove365 CRM Starter | `Dove365CRMStarter` | `1.0.0.20` | No | Dove365, prefix `dove365` |

## Dove365 Common Components

### Tables

Common includes 19 table components:

- Standard/platform tables: `account`, `activitypointer`, `annotation`, `connection`, `contact`, `email`, `phonecall`, `position`, `sharepointdocument`, `sharepointdocumentlocation`, `systemuser`, `task`, `team`.
- Custom/business tables: `dove365_case`, `dove365_caseresolve`, `dove365_opportunity`, `dove365_opportunityconvert`, `dove365_opportunityserviceoffering`, `dove365_serviceoffering`.

### Global Choices

Common includes global choices:

- `dove365_billingfrequency`: Weekly, Monthly, Quarterly, Annual.
- `dove365_billingtype`: Fixed Price, Daily Rate, Hourly, Monthly Retainer, Per User, Subscription, Per Unit, Milestone Based, Time & Materials, Usage Based, Commission Based, Custom.
- `dove365_casetype`: Support Request, Issue, Complaint, Service Request, Billing Query, Change Request, General Enquiry, Other.
- `dove365_durationunit`: Days, Weeks, Months, Years.
- `dove365_priority`: Critical, High, Medium, Low.
- `dove365_servicecategory`: Consulting, IT & Technology, Marketing, Design, Sales, Support, Maintenance, Repair, Training, Installation, Finance, Legal, Healthcare, Construction, Logistics, Manufacturing, Data & Reporting, Administration, Cleaning, Education, Real Estate, Human Resources, Customer Service, Operations, Other.
- `dove365_source`: Email, Phone, Website, In Person, Internal, Other.
- `processstage_category`: includes Microsoft standard values plus Dove365 custom stage categories for lead, opportunity, and case BPF stages.

### Business Process Flows

- `Case - Resolve`, backed by `dove365_caseresolve`.
- `Opportunity - Convert`, backed by `dove365_opportunityconvert`.

### Processes and Cloud Flows

- Cloud flow: `Notify when flows fail`; exported as `Notifywhenflowsfail-633EF563-4350-F111-BEC7-7CED8D399CB5.json`.
- Classic/background or real-time processes: `Case - Resolve`, `Case - Cancelled`, `Copy Main Address`, `Show Escalated Fields`, `Show Cancelled Reason`, `Show Opportunity Lost Reason`.

> Note: `Opportunity - Won` and `Opportunity - Lost` were on-exit classic workflows that have been replaced by the `Opportunity - Close` Power Automate flow in CRM Starter (see below). `Case - Resolve` on-exit behaviour and `Lead - Convert` on-exit behaviour have similarly been replaced by Power Automate flows.

### Environment Variables

| Schema name | Display name | Purpose | Required |
|---|---|---|---|
| `dove365_FlowFailureNotificationAccount` | Flow Failure Notification Account | Recipient mailbox/account for failed flow notifications. | No |
| `dove365_ServiceAccountEmail` | Service Account Email | Sender/service account used for notification emails, reminders, and system-generated communications. | No |

### Connection References

Both connection references must be signed in with the dedicated service account during or immediately after Common import.

| Logical name | Display name | Connector |
|---|---|---|
| `dove365_OutlookConnectServiceAccount` | Outlook Connect (Service Account) | Office 365 Outlook |
| `dove365_DataverseConnectionServiceAccount` | Dataverse Connection (Service Account) | Microsoft Dataverse |

### PCF Controls

| Control | Namespace | Version | Purpose |
|---|---|---:|---|
| LinkedIn Profile Badge | `Dove365.LinkedInBadge` | `1.0.0` | Renders a LinkedIn profile badge from a bound LinkedIn URL field. |

Properties: `linkedinUrl` bound to `SingleLine.URL`, optional `badgeSize`, optional `badgeTheme`.

### Web Resources

- `dove365_CasesIcon`
- `dove365_ServiceOfferingsIcon`
- `dove365_LeadsIcon`
- `dove365_OpportunitesIcon`
- `dove365_Dove365IconTransparent`
- `dove365_Dove365AppIcon`
- `dove365_Dove365Theme`

### Theme Override

Common contains organization setting `OverrideAppHeaderColor`. This supports the Dove365 theme assets and app header branding.

## Dove365 CRM Starter Components

### Tables

CRM Starter includes 10 table components:

- `account`
- `contact`
- `dove365_case`
- `dove365_caseresolve`
- `dove365_lead`
- `dove365_leadconvert`
- `dove365_opportunity`
- `dove365_opportunityconvert`
- `dove365_opportunityserviceoffering`
- `dove365_serviceoffering`

### App Module and Sitemap

- Model-driven app: `dove365_Dove365CRMStarter`, display name `Dove365 CRM Starter`.
- Sitemap groups: Dashboard, Relationships, Pipeline, Service.
- App roles mapped include `Dove365 CRM Admin`, `Dove365 CRM User`, and built-in/system roles referenced by ID in the app module.

### Security Roles

- `Dove365 CRM Admin`
- `Dove365 CRM User`

Design Context: Admin users maintain service offerings. Standard CRM users can view and append service offerings where configured.

### CRM Starter Choices

Local lead choices found in metadata:

- `dove365_lead_dove365_contactmethod`: Any, Email, Phone, Mail.
- `dove365_lead_dove365_gender`: Male, Female, Other.
- `dove365_lead_dove365_leadsource`: Other, Conference, Network Event, Website Contact Form, Agency, Word of Mouth, Referral.
- `dove365_lead_dove365_maritalstatus`: Married, Single, Divorced, Widowed.
- `dove365_lead_dove365_qualify`: Yes, No.
- `dove365_lead_dove365_role`: Employee, Decision Maker, Influencer.
- `dove365_lead_dove365_temperature`: Hot, Warm, Cold, Dead.
- Marketing flags: `dove365_emailmarketing`, `dove365_mailmarketing`, `dove365_phonemarketing`.

### Forms and Views

Key forms found:

- Account: `New Account`, `New Account Quick Create`, `Account Contact Card`.
- Contact: `New Contact`, `New Contact Quick Create`, `Contact Card`.
- Lead: `New CRM Lead`, `New Lead Quick Create`, BPF form `Information`.
- Case: `New CRM Case`.
- Opportunity: `New CRM Opportunity`, `New CRM Opportunity Quick Form`.
- Opportunity Service Offering: `CRM Opportunity Service Offering`.
- Service Offering: `CRM Service Offering`, `CRM Service Offering Quick Form`.

Key views found:

- Accounts: Active Accounts, All Accounts, Inactive Accounts, My Active Accounts.
- Contacts: Active Contacts, All Contacts, Inactive Contacts, My Active Contacts.
- Leads: Active Leads, Converted Leads, Inactive Leads, My Leads, Lead Lookup, Lead Associated, Quick Find Active Leads.
- Cases: Active Cases, Inactive Cases, My Cases.
- Opportunities: Active Opportunities, Inactive Opportunities, Lost Opportunities, My Opportunities, Won Opportunities.
- Service Offerings: CRM Service Offerings View plus Common service offering views.

### Processes and Power Automate Flows

CRM Starter includes or references the following classic processes:

- `Lead - Convert Lead to Contact`
- `Lead - Deactivate Lead`
- `Lead - Build Full Name`
- `Show Existing Contact and Account when Converted`
- `Close Convert Reason`
- `Copy Main Address`
- `Opportunity - Convert` (BPF)
- `Case - Resolve` (BPF)
- `Show Escalated Fields`
- `Show Cancelled Reason`
- `Show Opportunity Lost Reason`

CRM Starter includes the following Power Automate flows, which replace previous on-exit classic workflows:

| Flow display name | Trigger | Replaces |
|---|---|---|
| `Opportunity - Close` | When Opportunity BPF stage is finished | `Opportunity - Won` and `Opportunity - Lost` classic workflows |
| `Case - Resolve` | When a Case is Resolved | `Case - Resolve` classic on-exit workflow |
| `Lead - Convert` | When a Lead is Converted or Not Converted | `Lead - Convert` classic on-exit workflow |

All three flows use the `dove365_DataverseConnectionServiceAccount` Dataverse connection reference.

Some processes are also present in or depend on Dove365 Common.

## External Configuration Dependencies

The following are not fully delivered by solution import and must be configured in the target environment:

- Dynamics 365-enabled environment, ideally to support Outlook for Dynamics 365.
- Power Apps Component Framework enabled.
- Power Apps Code Apps enabled.
- Dataverse Intelligence enabled.
- Dataverse Model Context Protocol enabled.
- Server-side sync for incoming email, outgoing email, and appointments.
- Approved and enabled mailboxes.
- Email tracking enabled.
- SharePoint server-based integration configured.
- Document Management Settings configured with the SharePoint site.
- Contacts selected for Document Management Settings to enable contact files.
