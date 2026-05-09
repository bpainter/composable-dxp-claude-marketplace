---
type: reference
project: skills-library
scope: skill
plugin: software-engineering
parent: "[[security-advisor]]"
tags: [type/reference, plugin/software-engineering, scope/skill, topic/software]
status: active
---
# Compliance Frameworks & Security Operations

## SOC 2 Type II Overview

**What it is**: Trust Services Audit. Demonstrates controls are effective over time (6-12 month period).

**Five Trust Service Categories** (pick which apply):
- **CC** (Common Criteria): All SOC 2 audits cover this. System security, change management, asset management, access control.
- **C** (Confidentiality): Data confidentiality controls. Encryption, access restrictions.
- **A** (Availability): System availability controls. Uptime, redundancy, disaster recovery.
- **PO** (Processing Integrity): Data accuracy and completeness. Error detection, validation.
- **PI** (Privacy): Personal data handling per privacy laws. Data collection, usage, retention.

**Type I vs. Type II**:
- **Type I**: Snapshot in time (one day audit)
- **Type II**: Effectiveness over time (6-12 months). Better for investor confidence, customer contracts.

**Control Evidence**:
- Logs (access logs, change logs, audit trails)
- Policies (written, approved, distributed)
- Training records (employee security training)
- Incident response records (problems identified, resolved)
- Configuration reviews (hardening configs, vulnerability scans)

**Typical Cost**: $15K-50K depending on scope, size.
**Timeline**: Plan 6-9 months before audit for evidence collection.

### SOC 2 Control Mapping Example

| Control | Evidence |
|---------|----------|
| Multi-factor authentication (CC6.1) | Azure/Okta logs showing MFA enabled; IAM policy requiring MFA |
| Encryption in transit (CC6.2) | TLS config on load balancers; certificate audit |
| Access reviews (CC6.1) | Quarterly access reviews; removed access records |
| Change management (CC7.2) | Change tickets, approval records, testing logs |
| Incident response (CC9.2) | Incident records: detection time, containment steps, root cause |
| Data retention/deletion (PI2.1) | Data retention policy; deletion request logs |

## ISO 27001 ISMS

**What it is**: Information Security Management System. Certification audit by external third party. Ongoing compliance (annual audits).

**14 Control Domains** (114 controls total):
1. Organization of Information Security
2. Asset Management
3. Human Resources Security
4. Asset Management (continued)
5. Access Control
6. Cryptography
7. Physical & Environmental Security
8. Operations Security
9. Communications Security
10. System Acquisition, Development, Maintenance
11. Supplier Relations
12. Information Security Incident Management
13. Business Continuity Management
14. Compliance

**Certification Path**:
1. Gap assessment (where are you weak?)
2. ISMS design (write policies, design controls)
3. ISMS implementation (deploy controls, collect evidence)
4. Stage 1 audit (readiness check; ~1 month before Stage 2)
5. Stage 2 audit (effectiveness audit; external auditor)
6. Certificate issued (valid 3 years)
7. Surveillance audits (annual checks)

**Typical Cost**: $30K-100K+ depending on organization size.
**Timeline**: 9-18 months from gap assessment to certification.

### ISO 27001 Control Examples

```
A.5: Policies for Information Security
  - Written policy, approved, communicated to staff
  - Regular review (annual minimum)

A.6: Organization of Information Security
  - CISO or equivalent role defined
  - Segregation of duties (no one person controls critical systems)

A.9: Access Control
  - User access provisioning/deprovisioning process
  - Least privilege (users have only necessary permissions)
  - Periodic access reviews

A.12: Operations Security
  - Change management process
  - Backup & recovery testing
  - Capacity management (prevent denial of service)

A.14: System Acquisition, Development, Maintenance
  - Secure development process (input validation, code review)
  - Vulnerability scanning & patching
  - Testing before production deployment
```

## HIPAA (Healthcare Data)

**Scope**: Any organization touching Protected Health Information (PHI).

