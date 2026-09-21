# Salesforce Naming & Metadata Standards

**Version:** 2.0  
**Status:** Active
**Language:** English   
**Scope:** Salesforce Platform metadata, declarative automation, Apex, LWC, security and integrations

---

## 1. Purpose

This standard defines naming, documentation and organization conventions for Salesforce metadata. Its objectives are to improve readability, maintainability, discoverability, deployment safety and governance across teams and projects.

The standard is based on seven principles:

1. **Business meaning over implementation.** Names describe business meaning or technical responsibility, not storage type or implementation detail.
2. **Context over redundancy.** Do not repeat information already provided by the object, metadata type or surrounding context.
3. **Clarity over brevity.** Prefer an explicit name to an abbreviation that requires interpretation.
4. **English API names.** Developer/API names are written in English. User-facing labels can follow the organization's business language and Salesforce Translation Workbench strategy.
5. **Do not encode the metadata type unnecessarily.** Prefixes such as `PS`, `LP`, `CL`, `RT`, `flow`, `PB` and `WR` are not used merely to identify the metadata type.
6. **Names explain WHAT; descriptions explain WHY.** The name identifies responsibility. The Description documents purpose, invocation, dependencies and relevant business context.
7. **Standards must be enforceable.** Whenever practical, conventions should be verifiable through source control, pull-request checks, static analysis or CI/CD validation.

---

## 2. General Rules

### 2.1 Language

- Use **English** for Developer/API Names, source code identifiers and technical descriptions intended for cross-team maintenance.
- User-facing labels may be Portuguese when the organization's primary business language is Portuguese.
- Use Translation Workbench when the solution must support multiple user languages.
- Do not create API Names from automatically normalized Portuguese labels.

**Good**

```text
Label: Tipo de Sugestão
API Name: SuggestionType__c

Label: Configuração Padrão
API Name: DefaultSetting__c
```

**Avoid**

```text
Tipo_de_Sugestao__c
Configuracao_Padrao__c
```

### 2.2 Casing

Use **PascalCase** for custom objects and fields.

```text
ContractRequest__c
SuggestionType__c
ExternalCustomerId__c
ContractStartDate__c
```

Use the conventions defined in the relevant sections for metadata where separators improve readability, especially Flow.

### 2.3 Underscores

Avoid `_` as a word separator in API Names for custom objects and fields. Salesforce-required suffixes such as `__c`, `__mdt` and `__e` are exceptions.

For technical artifacts such as Flow and Permission Sets, `_` may be used as a **semantic separator** when it materially improves readability.

```text
Account_AfterSave_SyncERP
Contract_Manage
```

### 2.4 Abbreviations and acronyms

- Do not truncate names merely to make them shorter.
- Avoid ambiguous abbreviations such as `Cust`, `Cfg`, `Mgr`, `Sts` or `Amt` unless an organization-wide glossary explicitly standardizes them.
- Widely recognized domain or system acronyms may be preserved consistently.

**Preferred**

```text
CustomerEligibilityStatus__c
ERPAccountId__c
SAPCustomerId__c
```

**Avoid**

```text
CustEligSts__c
ERPAccountID__c
SapCustomerId__c
```

### 2.5 Avoid technical type prefixes and suffixes

Do not encode a field's data type or relationship mechanism in its name.

**Avoid**

```text
SuggestionTypeTxt__c
DefaultSettingLkp__c
LkpDefaultSetting__c
IsActiveChk__c
ExpirationDateDt__c
```

**Preferred**

```text
SuggestionType__c
DefaultSetting__c
IsActive__c
ExpirationDate__c
```

### 2.6 Avoid contextual redundancy

Do not repeat the parent object's concept unless required to remove genuine ambiguity.

For `Address__c`:

```text
Type__c
Street__c
City__c
```

Avoid:

```text
AddressType__c
AddressStreet__c
AddressCity__c
```

### 2.7 Description is part of the metadata contract

Use Description/Help Text wherever Salesforce provides it. For significant automation, code-facing metadata and configuration, document at minimum:

```text
Purpose:
Invocation:
Business Owner:
Technical Owner:
Dependencies:
Integration:
Story:
Notes:
```

Only include applicable entries. Do not duplicate obvious information simply to fill the template.

