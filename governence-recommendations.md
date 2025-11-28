GitHub Enterprise Policies & Best Practices
Abhishek Dutta
·
@abhi-dutta
July 29, 2024
|
Updated August 5, 2025
Scenario overview 
Adopting GitHub’s general platform best practices is crucial for effectively managing software development projects. These practices provide a structured framework that ensures projects are secure, maintainable, and scalable. By fostering a collaborative and efficient environment, these guidelines help organizations avoid common pitfalls, maintain high-quality codebases, and streamline project workflows. Ultimately, adhering to these best practices leads to successful and sustainable software projects.

Key design strategies and checklist 
Use these key strategies as a baseline to implement GitHub’s best practices for governance:

Restrict Actions execution to specific repositories and not let repo users and owners to arbitrarily create and execute actions workflow. Set this at the Organization level.

Use Actions created by GitHub and Verified Creators wherever applicable. This settings can be enforced at Enterprise level.

Default Workflow token permission should be read-only: Following the principle of least privilege, it is recommended to set the default workflow token permissions to read-only. Default is read/write, which should be avoided because if a token is compromised, malicious actors can run exploits within the GitHub platform through actions execution.

Disable auto approval of Pull Requests. By default it is enabled, but it might lead to attackers bypassing code reviews for pull requests and thus approving and merging pull requests unintentionally. This can be set at either the Enterprise or Organizational level.

Disable forking if not absolutely needed: You can help prevent sensitive information from being exposed by disabling the ability to fork repositories in your organization.

Disable changing repository visibility: You should restrict who has the ability to change the visibility of repositories in your organization, such as changing a repository from private to public. Restricting who has the ability to change the visibility of repositories in your organization helps prevent sensitive information from being exposed.

Implement approval flow for fine grained personal access tokens. If enabled at the Enterprise or Organization level, all organization members must request to get approval for their fine-grained personal access tokens that access any organization in your enterprise. This will help in reviewing which users are accessing what resources and with permissions on the PAT.

Enterprise owners should create a policy to prevent org members from inviting outside collaborators. Default is “No Policy”. Recommended is Enterprise owners or Organization Owners, so that there is centralized control on who can invite and vet outside collaborators.

Set up an Enterprise policy which which prevents users and members from creating public repositories.

Webhooks should always be configured with a secret. Not using a secret in the webhook will mislead the receiving entity of the webhook on the authenticity of the payload received.

Webhooks should be configured to use SSL.

Always use Repository Rulesets with fine grained policy enforcements around checks needing pull request review, required checks, and leverage protected branches.image1

Define CODEOWNERS: To protect a repository against unauthorized changes, you also need to define owners using a CODEOWNERS file. The most secure method is to define a CODEOWNERS file in the .github directory of the repository and define the repository owner as the owner of either the CODEOWNERS file (/.github/CODEOWNERS @owner_username) or the whole directory (/.github/ @owner_username).

Initiate and impose commit signing whenever possible. This will deter malicious actors from creating a commit with malicious code and help prevent a possible supply chain attack.

Bypass of rulesets should not be allowed under the Repository Ruleset configuration. Enforcing policies around repo ruleset is designed for a reason and allowing users to bypass rulesets might allow an attacker to gain access as a user who is allowed to bypass ruleset and compromise the integrity of the codebase.

Runner groups should be limited to a select number of repositories. Configuring a runner group for all repositories can expose vulnerabilities or allow malicious actors to exploit misconfigured runners. Additionally, maintaining runner groups for specific repositories ensures that self-hosted runners are used for their intended specialized workloads. Granting access to everyone in the organization can lead to wasting resources on unnecessary execution tasks.

Bypass Push Protection: By default, anyone with write access can bypass a commit blockage due to protection. Considering the criticality of leaked secrets, it is recommended to have bypass capability limited to spefic roles and teams.image2

Audit log streaming is often overlooked by enterprises when implementing GitHub. Streaming audit logs from GitHub is crucial for monitoring platform usage trends. The extensive data captured in these logs provides valuable insights into potential unwanted activities occurring on the platform.

Seeking further assistance 
GitHub Support 
Visit the GitHub Support Portal for a comprehensive collection of articles, tutorials, and guides on using GitHub features and services.

Can’t find what you’re looking for? You can contact GitHub Support by opening a ticket.

