# ALM and Solution Layering

## Import Order

Always install in this order:

1. Dove365 Common
2. Dove365 CRM Starter

CRM Starter has table, form, process, BPF, web resource, and app dependencies on Common components.

## Development Approach

The supplied exports are unmanaged solutions:

- `Dove365Common` version `1.0.0.23`
- `Dove365CRMStarter` version `1.0.0.20`

Recommended development pattern:

- Use unmanaged solutions in development.
- Keep Common for reusable platform assets.
- Keep CRM Starter for the app, roles, sitemap, lead features, and CRM-specific forms/views.
- Avoid moving the same component between solutions without deliberately checking dependencies.

## Production Approach

Recommended production deployment is managed solution import:

1. Export managed Dove365 Common from the controlled build environment.
2. Import Dove365 Common managed into Test, then Production.
3. Export managed Dove365 CRM Starter from the controlled build environment.
4. Import Dove365 CRM Starter managed into Test, then Production.
5. Apply environment variable values and connection references during import.

## Environment Strategy

Use at least:

- Development: unmanaged authoring and configuration.
- Test/UAT: managed import validation, user acceptance testing, deployment rehearsal.
- Production: managed solutions only, with controlled update cadence.

## Publisher and Naming

The publisher is Dove365 with prefix `dove365` and option value prefix `67080`. Continue using this publisher for product-owned components to keep schema naming consistent.

## Dependency Management

Known ALM risk areas:

- Table relationships, especially Case, Opportunity, Contact, Account, Service Offering, and Opportunity Service Offering relationships.
- Forms and views split between Common and CRM Starter.
- PCF control dependency on Power Apps Component Framework being enabled.
- Environment capability dependency on Dataverse Intelligence being enabled where AI-ready Dataverse features are expected.
- Environment capability dependency on Dataverse Model Context Protocol being enabled where MCP-based access or future AI/agent integrations are expected.
- Cloud flow connection reference `dove365_OutlookConnectServiceAccount`.
- Environment variables `dove365_FlowFailureNotificationAccount` and `dove365_ServiceAccountEmail`.
- Business process flow backing tables and process activation.
- Classic process dependencies duplicated across Common and CRM Starter.
- Generative page dependency files in the app may cause import issues if required hidden/configuration components are missing.

## Recommended Export/Import Process

1. Validate solution checker results in development.
2. Publish all customizations.
3. Export Common first.
4. Export CRM Starter second.
5. Import Common into target.
6. Import CRM Starter into target.
7. Resolve connection references and environment variables.
8. Confirm required environment capabilities are enabled, including Dataverse Intelligence and Dataverse Model Context Protocol.
9. Activate or confirm activation of flows and BPFs.
10. Share the app with required security roles.
11. Run smoke tests for accounts, contacts, leads, opportunities, cases, documents, and email tracking.