Example:

```text
Purpose:
Creates an ERP customer after Account activation.

Invocation:
Account_AfterSave_SyncERP.

Dependencies:
SAPCustomerClient
CustomerIntegrationConfig__mdt

Business Owner:
Sales Operations

Story:
CRM-4281
```

---

## 3. Custom Objects

### Convention

Use a singular business noun or meaningful business concept in PascalCase.

```text
ContractRequest__c
ServiceAgreement__c
CustomerPreference__c
```

Avoid technical implementation names such as:

```text
ContractData__c
GenericRecord__c
CustomTable__c
```

Use a clear Description stating ownership and business purpose.

---

## 4. Custom Fields

### 4.1 General convention

Use PascalCase and choose the shortest name that remains unambiguous in the object's context.

```text
ExpirationDate__c
CustomerSegment__c
MaximumDiscountPercentage__c
```

### 4.2 Boolean fields

Prefer names that read naturally as a true/false proposition.

```text
IsEligible__c
HasActiveContract__c
RequiresApproval__c
```

Avoid:

```text
EligibleFlag__c
ActiveContractChk__c
```

### 4.3 Date and Date/Time

Make the business event explicit when necessary.

```text
ActivationDate__c
ExpirationDate__c
LastSynchronizationDateTime__c
```

Do not append `Date` when the concept is already naturally a date only if doing so would remain immediately clear. Consistency within the domain takes precedence.

### 4.4 Currency, percentage and quantity

Use the business meaning rather than the storage type.

```text
MaximumDiscount__c
AnnualRevenueTarget__c
RequestedQuantity__c
```

Where ambiguity exists, an explicit semantic qualifier is allowed:

```text
MaximumDiscountPercentage__c
```

### 4.5 External identifiers

Prefer a name that identifies the owning/source system and business entity.

```text
ERPAccountId__c
SAPCustomerId__c
CommerceOrderId__c
LegacyCustomerId__c
```

Avoid generic names when multiple source systems may exist:

```text
ExternalId__c
ExternalCode__c
```

Configure the Salesforce **External ID** and, where appropriate, **Unique** field attributes according to the integration contract.

### 4.6 Field descriptions and Help Text

- **Description:** technical/business purpose, source of truth, automation dependency and unusual constraints.
- **Help Text:** concise user-facing guidance about what value to enter or how the field is used.

Do not use Help Text as technical documentation.

---

## 5. Relationships

### 5.1 Relationship fields

Use the name of the referenced business concept unless the relationship has a distinct role.

```text
Account__c
PrimaryContact__c
BillingAccount__c
Approver__c
```

### 5.2 Child Relationship Name

Use a meaningful **plural collection name** that reads naturally in SOQL and Apex.

```text
Invoices
ApprovalRequests
ContractItems
```

Example:

```sql
SELECT Id,
       (SELECT Id, Status__c FROM ApprovalRequests__r)
FROM Opportunity
```

Avoid mechanically combining object and field names when the resulting collection is less readable.

### 5.3 Related List Label

Use a user-facing plural label describing the child collection. Add a role qualifier only when multiple relationships between the same objects would otherwise be ambiguous.

```text
Invoices
Approval Requests
Billing Contacts
Technical Contacts
```

---

## 6. Record Types

### Convention

The Developer Name represents the business classification only. Do not prefix it with `RT` or repeat the object name.

For Account:

```text
ShippingCompany
Partner
Customer
Supplier
```

Avoid:

```text
RTAccountShippingCompany
AccountShippingCompany
```

### Integration rule

Do **not** use Record Type names as a general external integration contract. Record Types are Salesforce configuration and may change due to business reclassification.

When an external system requires a stable code, prefer one of the following:

- a dedicated External ID/business key field;
- Custom Metadata mapping;
- an explicit translation layer in the integration service.

Example:

```text
RecordTypeMapping__mdt
```

---

## 7. Validation Rules

Use a concise expression of the business condition being enforced. The name should remain understandable without opening the formula.

```text
CloseDateRequiredWhenClosed
EndDateMustBeAfterStartDate
DiscountRequiresApproval
BillingCountryRequiredForInvoice
```

Avoid generic names:

```text
Validation1
CheckFields
RequiredFields
```