GitHub Expert Services 
GitHub’s Expert Services Team is here to help you architect, implement, and optimize a solution that meets your unique needs. Contact us to learn more about how we can help you.

GitHub Partners 
GitHub partners with the world’s leading technology and service providers to help our customers achieve their end-to-end business objectives. Find a GitHub Partner that can help you with your specific needs here.

GitHub Community 
Join the GitHub Community Forum to ask questions, share knowledge, and connect with other GitHub users. It’s a great place to get advice and solutions from experienced users.

Related links 
GitHub Documentation 
For more details about GitHub’s features and services, check out GitHub Documentation.


Custom Properties Best Practices
Collin McNeese
·
@collinmcneese
September 2, 2025
|
Updated September 5, 2025
Scenario overview 
Organizations need consistent metadata to govern repositories at scale, track compliance requirements, and make informed decisions about their codebase. GitHub repository custom properties provide structured metadata that enables automated governance, improves visibility across repositories, and supports regulatory compliance requirements.

Key design strategies and checklist 
Define your metadata schema
Identify governance problems custom properties will solve (compliance tracking, ownership, criticality classification).
Design property types that support automated decision-making and policy enforcement.
Plan for organizational growth and potential schema evolution.
Start with core governance properties
Begin with properties that address immediate pain points (owner identification, business criticality).
Focus on metadata that enables automation rather than just manual reporting.
Establish property governance
Create RACI matrix for property definition, updates, and enforcement.
Document property meanings, valid values, and usage guidelines.
Automate property management
Use APIs and scripts to populate properties at repository creation.
Configure required properties that must be set during repository creation workflow.
Implement validation and drift detection to maintain data quality.
Integrate with policy enforcement
Connect custom properties to repository rulesets for automated governance.
Use properties to trigger different workflows and security policies.
Plan for enterprise-wide consistency
Start with organization-level properties and promote to enterprise level when patterns emerge.
Balance central control with team autonomy for property values.
Quick checklist 
Area	Action
Schema Design	Property schema documented; governance use cases defined
Property Coverage	≥90% active repositories have required properties populated
Automation	API scripts for property management and validation
Integration	Properties integrated with rulesets and workflow automation
Documentation	Property meanings and usage guidelines published
Compliance	Audit trail for property changes via API logging
Assumptions and preconditions 
Clear understanding of organizational governance requirements and compliance frameworks.
Agreement on repository classification and metadata taxonomy.
Plan availability for custom properties - GitHub allows up to 100 property definitions per organization.
Required roles:
Organization owners or custom role with the ability to manage repository custom properties for organization-level properties
Enterprise owners for enterprise-level property management
Repository users with appropriate permissions for setting property values (if delegated).
Recommended implementation 
1. Core property schema 
Define properties that address your most pressing governance needs. Start with a minimal set and expand based on proven value. The following table provides example properties that many organizations find useful, but you should customize based on your specific governance requirements:

Property	Type	Purpose	Example Values
business-criticality	single-select	Risk assessment and policy targeting	Critical, High, Medium, Low
owner-team	string	Responsibility and escalation	“platform-team”, “customer-experience”
compliance-frameworks	multi-select	Regulatory requirement tracking	SOX, HIPAA, GDPR, PCI, FedRAMP
data-classification	single-select	Data handling requirements	Public, Internal, Confidential, Restricted
environment	single-select	Deployment tier classification	Production, Staging, Development, Sandbox
public-facing	true_false	External exposure assessment	true, false
For public repositories, avoid properties that could expose sensitive organizational information. Consider using generic classifications rather than specific internal project names or security postures.
Example property configurations for different repository types:

# Financial services repository (SOX, PCI DSS)
compliance-frameworks: ["SOX", "PCI-DSS"]
data-classification: "Restricted"
business-criticality: "Critical"
environment: "Production"
public-facing: "true"

# Platform service repository
service-tier: "Platform"
business-criticality: "Critical"
owner-team: "platform-ops"
environment: "Production"

# Legacy modernization candidate
tech-stack: ["Java", "Spring Boot"]
architecture-pattern: "Monolith"
modernization-candidate: "true"
migration-priority: "High"
business-criticality: "Medium"

2. Integration with repository rulesets 
Connect custom properties to automated policy enforcement through repository rulesets. While rulesets define the “what” of your governance policies, custom properties enable the “when” and “where” - allowing you to apply different governance rules based on repository characteristics rather than manually managing which repositories get which rules.