**Technical Safeguards** (most relevant to architects):
- **Access Control**: User IDs, authentication (MFA), authorization (RBAC), audit logging
- **Audit Controls**: Log all access to PHI; retain logs ≥6 years
- **Integrity Controls**: Checksums, digital signatures to detect tampering
- **Transmission Security**: Encrypt PHI in transit (TLS); secure disposal of data

**Additional Requirements**:
- **Business Associate Agreement (BAA)**: Signed with any vendor touching PHI (cloud provider, analytics, backup)
- **Annual Risk Assessment**: Identify vulnerabilities; document remediation
- **Breach Notification**: Report breaches >500 individuals to HHS within 60 days
- **Security Officer**: Designated security officer accountable for controls

**Key Technical Controls**:
```
✓ Encryption: All PHI at rest (AES-256), in transit (TLS 1.2+)
✓ Access: MFA, role-based access, automatic logout
✓ Logging: All PHI access logged; audit logs read-only
✓ Backups: Encrypted, tested quarterly
✓ Vendor Management: BAA signed, annual assessment
✓ Incident Response: Detection, containment, notification plan
```

**Typical Cost**: BAA negotiation (free), annual risk assessment ($5-20K), compliance remediation varies.
**Timeline**: Continuous compliance; initial setup 3-6 months.

## PCI-DSS (Payment Card Data)

**Scope**: Any organization storing, processing, or transmitting credit card data (PAN, CVV, expiry).

**12 Requirements**:
1. Firewall configuration (network boundary)
2. Default passwords removed
3. Data at rest encrypted
4. Data in transit encrypted
5. Anti-virus software
6. Software patching (security updates within 30 days)
7. Restrict data access by role
8. User identification (unique user IDs)
9. Restrict physical access
10. Unique audit trails (logging)
11. Regular security testing (vulnerability scans, penetration testing)
12. Security policy (written, approved)

**Assessment Tiers** (based on transaction volume):
- **Tier 1**: >6M transactions/year → Annual QSA audit + quarterly vulnerability scans
- **Tier 2**: 1-6M transactions/year → Annual QSA audit + quarterly scans
- **Tier 3**: <1M transactions/year, online → Annual Self-Assessment Questionnaire (SAQ)
- **Tier 4**: <100K transactions/year, all online → Self-Assessment Questionnaire

**Scope**: Define what's in-scope (systems handling card data). Reduce scope via tokenization.
```
BAD: Store full PAN → All systems are in-scope
GOOD: Accept card, tokenize immediately, store token → Only gateway in-scope
```

**Cost**: $5-50K+ for QSA audit, varies by complexity.
**Timeline**: Ongoing; annual re-certification.

## FedRAMP (U.S. Government)

**What it is**: Compliance program allowing cloud services to sell to U.S. federal agencies. Based on NIST SP 800-53.

**Impact Levels**:
- **Low**: No significant impact if breached (unclassified info)
- **Moderate**: Significant impact (PII, financial data)
- **High**: Major impact (classified info, national security)

**Security Controls** (NIST SP 800-53):
- **AC** (Access Control): MFA, RBAC, session management (e.g., AC-2, AC-3, AC-11)
- **AU** (Audit): Comprehensive audit logging; protect logs from tampering (AU-2, AU-11, AU-12)
- **AT** (Awareness/Training): Security training for all personnel (AT-1, AT-2)
- **CA** (Security Assessment & Authorization): Regular risk assessments, penetration testing (CA-2, CA-8)
- **CM** (Configuration Management): Baseline configs, change control (CM-2, CM-3)
- **CP** (Contingency Planning): Disaster recovery, backup testing (CP-4, CP-9)
- **IA** (Identification & Authentication): Strong authentication, password policy (IA-2, IA-5)
- **SC** (System & Communications Protection): Encryption, network segmentation (SC-7, SC-13)