Always provide a clear error message. Use Description to state the business rule or Story/requirement behind non-obvious validations.

---

## 8. Page Layouts

Use a human-readable label based on the object and business scenario.

```text
[Case] Shipping Company
[Account] Partner
[Opportunity] Enterprise Sales
```

If only one layout exists:

```text
[Account] Default Layout
```

Do not introduce a technical prefix solely to identify a Page Layout.

---

## 9. Compact Layouts

Use a business-focused name. Do not prefix with `CL` merely to identify the metadata type.

```text
AccountDefault
AccountPartner
CaseServiceConsole
```

Labels may follow the same readable convention used for Page Layouts:

```text
[Account] Default
[Account] Partner
```

---

## 10. Lightning Record Pages / FlexiPages

Use the object or domain plus the business audience/scenario.

```text
Case_Client
Case_Partner
Account_Default
Opportunity_EnterpriseSales
```

Avoid redundant type markers such as:

```text
LPCaseClientLayout
LPAccountDefaultLayout
```

For app pages and home pages, use the functional purpose:

```text
SalesOperationsHome
ServiceSupervisorHome
ContractOperations
```

---

## 11. Salesforce Flow

Flow is the default declarative automation standard. Workflow Rules and Process Builder are treated as legacy automation and must not be used for new implementations.

### 11.1 General principles

- Keep each Flow focused on a clear responsibility.
- Use Description to state business purpose, invocation, objects affected, dependencies and relevant work item/Story.
- Prefer reusable subflows for cohesive reusable behavior, not merely to reduce canvas size.
- Make names understandable to someone unfamiliar with the original project.
- Do not prefix a Flow with `flow` solely because it is a Flow.

### 11.2 Record-triggered Flow

Convention:

```text
<Object>_<Timing>_<Purpose>
```

Examples:

```text
Account_BeforeSave_SetDefaults
Account_AfterSave_SyncERP
Case_AfterSave_HandleClosure
Opportunity_AfterSave_CreateContract
```

Use Salesforce execution semantics in the timing portion. Prefer recognizable values such as `BeforeSave` and `AfterSave` rather than historical trigger terminology such as `BeforeInsert` when the Flow runs for more than a single create scenario.

If separate create/update flows are intentionally required, make the event explicit in the purpose/context rather than relying on an ambiguous name.

### 11.3 Screen Flow

Convention:

```text
<Domain>_<Purpose>
```

Examples:

```text
Case_CreateReturnRequest
Opportunity_RequestApproval
Customer_Onboarding
```

### 11.4 Scheduled Flow

Convention:

```text
<Domain>_Scheduled_<Purpose>
```

Examples:

```text
Case_Scheduled_CloseStaleCases
Contract_Scheduled_SendExpirationReminder
```

### 11.5 Autolaunched Flow / Subflow

Convention:

```text
<Domain>_<Action>
```

Examples:

```text
Case_SendSurvey
Account_CalculateCustomerTier
Order_CreateInvoice
```

### 11.6 Flow elements

Element names describe the operation and business target.

**Get Records**

```text
GetAccount
GetActiveContracts
GetOpenCases
```

**Create Records**

```text
CreateCase
CreateContractItems
```

**Update Records**

```text
UpdateOpportunity
UpdateCustomerStatus
```

**Delete Records**

```text
DeleteExpiredReservations
```

**Assignment**

```text
AssignDefaultValues
AssignRequestOwner
```

**Decision**

```text
EvaluateEligibility
EvaluateContractStatus
```

Decision outcomes should read as meaningful conditions:

```text
CustomerIsEligible
ContractIsActive
ContractIsExpired
ContractHasNoExpirationDate
```

**Loop**

```text
LoopContractItems
LoopOpenCases
```

**Action / Subflow**

```text
SendApprovalNotification
CalculateCustomerTier
CreateERPRequest
```

Avoid names that merely reproduce the element type without explaining its purpose.

### 11.7 Flow variables and resources

Prefer semantic names without Hungarian notation or storage-type prefixes.

```text
recordId
accountId
customerNumber
contractStartDate
isEligible
hasActiveContract
account
accounts
contract
contracts
```

Avoid:

```text
varAccountId
txtCustomerNumber
boolIsEligible
colAccounts
recAccount
```