Read more on working with rulesets in repository rulesets best practices.
This integration solves common governance challenges:

Scalable policy application: Instead of manually adding repositories to rulesets, properties enable dynamic targeting. As your organization grows from dozens to hundreds of repositories, this automation becomes essential.
Context-aware enforcement: A development sandbox repository may not need to have the same security requirements as a production payment processing service. Properties allow rulesets to automatically adjust enforcement based on risk level, compliance requirements, and operational context.
Compliance audit trails: Properties create clear documentation of why certain repositories have specific security controls, making it easier to demonstrate compliance during audits.
Production Repository Ruleset Targeting:

Target repositories where environment = "Production" OR business-criticality = "Critical"
Apply enhanced security requirements: required approvals, code scanning, deployment gates
Compliance Framework Enforcement:

Target repositories where compliance-frameworks includes “SOX” or “HIPAA”
Enforce signed commits, restrict file paths, require security workflows
Public-Facing Application Controls:

Target repositories where public-facing = true
Require additional security scanning, dependency review, and deployment validation
3. Integration with repository policies 
Custom properties enable flexible targeting for repository policies that govern repository lifecycle events such as creation, deletion, naming, and visibility changes. These policies can be implemented at both organization and enterprise levels for scalable governance.

When custom properties are marked as required for repository creation, teams must set these properties during the repository setup process, which may influence repository naming and visibility choices.

Example policy targeting:

Prevent repositories with data-classification = "Restricted" from being made public
Require repositories with service-type = "microservice" to use svc- prefix for operational clarity
Restrict deletion of repositories where environment = "Production" to organization owners only
4. Reporting and monitoring 
Implement monitoring to track property coverage and identify governance gaps. Without visibility into property adoption and accuracy, organizations face several practical challenges:

Compliance blind spots: During regulatory audits, organizations need to quickly demonstrate which repositories handle regulated data and what controls apply. Missing or incorrect properties can result in audit findings or unnecessary remediation work on misclassified repositories.
Security incident response: When vulnerabilities affect specific technologies or components, security teams need to rapidly identify all repositories using those technologies. Without accurate technical metadata, teams spend valuable time manually inventorying affected systems during incidents.
Governance drift: Properties assigned during repository creation often become outdated as projects evolve. A repository’s risk profile or operational context may change over time, requiring different governance controls. Regular monitoring helps identify these changes before they create compliance or security gaps.
An example method for scanning repository properties involves using the GitHub API to retrieve custom property values for all repositories in an organization, then identifying those that are missing required properties or have outdated values.

gh api orgs/ORG/properties/values --paginate | jq -r '
  .[] |
  "\(.repository_name),\((.properties[] | select(.property_name == "business-criticality") | .value) // "MISSING"),\((.properties[] | select(.property_name == "owner-team") | .value) // "MISSING")"
' | column -t -s,

For more targeted analysis, you can use the repository_query parameter to focus on specific repositories. For example, to find only repositories missing the business-criticality property:

gh api 'orgs/ORG/properties/values?repository_query=no:props.business-criticality' --paginate | jq -r '
  .[].repository_name
'

Additional solution detail and trade-offs to consider 
Choosing the right scope for your properties 
When implementing custom properties, one of the first decisions is whether to start at the organization level or think enterprise-wide from the beginning.

Start with organization-level properties when:

Piloting the approach or working within a single business unit
You need to iterate quickly and test different property schemas
Proving value before expanding to other organizations
Working with smaller, more agile teams
Move to enterprise-level properties when:

You need consistency across multiple organizations
Compliance requirements demand standardized metadata
You have proven patterns that work across business units
Centralized governance becomes more important than speed
Keep in mind that enterprise-level properties require more coordination and slower change management - you’ll need buy-in from multiple stakeholders before making schema changes.

Managing who can set property values 
The question of who controls property values significantly impacts adoption and data quality.

Centralized control works well for compliance-critical properties where consistency matters more than speed:

Governance teams manage values for properties like data-classification
Ensures accuracy and regulatory compliance
May create bottlenecks for new repositories
Best for highly regulated environments
Delegated management increases adoption and reduces friction:

Repository teams can set operational metadata
Requires clear documentation about property meanings
Works well for team ownership, project status, tech stack
Risk of inconsistent or incorrect metadata
Hybrid approach (most common):

