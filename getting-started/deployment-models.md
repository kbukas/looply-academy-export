# Deployment Models

Looply offers a range of flexible deployment models designed to meet the unique needs of enterprises across different industries, regulatory environments, and technical landscapes. All models are built on the same cloud-native, serverless core — allowing for infinite scalability, low operational overhead, and strong security by design.

This guide outlines each deployment model in detail, including technical ownership, use cases, and operational benefits.

***

### 1. Looply Public Cloud - Multi-Tenant SaaS&#x20;

Overview

This is the default and most common Looply deployment. Your organization is provisioned as a logically isolated tenant within our managed multi-tenant environment, hosted on AWS in the EU (Ireland) region.

Looply handles everything: provisioning, updates, scaling, backups, and security. All infrastructure is fully managed.

#### Ideal For

* Organizations that prefer a plug-and-play SaaS solution
* Enterprises with no internal DevOps or AWS teams
* Customers who need fast time-to-value

#### Benefits

* No infrastructure setup or maintenance
* Rapid onboarding — be live in hours
* Secure, enterprise-grade hosting with automatic scaling
* Patching and upgrades managed entirely by Looply
* Full role-based access control and audit logs included

#### Customer Responsibilities

* Sign up for Looply and invite team members
* Connect Microsoft Entra ID (Azure AD) tenant
* Install SAP ABAP add-on and expose required endpoints

***

### 2. Looply Private Cloud - Looply Managed

#### Overview

This model offers a balance between complete isolation and a managed service. Looply creates and manages a dedicated AWS sub-account on your behalf. The environment is exclusively yours but operated and monitored by Looply.

This gives you many of the benefits of private cloud with none of the infrastructure maintenance burden.

#### Ideal For

* Enterprises who want tenant isolation without taking on DevOps responsibilities
* Regulated organizations requiring dedicated resource control
* Customers scaling across business units or geographies

#### Benefits

* Dedicated AWS account, isolated from all other tenants
* Managed by Looply, including updates, observability, and maintenance
* Custom AWS region or security configuration available on request
* No infrastructure management required by customer

#### Customer Responsibilities

* Customers are required to complete all responsibilities listed under the Multi-Tenant SaaS model.
* Approve account provisioning and region selection
* Coordinate access for any enterprise-specific tools (optional)

***

### 3. Customer-Owned Private Cloud - Looply Managed

#### Overview

Looply can be deployed directly into your AWS account. In this model, Looply provides you with infrastructure templates and deployment tooling to provision and manage the environment within your own cloud footprint.

You maintain full visibility and control over the infrastructure while still leveraging Looply's cloud-native capabilities.

#### Ideal For

* Enterprises with strict internal policies requiring all systems to be hosted in their own AWS accounts
* Teams with DevOps maturity and AWS management capabilities
* Customers with custom networking, logging, or compliance tooling

#### Benefits

* Full infrastructure control (IAM, VPC, logging, observability)
* Deploy Looply alongside other internal workloads or data lakes
* Integrate with your enterprise monitoring and incident response stack
* Optional customization of VPC and subnet structure

#### Customer Responsibilities

* Customers are required to complete all responsibilities listed under the Multi-Tenant SaaS model.
* Maintain the underlying AWS environment (costs, IAM, upgrades)

#### Looply Responsibilities

* Deploy using Looply-provided CloudFormation or Terraform templates
* Provide infrastructure-as-code templates and versioning
* Push platform updates, security patches, and major features
* Provide integration support during provisioning and go-live

***

### Shared Benefits Across All Models

Regardless of the deployment model, all Looply instances come with:

* Auto-scaling, serverless architecture&#x20;
* Microsoft 365 and SAP integration capabilities
* Secure token-based authentication
* Real-time notifications, adaptive card support, and visual workflow builder
* Developer-friendly, no-code/low-code interface
* Monitoring, error handling, and audit logging by default

***

### Choosing the Right Deployment Model

Here’s a quick guide to help you select the best fit:

{% hint style="info" %}
Scroll horizontally to compare all deployment models listed the table below
{% endhint %}