Collections should normally use a plural noun; single-record resources use singular nouns.

For input/output variables, document the contract in Description and use Salesforce's Available for input/output settings rather than encoding `Input`/`Output` into every variable name unless the distinction is necessary for clarity.

### 11.8 Formulas and constants in Flow

Use semantic names:

```text
NormalizedCustomerNumber
ExpirationThresholdDate
MaximumRetryCount
```

Avoid:

```text
Formula1
Constant1
TempValue
```

---

## 12. Legacy Automation

The following technologies must not be selected for new automation:

- Workflow Rules
- Process Builder

Existing automation should be inventoried, regression tested and progressively migrated to Salesforce Flow. Existing names may be retained during migration when renaming would create unnecessary deployment risk.

Prefixes retained only for identifying legacy artifacts are allowed during a transition period, for example:

```text
PBCaseAutomation
WROpportunityExternalID
```

They are not part of the naming standard for new metadata.

---

## 13. Permission Sets and Permission Set Groups

### 13.1 Permission Sets

Design Permission Sets around cohesive **capabilities**, not job titles whenever practical.

Convention:

```text
<Domain>_<Capability>
```

Examples:

```text
Contract_Read
Contract_Manage
CustomerData_Read
CustomerData_Manage
Integration_ERP
Case_Supervisor
```

Do not prefix with `PS` solely to indicate the metadata type.

### 13.2 Permission Set Groups

Permission Set Groups represent personas or bundles of capabilities.

```text
SalesRepresentative
SalesManager
ServiceAgent
ServiceSupervisor
```

Conceptual model:

```text
Persona
  -> Permission Set Group
     -> Capabilities
        -> Permission Sets
```

Use Muting Permission Sets deliberately and document exceptions; do not rely on muting as a substitute for poorly scoped Permission Sets.

---

## 14. Profiles

Profiles should not be the primary mechanism for accumulating business permissions. Keep profile-specific configuration to platform requirements and prefer Permission Sets/Permission Set Groups for additive access that represents capabilities.

When profiles must be named, use a clear persona or technical purpose and avoid organizational abbreviations that are likely to change.

---

## 15. Custom Permissions

Use an action/capability statement that is meaningful in formulas, Apex and Flow.

```text
ApproveHighValueDiscount
ManageCustomerCredit
BypassContractValidation
```

For bypass permissions, explicitly state what is bypassed. Avoid generic names such as `AdminOverride` or `BypassValidation`.

---

## 16. Custom Metadata Types

Use Custom Metadata Types for deployable application/configuration metadata and explicit mappings.

Type names should describe the configuration concept:

```text
IntegrationEndpoint__mdt
CountryConfiguration__mdt
RecordTypeMapping__mdt
FeatureConfiguration__mdt
```

Record Developer Names should identify the configured key/context:

```text
SAPCustomer
SAPOrder
Brazil
Argentina
```

Do not store secrets in Custom Metadata. Use Salesforce credential-management capabilities for authentication material.

---

## 17. Custom Settings

Use Custom Settings only when their specific runtime/legacy characteristics are required. For new deployable configuration, evaluate Custom Metadata Types first.

Name Custom Settings for their configuration responsibility, not for their implementation:

```text
LegacyIntegrationSettings__c
```

Document why a Custom Setting is preferred over Custom Metadata when the choice is not obvious.

---

## 18. Named Credentials and External Credentials

Name credentials after the external system or integration responsibility rather than implementation details.

Examples:

```text
SAP
CustomerAPI
PaymentGateway
```

Where separate credentials are required by architecture, make the distinction explicit and stable. Do not include secrets, usernames, URLs or ephemeral values in metadata names.

Environment differences should preferably be handled through deployment/configuration strategy rather than embedded throughout Apex or Flow.

---

## 19. Platform Events

Use a business event in past tense or a domain event concept where practical.

```text
OrderCreated__e
CustomerUpdated__e
PaymentAuthorized__e
ContractExpired__e
```

Event fields use the same field naming standards as custom objects.

Avoid implementation-centric names such as:

```text
SendOrderMessage__e
IntegrationEvent1__e
```

---

## 20. Apex

### 20.1 General convention

Use PascalCase for classes and interfaces. Names must communicate responsibility.