Governance teams control: compliance properties (regulatory frameworks, data classification)
Teams self-manage: operational properties (environment designation, tech stack)
Requires clear boundaries and good documentation to prevent confusion
Maintaining data quality over time 
Properties become stale as repositories evolve. A repository might start as a proof-of-concept but eventually handle production traffic, changing its risk profile entirely.

Automated validation helps catch obvious problems:

Use GitHub Actions workflows to check properties during pull requests or deployments
Validate missing required properties, invalid values, or unclassified repositories
Generate alerts for properties that need attention
Regular auditing is equally important but often overlooked:

Schedule regular reviews to compare property values against reality
Check whether repositories marked as “Development” are actually handling production workloads
Verify properties align with external systems (CMDB, service catalogs)
Update properties based on organizational changes
Start simple. Begin with 5-6 essential properties that solve real problems rather than trying to capture every possible piece of metadata. You can always expand later once the foundational patterns are working well.

Avoid creating too many properties initially. Start with essential properties and expand based on proven value. Too many properties can create maintenance overhead and reduce adoption.
Seeking further assistance 
GitHub Support 
Visit the GitHub Support Portal for a comprehensive collection of articles, tutorials, and guides on using GitHub features and services.

Can’t find what you’re looking for? You can contact GitHub Support by opening a ticket.

GitHub Expert Services 
GitHub’s Expert Services Team is here to help you architect, implement, and optimize a solution that meets your unique needs. Contact us to learn more about how we can help you.

GitHub Partners 
GitHub partners with the world’s leading technology and service providers to help our customers achieve their end-to-end business objectives. Find a GitHub Partner that can help you with your specific needs here.

GitHub Community 
Join the GitHub Community Forum to ask questions, share knowledge, and connect with other GitHub users. It’s a great place to get advice and solutions from experienced users.

Related links 
GitHub Documentation 
For more details about GitHub’s features and services, check out GitHub Documentation.

Specific custom properties documentation can be found here:

Managing custom properties for repositories in your organization
Managing custom properties for repositories in your enterprise
Using the API to manage custom properties
Repository search syntax


Rulesets Best Practices
Ari LiVigni
, 
Greg Mohler
, 
Greg Ose
 & 
Collin McNeese
October 8, 2024
|
Updated October 3, 2025
Scenario overview 
Enterprises need consistent, enforceable guardrails for how code enters, evolves within, and is released from repositories. GitHub repository rulesets and custom properties let you express policy once and apply it broadly while still allowing controlled exceptions.

Key design strategies and checklist 
Define ownership & scope
Decide what lives at the enterprise / organization ruleset layer vs. delegated repository rulesets.
Create a RACI matrix (owner for design, approver for enforcement changes, reviewers, informed dev community).
Start with a minimal baseline ruleset in Evaluate mode
Use Evaluate to surface potential merge/push friction before enforcement.
Track violations (Rule Insights) to prioritize developer enablement.
Tier your protection
Map repositories to tiers via custom properties (e.g. production=true, security_tier=high|medium|low).
Attach progressively stronger rulesets per tier (baseline → production → sensitive).
Centralize critical workflows
Use required workflows and status checks for organization-wide gates.
Keep them in internal reusable workflow repos and version them.
Bypass & exception model
Grant bypass only to roles/teams with clear break-glass standard operating procedures.
Monitor bypass exceptions via the audit log, REST API, webhooks, or the native rule insights dashboard; look for patterns indicating a need to adjust rules.
Change management & versioning
Rulesest history is retained for 180 days; you can view all the changes to a ruleset and revert back to a specific iteration.
Measurement & feedback
Metrics: % repos covered per tier, # blocked events by rule, mean time to remediate violation patterns, bypass frequency.
Use rule insights to adjust high-friction rules.
Leverage curated recipes safely
Refer to GitHub’s starter workflows (treat as starting points; review & adapt).
Phase your rollout
Test phase - Start with a ruleset in evaluate mode, targeting only your test repos to confirm functionality works as intended.
Pilot phase - Roll out to specific pilot teams by selecting several named repos; validate results and incorporate feedback as needed.
Expand phase - Expand to the entirety of the tier you’re targeting (all repos or dynamic by property); review rule insights and adjust rules where failures occur.
Enforce phase - Move rule to active mode where requirements become blocking controls; continue monitoring failures/bypasses and adjust rules as needed.
Documentation & training
Create a lightweight internal page including required rules per tier, how to request an exception, and how to interpret a failed push.
Developers should know about new rules before they’re live, especially when those rules could block their workflow.
Quick checklist 
Area	Action
Governance	RACI defined; approval workflow documented
Targeting	Custom properties applied to ≥90% active repos
Rollout	Baseline ruleset in Evaluate ≥1 sprint before enforce
Exceptions	Audit log review cadence (weekly)
Automation	tools/export & drift detection in CI
Metrics	Dashboard for coverage, violations, bypasses
Assumptions and preconditions 
Agreement on taxonomy for repository tiers and custom property keys/values.
GitHub Actions reusable workflows producing stable status checks before you enforce them.
Plan availability for rulesets - refer to GitHub Docs: Available rules for rulesets.
Required roles:
Organization owners or custom role with “Manage organization ref update rules and rulesets” for org-level rulesets
Repo admins or custom role with “Edit repository rules” for repo-level rulesets
Custom properties managed by owners or delegated roles.
Recommended implementation 
1. Custom properties strategy 
Read more on working with custom properties in custom properties best practices.
Define a schema that makes sense for your organization. For example:

