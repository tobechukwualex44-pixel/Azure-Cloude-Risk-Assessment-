# Azure Cloud Security Risk Assessment

## Subtitle
A hands-on GRC exercise using Microsoft Defender for Cloud to identify, score, and remediate cloud misconfigurations, with inherent and residual risk mapped to NIST CSF 2.0.

## Overview
This project simulates a cloud security risk assessment of a small Azure environment. Using Microsoft Defender for Cloud (Foundational CSPM, free plan) alongside manual review, I identified three security misconfigurations, scored each by likelihood and impact, recorded remediation actions, and documented the residual risk after fixes were applied. Where a fix had not yet been carried out, the risk was left open and reported honestly.

## Objectives
- Identify misconfigurations in an Azure subscription and Microsoft Entra ID tenant
- Score risks using a likelihood x impact method
- Distinguish inherent risk from residual risk
- Map findings to a recognised control framework (NIST CSF 2.0)
- Document findings in a risk register suitable for management reporting

## Tools and Technologies
- Microsoft Defender for Cloud (Foundational CSPM, free plan)
- Azure Portal (subscriptions, resource groups, storage accounts)
- Microsoft Entra admin center (Security Defaults)

## Scoring Method
Risk score = Likelihood (1-5) x Impact (1-5)

| Score | Rating |
|---|---|
| 1-5 | Low |
| 6-11 | Medium |
| 12-19 | High |
| 20-25 | Critical |

Residual risk is only reduced once a remediation has actually been carried out. If a risk is still Open, its residual score equals its inherent score.

## Step-by-Step Process

### 1. Opening Microsoft Defender for Cloud
I opened Microsoft Defender for Cloud from within the Azure subscription and confirmed the Foundational Cloud Security Posture Management (CSPM) plan was active. CSPM continuously checks cloud resources against security best practices and flags misconfigurations.

![Defender for Cloud overview] <img width="1470" height="833" alt="Screenshot 2026-10-26 at 8 27 09 AM" src="https://github.com/user-attachments/assets/71b92881-209b-48fb-95ba-123a5bff926a" />


### 2. Reviewing Defender Recommendations
Defender for Cloud flagged that the subscription had only one Owner assigned, a High severity finding because it creates a single point of failure for administrative access.

![Defender recommendation] <img width="1470" height="836" alt="Screenshot 2026-10-26 at 8 44 00 AM" src="https://github.com/user-attachments/assets/bd4a9a52-b0a4-4664-bdf7-abda84c21b1d" />


### 3. Manual Review: Storage Account Exposure
While creating a storage account (hrostorage26) in the resource group "HRO resources OT", I found that new storage accounts default to public network access being enabled. I disabled public access under the Networking tab before deployment to reduce the exposure of HR data.

![Storage account networking setting] <img width="1256" height="838" alt="Screenshot 2026-10-27 at 2 41 32 AM" src="https://github.com/user-attachments/assets/e1130cc0-dedc-47ea-8a59-b0374cc37ea3" />


### 4. Manual Review: Security Defaults
In the Microsoft Entra admin center, under Entra ID, Overview, Properties, I found Security Defaults set to No, meaning there was no tenant-wide multi-factor authentication. I switched it to Yes.

![Security Defaults setting] <img width="1470" height="797" alt="Screenshot 2026-10-27 at 2 46 07 AM" src="https://github.com/user-attachments/assets/5353ff97-5800-4f42-a850-970d11ca9696" />


## Risk Register

| # | Risk | Likelihood | Impact | Inherent Score | Remediation | Status | Residual Score |
|---|---|---|---|---|---|---|---|
| 1 | Public network access enabled on HR storage account (hrostorage26) | 4 | 5 | 20 (Critical) | Storage account > Networking > disable public access | Remediated | L1 x I5 = 5 (Low) |
| 2 | Single point of failure: only one Owner assigned to the subscription | 3 | 5 | 15 (High) | Assign a second Owner and consider a break-glass emergency account | Open | 15 (High) until executed; expected 10 (Medium) afterwards |
| 3 | Security Defaults disabled, so no tenant-wide MFA (NIST CSF 2.0: PR.AA) | 5 | 5 | 25 (Critical) | Entra ID > Overview > Properties > Manage security defaults > Yes | Remediated | L2 x I5 = 10 (Medium) |

## Key Reasoning
- **Likelihood drops after remediation, but impact usually does not.** Closing public access makes a breach less likely, but if data were ever exposed, the harm would be the same.
- **Risk 2 is deliberately left Open.** I recorded it as unremediated instead of claiming a fix I had not carried out, because residual risk should only reflect actions actually completed.
- **Framework mapping:** Risk 3 maps to the NIST CSF 2.0 Protect function, Identity Management, Authentication and Access Control (PR.AA).

## Key GRC and Security Concepts Demonstrated
- Cloud Security Posture Management (CSPM)
- Risk identification, scoring and prioritisation
- Inherent versus residual risk
- Risk treatment and status tracking
- Mapping findings to NIST CSF 2.0
- Least privilege and separation of administrative access
- Multi-factor authentication as a preventive control

## Skills Demonstrated
- Cloud security assessment
- Risk register development
- Control framework mapping
- Security configuration review
- Technical documentation and reporting

## What I Learned
- New Azure storage accounts can default to public network access, so it needs to be checked at creation.
- Entra ID is the shared identity layer for both Azure and Microsoft 365, so one setting there affects the whole tenant.
- A risk register is only trustworthy if status and residual risk are reported honestly.

## Notes
This project was completed independently as self-directed learning, using free-tier Azure and Microsoft Entra ID resources, with fictional test data only.
