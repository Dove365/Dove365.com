# Admin Guide

## Overview

The Dove365 Admin app is part of Dove365 Common and is the central administration hub for the Power Platform environment. It is separate from the CRM Starter app and requires the **Dove365 Global Admin** security role to access.

### Admin App Areas

| Area | Contents |
|---|---|
| User Management | Users, Teams, Mailboxes, Positions |
| SharePoint Settings | Document Locations, Documents |
| Data Configuration | Service Offerings |
| Support | Training Material, Report a Bug |

---

## User Management

### Managing Users

Users must already exist in the Dataverse environment before appearing in the Admin app. Users are provisioned through the Microsoft 365 admin centre and appear in Dataverse after licensing and environment access is granted.

**Key user fields to complete:**

- Full Name and User Name (read-only, set by M365)
- Job Title
- Primary Email (read-only, set by M365)
- Mobile Phone and Main Phone
- Business Unit, Manager, and Position

### Assigning Security Roles

Dove365 uses a custom **Security Role Manager** PCF control on the User and Team forms. This control displays all available Dove365 security roles as checkboxes. Checking or unchecking a role applies the change immediately — no form save is required.

**Standard Dove365 roles:**

| Role | Purpose |
|---|---|
| `Dove365 CRM User` | Standard CRM access for day-to-day users |
| `Dove365 CRM Admin` | Administrative CRM access including service offering maintenance |
| `Dove365 Global Admin` | Full Admin app access |
| `Dynamics 365 for Outlook User` | Enables the Outlook add-in (shown if D365 is enabled in the environment) |

### Managing Teams

Teams group users for role assignment and record ownership. Roles assigned to a team apply to all members.

- Open User Management > Teams and select New.
- Enter Team Name, Business Unit, and Administrator.
- Add members via the Team Members subgrid.
- Use the Security Role Manager control on the team form to assign roles.

> **Note:** Assigning a role to a team does not propagate it to individual user records. Users have the role through team membership only.

### Managing Positions

Positions define the reporting hierarchy.

1. Open User Management > Positions and create positions for each level in your structure.
2. Set the Parent Position on each record to build the hierarchy.
3. Assign positions to users via the Position field on the User form.

---

## Mailboxes

Mailboxes must be approved and tested before email tracking, server-side synchronisation, and Dynamics 365 App for Outlook will work.

### Setup Steps

1. Open User Management > Mailboxes and use the Active Mailboxes view.
2. Open a mailbox record and confirm the Server Profile is set correctly, and that Incoming Email, Outgoing Email, and Appointments/Contacts/Tasks synchronisation methods are all set to **Server-Side Synchronization**.
3. Select **Approve Email** from the command bar and confirm.
4. Select **Test & Enable Mailbox** from the command bar.
5. Wait for the test to complete and verify that Incoming Email Status, Outgoing Email Status, and ACT Status all show **Success** in the Configuration Status section.

### Service Account Mailbox

The Dove365 service account used by Power Automate connection references must also have its mailbox approved and tested. Ensure the **Outlook Connect Service Account** connection reference uses this account and is connected.

### Troubleshooting Mailbox Issues

- Email not approved → run Approve Email first
- Server profile missing → assign the correct Exchange/Microsoft 365 server profile
- OAuth not enabled on Exchange → check Exchange admin settings
- Mailbox linked to another environment → the Exchange setting must be overwritten

> A mailbox can only synchronise with one Dataverse environment at a time.

---

## SharePoint Settings

### Prerequisites

- Server-based SharePoint integration enabled in Power Platform admin or Advanced Settings
- SharePoint site URL configured
- Document Management Settings set for the required tables (Account, Contact, Opportunity, Case, etc.)

### Setup Steps

1. Enable server-based SharePoint integration from Power Platform admin centre or Settings > Document Management > Advanced Settings.
2. In Document Management Settings, select the tables for which SharePoint document folders should be created.
3. Review Document Locations in the Admin app to confirm locations are being created for records.
4. Use the Documents view to browse files stored in managed locations.

### Troubleshooting SharePoint Issues

If documents are not appearing on records, confirm:

- Server-based SharePoint integration is enabled
- Document Management Settings include that table
- The SharePoint site is reachable
- User permissions allow access to the SharePoint document library

> Contact records require the Contact table specifically enabled in Document Management Settings.

---

## Data Configuration

### Managing Service Offerings

Admins maintain the service catalogue in Data Configuration > Service Offerings. CRM users select from these offerings when adding services to opportunities and cases.

**Key fields:**

| Field | Purpose |
|---|---|
| Service Name | Clear, consistent name visible to all CRM users |
| Service Code | Short reference code for reporting |
| Service Category | e.g. Consulting, IT & Technology, Sales, Support |
| Service Type | One-Time, Recurring, Project, or Subscription |
| Billing Type | Fixed, Time & Materials, Retainer, etc. |
| Default Price / Cost | Reference values for pipeline context |
| Duration | Expected duration and unit |

**Best practices:**

- Agree on naming conventions before creating records
- Search before creating to avoid duplicates
- Deactivate rather than delete retired offerings — deletion breaks existing records that reference them

> Service Offering pricing fields are for CRM reference and pipeline context only. They are not a quoting, ordering, or invoicing module.

---

## Dynamics 365 App for Outlook

### Admin Deployment

The Dynamics 365 App for Outlook allows users to track emails, view CRM data, and create records directly from Outlook on desktop and web.

**Prerequisites:**
- Server-side synchronisation configured
- Mailboxes approved and tested for target users
- OAuth enabled on Exchange
- Dynamics 365 for Outlook User role assigned directly to the user

**Deployment steps:**

1. In the Admin app, assign the **Dynamics 365 for Outlook User** role to the target user via the Security Role Manager on their User form.
2. Go to Settings > Dynamics 365 App for Outlook in Advanced Settings.
3. Choose a deployment option:
   - Enable auto-deploy for all eligible users
   - Add App for Eligible Users (push all at once)
   - Select specific users and choose Add App to Outlook
4. Status changes to **Added to Outlook** when successful. The add-in appears in the user's Outlook ribbon.

> Assigning the role to a team does **not** automatically propagate App for Outlook privileges to team members. Assign the role directly to individual users.

> An Exchange mailbox can only synchronise with one Dataverse environment at a time. If a user's mailbox was previously connected to another environment, this must be cleared before the add-in will work here.

### User Guide

Once deployed, users can:

- Open the Dynamics 365 pane by selecting **Dynamics 365** in the Outlook ribbon (desktop) or More (...) > Dynamics 365 (web)
- Pin the pane to keep it open across emails
- View contact and account CRM data for email senders and recipients
- **Track** an email to save a copy in Dataverse (appears on linked record timelines)
- **Set Regarding** to link a tracked email to a specific opportunity, case, lead, account, or contact
- **Untrack** to remove the copy from Dataverse
- Create new CRM records (contacts, leads, tasks, phone calls) directly from Outlook using the quick create control
- Navigate using the site map to access Home, Recent, Pinned, and Dashboards

---

## Monitoring Flows

Monitor the following Power Automate flows and confirm connection references remain valid:

| Flow | Connection Used |
|---|---|
| `Notify when flows fail` | Outlook Connect (Service Account) |
| `Opportunity - Close` | Dataverse Connection (Service Account) |
| `Case - Resolve` | Dataverse Connection (Service Account) |
| `Lead - Convert` | Dataverse Connection (Service Account) |

If any connection reference expires or loses consent, the dependent flows will fail. Review connection health in the Power Automate portal.

Flow failure notifications are sent to the configured `Flow Failure Notification Account`. The email subject includes the failing parent flow name.

---

## Common Admin Tasks

- Add or deactivate users as the team changes
- Assign and adjust roles using the Security Role Manager on User and Team forms
- Review mailbox sync status and resolve errors
- Update or add Service Offerings as the business evolves
- Check Power Automate flow health and connection references (Outlook and Dataverse)
- Review SharePoint document locations if integration issues arise
- Assign or remove the Dynamics 365 for Outlook User role as needed
- Deploy or redeploy Dynamics 365 App for Outlook when new users are onboarded
- Publish customisations after controlled changes in a dev environment
- Confirm BPFs are active after solution imports