Property	Type	Purpose
production	boolean	Attach production ruleset
security_tier	single-select (Low, Medium, High)	Progressive enforcement
compliance	multi-select (SOC 2, FedRAMP, HIPAA, PCI, etc.)	Target additional workflows
app_id	text	Link to CMDB / inventory
Populate on repo creation and automate updates via API to prevent drift.

For public repositories, avoid custom properties that could expose sensitive information (e.g. internal project names, security posture).
2. Baseline ruleset (all active repos) 
Protect main development branches with minimal developer friction. Ideal for repositories that are small internal projects led by a single developer or initial proof of concept work that is being iterated on before being deployed to production environments.

Recommended rules include (see Available rules for rulesets for definitions):

Branch rules:

Block force pushes & deletions - These protect against accidental or malicious destruction of commit history, ensuring code changes remain auditable and recoverable. Force pushes can overwrite colleagues’ work and break CI/CD pipelines that rely on stable commit references.
Require a pull request before merging - This establishes a minimal review gate that creates visibility into all changes, enables automated checks to run, and provides an opportunity for knowledge sharing even if formal approval isn’t required at this tier.
Require workflows to pass (e.g. central security/compliance workflow once stable inclusive of Dependency Review) - Automated security scanning catches common vulnerabilities before they enter the main branch, reducing remediation costs. Dependency Review specifically prevents introduction of known-vulnerable packages that could compromise the entire software supply chain.
3. Production tier ruleset 
Applied where repository supports a production application (e.g. custom property production=true). Add/tighten:

Branch rules:

Additional pull request requirements:
Required approvals ≥ 1 - Ensures at least one peer reviews changes to reduce defects and spread knowledge before merging.
Dismiss stale pull request approvals when new commits are pushed - Prevents outdated approvals from being applied to revised code, ensuring reviewers validate the final set of changes.
Require review from CODEOWNERS - Ensures domain experts and maintainers validate changes to sensitive or critical areas.
Require approval of the most recent reviewable push - Guarantees that approvals correspond to the latest reviewed commit.
Require conversation resolution before merging - Ensures reviewer feedback is addressed and reduces the risk of overlooked issues.
Automatically request Copilot code review - Adds an automated review layer to surface potential issues quickly and at scale.
Require code scanning results (severity threshold tuned to avoid overly cautious blocks) - Integrates automated security scanning (e.g. CodeQL) to catch vulnerabilities prior to merging while tuning thresholds to reduce noisy false positives.
4. Sensitive / regulated tier ruleset 
Organizations subject to compliance frameworks like SOX, HIPAA, PCI DSS, FedRAMP, or SOC 2 often need to demonstrate security controls during audits, including evidence that code changes follow proper authorization and review processes before affecting production systems.

Consider these rules where a repository supports sensitive data or regulated applications (e.g. custom property security_tier=high). Add/tighten:

Branch rules:

Require signed commits (if developer signing adoption ≥80%) - Provides stronger provenance and non-repudiation for commits, supporting audit and compliance requirements.
Require deployments to succeed before merging via required status checks - Ensures that changes pass deployment validation and do not introduce regressions into production systems.
Consider additional required workflows:
Additional security scanning (IaC analysis/DAST) - Detects infrastructure and runtime vulnerabilities that basic code scanning may miss.
Secret scanning bypass detection (e.g. Secret Scanning Review Action) - Helps detect attempts to circumvent secret scanning and prevents sensitive data leakage.
License compliance and security checks with Dependency Review - Prevents introducing dependencies with known vulnerabilities or overly restrictive licenses.
Restrict metadata (e.g. author email domain) - Ensures that valid employee accounts are used to suggest changes.
Push rules:

Restrict file paths (e.g. .github/secret_scanning.yml or high-risk paths) - Limits the surface area for accidental or malicious changes to critical configuration or secret scanning policies.
Restrict file extensions (e.g. binaries) - Prevents large or executable files from being added without proper review, reducing risk and repository bloat.
Restrict file size (e.g. oversized artifacts) - Prevents oversized files that can hinder repo performance or be used as an exfiltration vector.
5. Exception & bypass flow 
Consider your bypass actors carefully to avoid accidental “impossible merge” situations. For example, when a required workflow is unavailable, when an external status check cannot complete, or when a single-person team has no peer available to review changes. Audit all bypass events so recurring patterns can guide rule adjustments instead of repeated bypasses.

When bypass actors have been configured, the general flow is:

Developer opens a PR or push is blocked.
Developer checks internal guidance (ideally linked in the organization README).
If emergency: authorized role uses bypass.
Ruleset owners perform weekly review; adjust rules if recurring pattern emerges.
Additional solution detail and trade-offs to consider 
Finding the Right Balance 
Finding the right balance between enterprise, organization, and repository level rulesets depends on your specific project and team needs. Key factors to consider:

Control: Trade-off between central control and distributed repo-level flexibility
Complexity: Frequency and complexity of rule changes; complex rules may be safer at smaller scale where change impact is limited
Risk impact: Strict security or compliance requirements can be better defined at enterprise/organization level where repository owners have less administrative control or bypass ability
Commonality: Level of consistency and standardization required across repositories
Bypass exceptions: Enterprise/organization level rulesets benefit from role-type bypass exceptions (org/repo admins), while explicit actor bypasses should be more tightly scoped
Some repositories benefit from inheriting most rules from the enterprise/organization level, while others need more customization and autonomy. A hybrid approach with selective exceptions and overrides may suit specific cases. The goal is maximizing ruleset benefits without compromising code productivity and quality.

Implementation Approaches 
Minimal Inheritance: Define only essential high-level rules (code quality, security, compliance) at enterprise/organization level. Leave remaining rules to repository owners and teams for maximum customization and autonomy.
Strict Inherited Enforcement: Define and enforce comprehensive rules across all repositories to ensure enterprise-wide uniformity and alignment. Reduces risk of errors and deviations but limits team flexibility and creativity.
Micro Rulesets: Define multiple rule sets targeting different repository groups based on project type, domain, or function. Enables granular specificity while allowing differentiation and adaptation.
Avoid enabling too many restrictive rules simultaneously; stage them and monitor friction. Push rulesets can block legitimate automation if not tested; pilot them first.
Seeking further assistance 
GitHub Support 
Visit the GitHub Support Portal for a comprehensive collection of articles, tutorials, and guides on using GitHub features and services.

Can’t find what you’re looking for? You can contact GitHub Support by opening a ticket.

GitHub Expert Services 
GitHub’s Expert Services Team is here to help you architect, implement, and optimize a solution that meets your unique needs. Contact us to learn more about how we can help you.

GitHub Partners 
GitHub partners with the world’s leading technology and service providers to help our customers achieve their end-to-end business objectives. Find a GitHub Partner that can help you with your specific needs here.

GitHub Community 
Join the GitHub Community Forum to ask questions, share knowledge, and connect with other GitHub users. It’s a great place to get advice and solutions from experienced users.

Related links 
GitHub Documentation 
For more details about GitHub’s features and services, check out GitHub Documentation.

Specific ruleset and custom properties documentation can be found here:

Managing rulesets for repositories in your organization
Available rules for rulesets
Troubleshooting rules
Managing custom properties for repositories in your organization
Audit log events for rulesets