Prefer:

```text
AccountService
AccountSelector
AccountRepository
AccountDomain
AccountTriggerHandler
SAPCustomerClient
SAPCustomerRequest
SAPCustomerResponse
ContractExpirationBatch
ContractExpirationScheduler
CreateContractInvocable
```

Avoid generic containers:

```text
AccountHelper
GeneralUtils
CommonManager
GenericService
```

`Helper`, `Utils`, `Common` and `Manager` are acceptable only when the class has a genuinely cohesive responsibility that cannot be named more precisely.

### 20.2 Services

Convention:

```text
<Domain>Service
```

Examples:

```text
ContractService
CustomerEligibilityService
```

### 20.3 Selectors / repositories

When the architecture uses these patterns:

```text
AccountSelector
ContractRepository
```

Use one data-access vocabulary consistently within an application. Do not mix `Selector`, `DAO` and `Repository` for equivalent responsibilities without architectural justification.

### 20.4 Trigger handlers

```text
AccountTriggerHandler
OpportunityTriggerHandler
```

Keep trigger orchestration thin; business behavior belongs in appropriately scoped services/domain classes.

### 20.5 Integration clients

Convention:

```text
<System><Domain>Client
```

Examples:

```text
SAPCustomerClient
ERPOrderClient
PaymentGatewayClient
```

DTO/request/response objects should state their contract role:

```text
SAPCustomerRequest
SAPCustomerResponse
```

### 20.6 Batch and scheduled Apex

```text
ContractExpirationBatch
ContractExpirationScheduler
```

### 20.7 Queueable Apex

Use the business operation plus `Queueable` where the suffix improves operational discoverability.

```text
CustomerSynchronizationQueueable
InvoiceGenerationQueueable
```

### 20.8 Invocable Apex

```text
CreateContractInvocable
CalculateRiskInvocable
```

### 20.9 Tests

Convention:

```text
<ClassUnderTest>Test
```

Examples:

```text
ContractServiceTest
SAPCustomerClientTest
ContractExpirationBatchTest
```

Test method names should describe behavior and expected outcome rather than sequence numbers.

```text
shouldCreateContractWhenOpportunityIsWon
shouldRejectRequestWhenCustomerIsInactive
```

Avoid:

```text
test1
testMethodA
```

---

## 21. Lightning Web Components (LWC)

LWC bundle names use lower camel case and represent a component responsibility or business UI concept.

```text
customerSummary
contractStatusPanel
orderLineEditor
approvalRequestForm
```

Avoid generic names:

```text
customComponent
component1
commonWidget
```

JavaScript variables and functions use lower camel case:

```text
customerId
isLoading
loadContracts()
handleSave()
```

Use `handle<Event>` for local event handlers when it improves consistency, and use action-oriented names for business functions.

Custom events should describe what happened:

```text
save
selectionchange
contractcreated
```

Maintain consistency within a component library and document public `@api` properties as a component contract.

---

## 22. Aura Components and Visualforce

Treat Aura and Visualforce as existing/compatibility technologies unless their use is architecturally justified. For new UI development, evaluate Lightning Web Components first.

Existing components should not be renamed merely to comply with this standard if the change creates disproportionate dependency or deployment risk.

---

## 23. Email Templates and Alerts

Do not use `ET` or `EA` purely as metadata-type prefixes.

Use the domain, triggering scenario and audience/purpose.

```text
Case_SurveyInvitation
Contract_ExpirationReminder
Opportunity_ApprovalRequested
```

User-facing subjects and template labels may follow the organization's localization strategy.

---

## 24. Reports and Dashboards

Use names that identify the business question or operational purpose rather than the implementation/filter arrangement.

```text
Open Opportunities by Stage
Cases Approaching SLA
Contract Renewals Next 90 Days
```

Organize folders around governed business domains or audiences. Avoid personal initials, temporary project codes and ambiguous folders such as `Misc`, `New Reports` or `Tests` in production.

---

## 25. Queues, Public Groups and Sharing

Names should represent a stable business responsibility or access purpose.

```text
Tier2Support
ContractReviewers
BrazilSalesOperations
```

Avoid embedding employee names in long-lived configuration unless the configuration is intentionally user-specific.

---

## 26. Integration Naming

