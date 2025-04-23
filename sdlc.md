# Secure Development Life Cycle (SDLC) Policy
**Version:** 0.1 (Draft) **Owner:** Engineering Team Lead (Shamari Ishmael) **Approved by:** [TBD] **Effective date:** [TBD]

---

## 1. Purpose
This policy establishes the Secure Development Life‑Cycle (SDLC) for all software engineered, integrated,
or operated by **ProfitFill**. It ensures that security is embedded throughout the life‑cycle to protect
the confidentiality, integrity, and availability (CIA) of information assets and to meet the requirements of
ISO 27001:2022 Annex A controls 8.25 – 8.31

## 2. Scope
**Systems**
: All code, infrastructure-as-code (IaC), serverless functions, data pipelines, scripts, and
third-party libraries maintained by the company.

**Environments**
: Development, staging, and production workloads in AWS, plus local developer machines.

**Activities**
: Requirements, design, coding, build, test, deployment, maintenance, and retirement.

## 3. Normative References
- ISO 27001:2022
- NIST SP 800-218 Secure Software Development Framework (SSDF)
- OWASP Software Assurance Maturity Model (SAMM v2)
- OWASP Top 10 (Web & API)

## 4. Definitions
**CI/CD**
: Continuous Integration / Continuous Delivery via GitHub Actions.

**IaC**
: Infrastructure-as-Code (AWS CDK).

**SAST**
: Static Application Security Testing.

**SCA**
: Software Composition Analysis (dependency scanning).

**DAST**
: Dynamic Application Security Testing.

## 5. Roles & Responsibilities
| Role                                     | Responsibilities                                                                   |
|------------------------------------------|------------------------------------------------------------------------------------|
| **Enginerring Team Lead (Policy Owner)** | Maintain policy, approve exceptions, report KPIs to ISMS forum.                    |
| **Security Engineer**                    | Define secure-coding standards, operate SAST/SCA/DAST tooling.                     |
| **Software Engineers**                   | Follow secure-coding standards, remediate findings, perform peer reviews.          |
| **DevOps (shared responsibility)**       | Maintain CI/CD security controls, enforce environment segregation, manage secrets. |
| **Product Owner**                        | Capture and prioritise security requirements & acceptance criteria.                |

## 6. SDLC Policy Requirements
### 6.1 Planning & Requirements
1. Every epic or significant change must include security requirements linked to the company data-classification scheme.
2. A lightweight **threat-model** (STRIDE or equivalent) is required for new features handling sensitive data.

### 6.2 Design
1. A **secure-architecture review** checklist must be completed and attached to the ticket before coding starts.
2. Architecture diagrams are version-controlled in the `/docs/architecture` folder.

### 6.3 Development
1. Developers **SHALL** follow language-specific secure-coding standards (OWASP Top 10, CERT Python, Node.js best practices).
2. GitHub **branch protection** rules enforce peer review by >= 1 qualified reviewer and the completion of a security checklist template.
3. Hard-coded secrets are prohibited; AWS Secrets Manager or Parameter Store must be used.

### 6.4 Build & Integration (CI)
1. GitHub Actions is the **single source of build truth**; all merges trigger the `secure-build.yml` workflow.
2. The workflow executes:
   - Dependabot updates (daily)
   - **SCA** (GitHub Advanced Security or OWASP Dependency-Check)
   - **SAST** (CodeQL for JavaScript/Python)
   - **Secret scanning** (GitHub Secret Scanning)
3. Builds **fail** on any *critical* or *high* vulnerability unless an exception is approved by the Engineering Team Lead **and** Security Engineer.

### 6.5 Testing & Acceptance
1. **Unit & integration test** coverage target >= 80% lines.
2. **DAST** (OWASP ZAP) runs nightly against the staging environment; outputs are triaged daily.
3. High-risk releases (handling new PII flows or major auth changes) require sign-off from the Security Engineer.
4. Release **acceptance criteria** include: all automated security tests pass; no open *high* or *critical* findings.

### 6.6 Deployment & Release
1. Only artefacts produced by the signed CI pipeline may be deployed via AWS CDK pipelines.
2. Promotion path: dev -> staging -> production with manual approval for production.
3. Environments are segregated by labeling in AWS; production data never used in lower tiers.

### 6.7 Maintenance & Support
| Vulnerability severity | Remediation deadline  |
| ---------------------- | --------------------- |
| Critical               | <= 7 days             |
| High                   | <= 30 days            |
| Medium / Low           | Next scheduled sprint |

- End-of-life components must be decommissioned within **90 days** of vendor EOL notice.

### 6.8 Supplier / Outsourced Development
Currently **not in use**. If reintroduced:
1. Suppliers must agree contractually to this policy.
2. Code deliverables are subject to the same CI/CD and review gates.

### 6.9 Documenation & Evidence
- Pull-request history, CI logs, scan reports, threat-model diagrams, and change-control tickets are retained for **5 years**.

### 6.10 KPIs & Continual Improvement
- Mean Time to Remediate (MTTR) *critical* findings < 7 days.
- => 90% of pull requests pass security gates on first attempt.
- Policy effectiveness review conducted **annually** by the Engineering Team Lead and Security Engineer; output feed the ISMS continual-improvement plan.

### 6.11 Exceptions
- Exceptions must be documented in the ISMS risk register, include mitigating controls, and obtain approval from the Security Engineer.
- Temporary exceptions are reviewed within **30 days**.

### 6.12 Enforcement
Non-compliance may result in code being rejected.

---

## 7. Change Log
| Version | Date       | Description                 | Author                                  |
| ------- | ---------- | --------------------------- | --------------------------------------- |
| 0.1     | 2025-04-17 | Initial draft for ISO 27001 | Engineering Team Lead (Shamari Ishmael) |
