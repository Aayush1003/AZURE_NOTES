# 🔒 Security, Identity & Compliance

Security is a shared responsibility in the cloud. Azure provides robust tools to secure your resources and identities.

## 1. Microsoft Entra ID (formerly Azure Active Directory)
- **What is it?** Azure's cloud-based Identity and Access Management (IAM) service.
- **Features:** Authentication (verifying who you are), Authorization (verifying what you can do), Single Sign-On (SSO), Multi-Factor Authentication (MFA), Conditional Access (if/then rules for access based on location/device).
- **Difference from Windows Server AD:** Entra ID is designed for web-based services (HTTP/HTTPS, REST, SAML, OIDC) whereas Server AD is for on-prem networks (Kerberos, NTLM).

## 2. Role-Based Access Control (RBAC)
- **What is it?** A system that provides fine-grained access management to Azure resources.
- **How it works:** You assign roles (e.g., Owner, Contributor, Reader) to users/groups/service principals at a specific scope (Management Group, Subscription, Resource Group, or Resource).
- **Principle of Least Privilege:** Always give users the minimum level of access they need to do their jobs.

## 3. Azure Policy
- **What is it?** A service used to create, assign, and manage policies that enforce different rules and effects over your resources (e.g., "All VMs must be deployed in the East US region", "Certain VM SKUs are not allowed").
- **Difference from RBAC:** RBAC focuses on *user actions* (who can do what). Azure Policy focuses on *resource properties* (what a resource should look like).

## 4. Network Security Groups (NSG)
- **What is it?** Acts like a basic firewall for your Virtual Network. It contains security rules that allow or deny inbound/outbound network traffic to/from Azure resources (like VMs).
- **Scope:** Can be applied to a Subnet or directly to a VM's Network Interface Card (NIC).

## 5. Microsoft Defender for Cloud
- **What is it?** A Cloud Security Posture Management (CSPM) and Cloud Workload Protection Platform (CWPP). It provides a secure score, security recommendations, and threat protection for workloads in Azure, AWS, and GCP.