**Certification Path**:
1. **Pre-Authorization**: Gather NIST controls evidence, document system
2. **Authorization Package**: Submit to FedRAMP PMO (Program Management Office)
3. **3PAO Assessment**: Third-party assessor audits for 2-4 months
4. **Remediation**: Fix findings
5. **Authority to Operate (ATO)**: FedRAMP grants authorization for 3 years
6. **Continuous Monitoring**: Annual assessments, respond to incidents

**Cost**: $150K-500K+ depending on impact level and system complexity.
**Timeline**: 12-24 months from start to ATO.

## Vulnerability Management Lifecycle

### 1. Scanning
Automated scans identify CVEs in systems and dependencies.

**Tools**:
- **Application**: SonarQube, Fortify, Checkmarx
- **Infrastructure**: Nessus, Qualys, AWS Inspector, Azure Defender
- **Dependencies**: Snyk, Dependabot, OWASP Dependency-Check
- **Container Images**: Trivy, Anchore, Aqua

**Schedule**: Weekly infrastructure scans, daily dependency scans in CI/CD.

### 2. Prioritization
Rank CVEs by risk (CVSS score × exploitability × business impact).

**CVSS Score** (0-10):
- **9+**: Critical; patch immediately
- **7-9**: High; patch within 2 weeks
- **5-7**: Medium; patch within 1-2 months
- **<5**: Low; patch in next release

**Exploitability**: Is exploit code public? Is it being actively exploited?

**Business Context**: Is this system internet-facing? Does it handle sensitive data?

### 3. Remediation
Options:
- **Patch**: Update to patched version
- **Upgrade**: Update library/framework
- **Workaround**: Disable feature, apply configuration fix
- **Accept**: Document risk; ensure stakeholder approval

**Tracking**: Jira ticket → code change → testing → deployment → verify

### 4. Verification & Reporting
Rescan after remediation to confirm fix. Track metrics:
- Mean Time to Detect (MTTD)
- Mean Time to Remediate (MTTR)
- % vulnerabilities patched within SLA

## Incident Response Playbook

### Detection Phase
**Signals**:
- Security alerts (SIEM, WAF, IDS)
- Unusual user behavior (login from new location, bulk downloads)
- Performance degradation (resource exhaustion)
- Third-party notification (law enforcement, breach notification service)

**Response**: Alert on-call security team. Create incident ticket.

### Containment Phase
**Objective**: Stop active compromise.

**Actions**:
- Isolate affected system (disable network access, revoke credentials)
- Disable compromised user accounts
- Revoke API tokens, SSH keys
- Snapshot systems for forensics
- Prevent lateral movement (network segmentation blocks it)

**Timeline**: Ideally <15 minutes for critical systems.

### Eradication Phase
**Objective**: Remove attacker artifacts, fix root cause.

**Actions**:
- Patch vulnerability that enabled breach
- Remove backdoors, malware
- Rotate all credentials
- Audit logs for persistence mechanisms
- Scan for additional compromised systems

**Timeline**: Hours to days depending on scope.

### Recovery Phase
**Objective**: Restore operations safely.

**Actions**:
- Restore from clean backups
- Verify integrity (checksums, logs)
- Gradually restore systems; monitor for re-infection
- Restore access for legitimate users

**Timeline**: Hours depending on recovery time objective (RTO).

### Post-Incident Phase
**Objective**: Prevent recurrence.

**Root Cause Analysis (RCA)**:
- Timeline of events
- Root cause (why did it happen?)
- Contributing factors (what conditions allowed it?)
- Remediation (fix the root cause)
- Preventive controls (prevent recurrence)

**Communicate**:
- All-hands incident briefing
- Customer notification (if needed; depends on data exposure)
- Regulatory notification (if required)
- Media statement (for major incidents)