<table><thead><tr><th width="336">Features</th><th width="491">Looply Public Cloud (Multi-Tenant, Looply-Managed)</th><th width="491">Looply Private Cloud (Dedicated Tenant, Looply-Managed)</th><th width="491">Looply Private Cloud (Customer-Managed)</th></tr></thead><tbody><tr><td>Who Manages Infrastructure?</td><td>Looply</td><td>Looply</td><td>Customer</td></tr><tr><td>Who Owns AWS Account?</td><td>Shared (Looply’s AWS environment)</td><td>Dedicated AWS account per customer (Managed by Looply)</td><td>Customer-provided AWS account</td></tr><tr><td>Best For</td><td>Businesses seeking cost-effective, fully managed SaaS with strong security</td><td>Enterprises needing compliance, data isolation &#x26; fully managed infrastructure</td><td>Highly regulated enterprises needing full control over infra</td></tr><tr><td>Data Isolation</td><td>Logical isolation via IAM, DynamoDB partitioning, and API security</td><td>Full physical isolation(dedicated AWS resources)</td><td>Full physical isolation (customer controls AWS setup)</td></tr><tr><td>Data Residency Control</td><td>Limited region support predefined by Looply (customer can choose from available regions)</td><td>Customer can choose any AWS region</td><td>Customer can choose any AWS region</td></tr><tr><td>Compliance Readiness</td><td>Secure by design built on SOC2/ ISO 27001 compliant AWS infrastructure</td><td>SOC 2 / ISO 27001 compliance-ready, with dedicated infrastructure</td><td>Customer responsible for compliance certification</td></tr><tr><td>Network Security</td><td>Multi-tenant architecture with IAM-based access controls, API Gateway protection, and AWS WAF. All traffic is encrypted in transit.</td><td>Dedicated VPC per customer, PrivateLink for internal AWS service access, tenant-specific IAM roles, and encrypted communications.</td><td>Dedicated VPC per customer, PrivateLink for internal AWS service access, customer-controlled network security policies, and encrypted communications.</td></tr><tr><td>Outbound API Access</td><td>Shared NAT Gateway with static IP for whitelisting</td><td>Dedicated NAT Gateway per customer with static IP, with optional PrivateLink-based outbound API access for enhanced security</td><td>Dedicated NAT Gateway per customer with static IP. Customers can choose to restrict outbound internet access entirely using VPC PrivateLink, ensuring private-only API connectivity</td></tr><tr><td>Security Model</td><td>IAM-based tenant isolation, API authentication, encryption (at rest &#x26; in transit), AWS GuardDuty monitoring</td><td>VPC isolation, PrivateLink, IAM-based role filtering, API authentication, encryption (at rest &#x26; in transit), AWS GuardDuty monitoring, tenant-specific IAM roles, dedicated security policies based on customer requirements</td><td>Identical to Private Cloud (Looply-Managed) </td></tr><tr><td>Provisioning &#x26; Access Control</td><td>Fully managed by Looply</td><td>Looply provisions and manages all infrastructure, including ongoing optimizations and updates</td><td>Looply provisions infrastructure in customer’s AWS account using securely shared credentials (e.g., temporary IAM roles, access vault). Infrastructure-as-Code (IAC) deployment requires client secrets (Serverless Framework). Customer is responsible for access governance after provisioning.</td></tr><tr><td>Software Updates</td><td>Automatic (Looply maintains version control &#x26; security patches)</td><td>Automatic (Looply ensures all instances remain at the latest service pack level)</td><td>Customer-defined(Looply provides updates, but customer led rollout)</td></tr><tr><td>Firefighter Access &#x26; Update Control</td><td>Full control (Looply manages everything, no customer intervention required)</td><td>Looply has full admin access for patches, updates, and troubleshooting</td><td>Looply requires ad-hoc 'Firefighter Access' for applying patches, upgrades, and troubleshooting. Access is temporary, controlled via IAM, and revoked after completion. </td></tr><tr><td>Support &#x26; Troubleshooting Model</td><td>Full support (Looply manages infrastructure, instant issue resolution with 24/7 monitoring )</td><td>Full support (Looply has direct access to logs, monitoring, and security patches. Response time SLAs are flexible and defined contractually based on business needs)</td><td>Customer IT team is primary support, Looply offers ‘Advisory Support’ with a troubleshooting SLA defined contractually, dependent on customer-provided access</td></tr><tr><td>Migration Flexibility</td><td>Can migrate to Private Cloud later(data export/import process)</td><td>Easier transition between Looply-Managed and Customer-Managed models</td><td>Customer fully responsible for migrating out</td></tr><tr><td>Customization &#x26; Integrations</td><td>Standard setup, limited customization</td><td>Some customization (branding, integrations, policies)</td><td>Full control over integrations &#x26; network security</td></tr><tr><td>Disaster Recovery &#x26; High Availability</td><td>Serverless architecture with automated scaling, multi-AZ support, and Looply-managed backups</td><td>Serverless architecture with automated scaling, multi-AZ support, and Looply provides automated backups by default, with customizable retention policies based on customer requirements</td><td>Serverless architecture with automated scaling, multi-AZ support, but customer responsible for backup policies</td></tr><tr><td>Performance &#x26; Scaling</td><td>Auto-scaling based on AWS Lambda &#x26; DynamoDB on-demand scaling</td><td>Looply-optimized scaling with dedicated tenant resources</td><td>Auto-scaling enabled, but customer governs configuration</td></tr><tr><td>Data Retention &#x26; Backup Policies</td><td>Fully Configurable retention &#x26; standard Looply defined backup policies</td><td>Fully Configurable retention &#x26; customizable backup policies</td><td>Fully Configurable retention &#x26; customizable backup policies</td></tr><tr><td>Infrastructure Cost</td><td>Included in Looply’s pricing (No additional AWS cost to customer)</td><td>Included in Looply’s pricing (No additional AWS cost to customer)</td><td>Customer pays directly for AWS infrastructure usage</td></tr><tr><td>Usage + Licensing Cost</td><td>Metered based on usage + flat subscription fee (includes hosting, management, and support)</td><td>Metered based on usage + Flat subscription fee (Looply manages software &#x26; updates)</td><td>Flat subscription fee (Looply provides software updates, but customer manages infrastructure)</td></tr></tbody></table>

### Not Sure Which Model Is Right For You?

Reach out to our team at [support@looply.ai](mailto:support@looply.ai) or schedule a deployment planning call. We’ll help you align hosting with your security, compliance, and operational goals.

> Architecture diagrams and data flow overviews available upon request.
>
>