### 26.1 Source-system ownership

Where data provenance matters, make the owning/source system explicit.

```text
ERPAccountId__c
SAPCustomerId__c
CommerceOrderId__c
```

### 26.2 Integration configuration

Prefer explicit metadata names such as:

```text
IntegrationEndpoint__mdt
IntegrationMapping__mdt
RecordTypeMapping__mdt
```

### 26.3 Integration contracts

Do not expose Salesforce metadata names as external contracts without deliberate versioning and governance. Map external business/system identifiers at the integration boundary.

### 26.4 Secrets

Never place passwords, tokens, client secrets or private keys in names, descriptions, Custom Metadata records, Apex constants or source control. Use supported Salesforce credential and secret-management mechanisms.

---

## 27. Deprecated Prefixes

The following prefixes are **not part of the standard for newly created metadata** when their only purpose is identifying metadata type:

```text
EA    Email Alert
ET    Email Template
WR    Workflow Rule
FU    Field Update
CL    Compact Layout
LP    Lightning Page
PB    Process Builder
flow  Flow
PS    Permission Set
RT    Record Type
```

Existing metadata does not need to be renamed solely for compliance. Rename only when the maintenance/readability benefit exceeds dependency and deployment risk.

---

## 28. Source Control and CI/CD

Metadata naming is not a substitute for deployment tooling or artifact discovery.

All production-bound metadata should be managed through the organization's source-driven delivery process using, as applicable:

- Salesforce DX project/source format;
- Git as the version-control system;
- pull requests;
- peer review for declarative and programmatic changes;
- Salesforce deployment validation;
- automated Apex tests;
- static analysis and organization-specific naming checks;
- traceability to a Story, defect or change request.

Naming rules that can be evaluated deterministically should be automated rather than enforced only through reviewer memory.

Example pipeline:

```text
Pull Request
  -> Metadata validation
  -> Static analysis
  -> Naming validation
  -> Salesforce validation deployment
  -> Automated tests
  -> Review approval
  -> Merge
```

---

## 29. Pull Request Checklist

### Naming

- [ ] API/Developer Names are in English.
- [ ] Names represent business meaning or technical responsibility.
- [ ] No unnecessary metadata-type prefix was introduced.
- [ ] No data-type prefix/suffix was introduced for fields.
- [ ] Names are not unnecessarily truncated.
- [ ] Acronyms follow the organization's convention consistently.
- [ ] Object context is not redundantly repeated.

### Documentation

- [ ] Important fields contain a useful Description.
- [ ] User-facing fields contain Help Text where users need guidance.
- [ ] Flows explain purpose, invocation and major dependencies.
- [ ] Non-obvious architecture decisions link to a Story/ADR or equivalent record where appropriate.

### Automation

- [ ] New automation uses Flow rather than Workflow Rule or Process Builder.
- [ ] The Flow has a focused responsibility.
- [ ] Element/resource names are semantic and consistent.
- [ ] Decision outcomes read as meaningful conditions.
- [ ] Fault paths and operational/error handling have been considered.

### Security

- [ ] Access is assigned using the organization's Permission Set / Permission Set Group strategy.
- [ ] New permissions represent cohesive capabilities.
- [ ] No secrets or sensitive credentials were committed to metadata/source.
- [ ] Bypass mechanisms use explicit Custom Permissions and are documented.

### Integration

- [ ] External identifiers identify their owning/source system when needed.
- [ ] Record Type Developer Names are not being used as an accidental external contract.
- [ ] Environment-specific configuration is externalized appropriately.
- [ ] Named/External Credentials or the approved credential mechanism is used for authentication.

### Delivery

- [ ] Metadata is committed to Git in the standard Salesforce project structure.
- [ ] Salesforce validation deployment succeeds.
- [ ] Automated tests pass.
- [ ] Static-analysis/quality gates pass.
- [ ] The change is traceable to its business/technical requirement.

---

## 30. Quick Reference

### Custom metadata

```text
Object                 ContractRequest__c
Field                  ExpirationDate__c
External ID            ERPContractId__c
Record Type            ShippingCompany
Validation Rule        EndDateMustBeAfterStartDate
Custom Metadata Type   IntegrationMapping__mdt
Platform Event         OrderCreated__e
```

### Flow