**Sample Playbook Section**:
```
INCIDENT: Ransomware via Email Phishing

Detection:
  - User reports suspicious email
  - Endpoint protection detects file encryption

Containment (first 15 min):
  - Isolate affected system from network
  - Disable user account
  - Alert incident commander

Eradication (next 2 hours):
  - Snapshot system for forensics
  - Scan network for lateral movement
  - Patch email filtering rules

Recovery (next 4 hours):
  - Restore from backup (pre-encryption snapshot)
  - Re-enable user with new password
  - Validate no re-infection

Post-Incident:
  - Increase email security training
  - Deploy EDR (Endpoint Detection & Response)
  - Quarterly tabletop exercises
```

## Security Monitoring & SIEM

**SIEM** (Security Information & Event Management):
- Aggregates logs from all systems
- Correlates events (e.g., multiple failed logins → account breach)
- Alerts on suspicious patterns
- Provides audit trail for compliance

**Tools**: Splunk, ELK Stack, AWS Security Hub, Azure Sentinel

**Key Queries**:
```
# Failed login attempts (potential brute force)
event_type:login status:failure
| stats count by user, source_ip
| where count > 5

# Privilege escalation (user gains admin rights)
event_type:privilege_change new_role:admin
| where user NOT in (approved_admins)

# Data exfiltration (large outbound transfer)
event_type:network direction:outbound bytes > 1GB
| stats sum(bytes) by user, destination
```

## Penetration Testing

**Scope**: Define what you're testing and what you're not (avoid data centers, production outages).

**Approach**:
1. Reconnaissance (public info, DNS enumeration)
2. Scanning (active scanning for open ports)
3. Exploitation (attempt to gain access)
4. Post-exploitation (lateral movement, data access)
5. Reporting (vulnerabilities, business impact, remediation)

**Output**: Pentest report with CVSS scores, executive summary, remediation roadmap.

**Frequency**: Annual minimum for regulated organizations; more often for high-risk systems.

## Supply Chain Security (SBOM & Dependencies)

### Software Bill of Materials (SBOM)
Document every dependency (direct and transitive).

**Format**: CycloneDX or SPDX
```xml
<component type="library">
  <name>log4j-core</name>
  <version>2.14.1</version>
  <licenses>
    <license>
      <name>Apache-2.0</name>
    </license>
  </licenses>
</component>
```

**Uses**:
- Identify vulnerable dependencies quickly (e.g., Log4Shell CVE)
- License compliance (ensure no GPL components in proprietary code)
- Third-party risk assessment (unknown dependencies?)

### Dependency Scanning
Tools flag outdated or vulnerable dependencies.

**Tools**: Dependabot (GitHub), Snyk, WhiteSource
**Automation**: CI/CD gate; block merge if critical CVE detected

**Example** (Node.js):
```json
{
  "dependencies": {
    "log4j-core": "2.14.1",  // CVE-2021-44228 → 2.17.0+
    "lodash": "4.17.19"      // Prototype pollution → 4.17.21
  }
}
```

## Security Awareness Training

**Content**:
- Password hygiene (long, unique, manager)
- Phishing recognition (hover email links, look for misspellings)
- Social engineering (pretexting, physical tailgating)
- Data classification (don't share confidential data via email)
- Incident reporting (report to security team, not management)

**Delivery**: Annual mandatory training; quarterly reinforcement emails; phishing simulations.

**Measurement**: Track completion %, phishing click rate, reported incidents.

---

## Mapping Compliance to Architecture

| Requirement | Control | Implementation |
|---|---|---|
| HIPAA/PCI: Encryption at rest | SC-13 | AES-256 on RDS, S3, EBS |
| HIPAA/PCI: Encryption in transit | SC-13 | TLS 1.2+ on APIs, VPN tunnels |
| ISO 27001: Access control | AC-3 | IAM roles, RBAC, MFA |
| SOC 2: Audit logging | AU-12 | CloudTrail, app logs → SIEM |
| FedRAMP: Patch management | CM-3 | Monthly patch cycle, testing |
| PCI: Vulnerability management | CA-2 | Quarterly scans, MTTR SLA |

---

**Key Takeaway**: Compliance is not security theater. Well-designed controls simultaneously satisfy compliance requirements AND reduce actual risk.