```text
Record-triggered       Case_AfterSave_SendSurvey
Screen Flow            Case_CreateReturnRequest
Scheduled Flow         Contract_Scheduled_SendExpirationReminder
Subflow                Customer_CalculateRisk
Get Records            GetActiveContracts
Create Records         CreateCase
Update Records         UpdateOpportunity
Assignment             AssignDefaultValues
Decision               EvaluateEligibility
Decision Outcome       CustomerIsEligible
Loop                    LoopContractItems
Variable                customerNumber
Boolean variable        isEligible
Collection              accounts
```

### Security

```text
Permission Set         Contract_Manage
Permission Set Group   SalesManager
Custom Permission      ApproveHighValueDiscount
```

### Apex

```text
Service                ContractService
Selector               AccountSelector
Repository             ContractRepository
Trigger Handler        AccountTriggerHandler
Integration Client     SAPCustomerClient
Request DTO            SAPCustomerRequest
Batch                   ContractExpirationBatch
Scheduler               ContractExpirationScheduler
Queueable               CustomerSynchronizationQueueable
Invocable               CreateContractInvocable
Test                    ContractServiceTest
```

### LWC

```text
Bundle                  customerSummary
Property                customerId
Boolean                 isLoading
Method                   loadContracts
Handler                  handleSave
```

---

## 31. Migration from Version 1.x

Adoption of this standard does **not** require mass-renaming existing metadata. Renaming Salesforce metadata can introduce dependency, source-history, deployment and integration risk.

Use the following strategy:

1. Apply Version 2.0 to all new metadata.
2. Apply Version 2.0 when materially modifying an existing component if renaming is low risk.
3. Do not rename stable metadata merely for cosmetic consistency.
4. Inventory Workflow Rules and Process Builder processes and plan migration to Flow.
5. Remove deprecated prefix conventions from templates and project documentation for new development.
6. Introduce automated checks gradually, beginning with deterministic rules such as prohibited legacy automation, naming patterns and required source-validation gates.
7. Record justified exceptions in code review or an Architecture Decision Record (ADR).

---

## 32. Exceptions

A standard should improve architecture, not create unnecessary risk. An exception is acceptable when required by:

- Salesforce platform constraints;
- installed/managed package conventions;
- an established external integration contract;
- backward compatibility;
- regulatory or organizational standards;
- materially safer deployment/migration behavior.

Exceptions must be intentional and documented. Existing metadata is not automatically non-compliant merely because it predates Version 2.0.

---

## 33. References

The Version 2.0 standard is informed by Salesforce's current platform direction and architecture guidance, including:

- Salesforce Well-Architected Framework.
- Salesforce Well-Architected Operational Excellence / DevOps patterns.
- Salesforce Flow best-practice guidance published by Salesforce Admins.
- Salesforce Migrate to Flow guidance.
- Salesforce notice that Workflow Rules and Process Builder are no longer supported after December 31, 2025.

Reference URLs:

```text
https://architect.salesforce.com/docs/architect/well-architected/guide/framework.html
https://architect.salesforce.com/docs/architect/well-architected/guide/operational-excellence-devops-patterns
https://admin.salesforce.com/blog/2021/the-ultimate-guide-to-flow-best-practices-and-standards
https://help.salesforce.com/s/articleView?id=platform.flow_migrate_to_flow.htm&type=5
https://help.salesforce.com/s/articleView?id=001096524&type=1
```

---

## Change Log

### Version 2.0

- Formalized seven guiding principles.
- Standardized custom object/field naming around semantic PascalCase.
- Relaxed the underscore rule for technical metadata where separators improve readability.
- Removed metadata-type prefixes as a default convention.
- Reworked Record Type and relationship naming.
- Replaced Process Builder/Workflow Rule guidance with a legacy/migration policy.
- Rebuilt Flow naming for record-triggered, screen, scheduled and autolaunched flows.
- Added Flow element, outcome, resource, formula and variable conventions.
- Added Permission Set, Permission Set Group and Custom Permission standards.
- Added Custom Metadata, credential and integration conventions.
- Added Apex and LWC conventions.
- Added Platform Event, report/dashboard, queue/group and security guidance.
- Added source-control, CI/CD, PR checklist, migration and exception policies.
