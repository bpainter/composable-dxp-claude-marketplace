---
type: reference
project: skills-library
scope: skill
plugin: software-engineering
parent: "[[security-advisor]]"
tags: [type/reference, plugin/software-engineering, scope/skill, topic/software]
status: active
---
# Threat Modeling & Security Architecture

## Threat Modeling: STRIDE vs. PASTA

### STRIDE (Spoofing, Tampering, Repudiation, Information Disclosure, DoS, Elevation)

**When to use**: Best for data flow diagrams. Quick threat generation.

**Process**:
1. Draw data flow diagram (DFD): External entities, processes, data stores, flows
2. For each flow/element, ask: Could this be spoofed/tampered/disclosed/DoS'd/repudiated/escalated?
3. Document threat, impact, control

**STRIDE Example: API Authentication**
```
Threat: Spoofing (attacker forges JWT token)
Impact: Unauthorized access to user data
Control: RS256 signed tokens, key rotation, exp claim validation
```

**Threat: Tampering (attacker modifies JWT payload)**
Impact: Privilege escalation
Control: Validate signature, never trust unsigned tokens

### PASTA (7-stage, Risk-Centric)

**When to use**: Longer engagements. Business context matters. Need to quantify risk.

**7 Stages**:
1. Define business objectives & security goals
2. Identify assets (data, systems, people)
3. Define application architecture (trust boundaries, entry points)
4. Enumerate attack paths (attack trees)
5. Vulnerability analysis (weaknesses in controls)
6. Likelihood analysis (attacker capability, detection probability)
7. Impact analysis (CIA triad: confidentiality, integrity, availability)

**Output**: Risk score = Impact × Likelihood. Prioritize remediation by risk.

## Zero-Trust Architecture

**Principle**: Never trust; always verify.

### Identity Layer

```
User Request
    ↓
1. Multi-Factor Authentication (MFA)
2. Device Health Check (patched OS? antivirus? encryption?)
3. Behavioral Analytics (anomalous access pattern?)
4. Conditional Access (geo, time, sensitivity)
    ↓
Grant/Deny + Session Token
```

**Implementation**:
- **OIDC/OAuth 2.0**: Federated identity (Okta, Azure AD, Google)
- **MFA**: FIDO2 > TOTP > SMS
- **Device Trust**: Microsoft Intune, Kandji (MDM/MDM)
- **Conditional Access**: Azure Conditional Access, AWS Verified Access

### Network Segmentation

```
DMZ (Public APIs)
    ↓
Gateway (WAF, rate limiting, API validation)
    ↓
Private Subnet (Application tier)
    ↓
Isolated Subnet (Database, secrets)
```

**Rules**:
- Deny-all default; explicit allow rules
- Segment by trust level, not just by perimeter
- Use network policies (Kubernetes) or security groups (cloud)
- Encrypt all inter-segment traffic (mTLS, VPN tunnels)

**Example: Kubernetes Network Policy**
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: app-to-db
spec:
  podSelector:
    matchLabels:
      tier: application
  policyTypes:
  - Egress
  egress:
  - to:
    - podSelector:
        matchLabels:
          tier: database
    ports:
    - protocol: TCP
      port: 5432
```

### Data Layer

**Encryption at Rest**:
- Database: TDE (Transparent Data Encryption), AWS KMS, Azure Key Vault
- Files: Object encryption (S3-SSE, GCS CSEK)
- Backups: Encrypted snapshots, customer-managed keys
- **Never hardcode keys; rotate automatically**

**Encryption in Transit**:
- TLS 1.2+ (TLS 1.3 preferred)
- Certificate pinning for critical APIs
- mTLS between microservices
- VPN/tunnels for hybrid environments

**Key Management**:
- Hardware Security Module (HSM) for high-sensitivity keys
- Automatic key rotation (annual for encryption keys, quarterly for signing keys)
- Key versioning (support multiple key versions for zero-downtime rotation)
- Least-privilege key access (different keys per environment)

**Example: AWS KMS Integration**
```python
import boto3
kms_client = boto3.client('kms')

# Encrypt data
response = kms_client.encrypt(
    KeyId='arn:aws:kms:region:account:key/xyz',
    Plaintext=b'sensitive_data'
)
encrypted_blob = response['CiphertextBlob']

# Decrypt (only grants key access, validates permissions)
plaintext = kms_client.decrypt(CiphertextBlob=encrypted_blob)['Plaintext']
```

### Least-Privilege Access

**RBAC (Role-Based)**: User has role; role has permissions. Simple, coarse-grained.
**ABAC (Attribute-Based)**: Permissions based on attributes (user, resource, action, environment).

**Implementation**:
```
User → Role (e.g., Editor)
Role → Permissions (e.g., read:articles, write:articles, delete:own-articles)
Resource → Owner, Labels (environment=prod, sensitivity=high)
```

**Principle**: Service account per workload. Time-bound tokens. No shared credentials.

## Defense-in-Depth Layers

```
Layer 1: Perimeter (WAF, DDoS mitigation)
Layer 2: Network (Segmentation, NACLs)
Layer 3: Application (Input validation, OWASP controls)
Layer 4: Data (Encryption, access controls)
Layer 5: Monitoring (Logging, alerting, SIEM)
```

Each layer is independent. Breach at Layer 3 doesn't bypass Layer 4 encryption.

## Cloud Security Architecture

### AWS Security Services

**IAM**: Identity and access management. Policies evaluate all requests. Audit with CloudTrail.
```json
{
  "Effect": "Allow",
  "Action": "s3:GetObject",
  "Resource": "arn:aws:s3:::bucket/path/*",
  "Condition": {
    "IpAddress": { "aws:SourceIp": "10.0.0.0/8" }
  }
}
```

**VPC**: Virtual private network. Security Groups (stateful), NACLs (stateless).
**KMS**: Key management service. Encrypt EBS, RDS, S3, secrets.
**Secrets Manager**: Store/rotate database passwords, API keys.
**GuardDuty**: Threat detection using ML (detects unusual account access, reconnaissance).
**Security Hub**: Centralized dashboard for security findings.

### Azure Security Services

**Azure AD**: Identity provider. Conditional access, MFA, device compliance.
**Key Vault**: Secrets, keys, certificates. RBAC access.
**Azure Security Center**: Vulnerability assessment, compliance monitoring.
**Network Security Groups (NSGs)**: Layer 4 filtering.
**Application Gateway**: WAF, SSL offloading, request filtering.

### GCP Security Services

**Cloud IAM**: Role-based access. Service accounts. Workload Identity (bind k8s service accounts to GCP identities).
**Cloud KMS**: Encryption key management.
**Secret Manager**: Secure secrets storage with version history.
**Cloud Armor**: DDoS protection, WAF rules.
**Cloud Audit Logs**: Activity logging, data access logging.

## API Security

### OAuth 2.0 Flows

**Authorization Code Flow** (user-facing, web/mobile apps):
```
User clicks "Login with Google"
    ↓ Redirects to Google
    ↓ User grants permission
    ↓ Authorization code returned to app
    ↓ App exchanges code for access token (via backend)
    ↓ Access token used for API calls
```

**Client Credentials** (service-to-service, no user):
```
Service A requests token from Auth Server using client_id + client_secret
Auth Server issues access token
Service A uses token to call Service B
```

**Device Flow** (IoT, CLI apps without browser):
```
Device displays code to user
User visits device.example.com, enters code
Device polls for approval
Once approved, token issued to device
```

### JWT (JSON Web Tokens)

**Structure**: `header.payload.signature`

**Header**: Algorithm (RS256 = RSA SHA256)
**Payload**: Claims (sub = subject, aud = audience, exp = expiration, custom claims)
**Signature**: RSA(header.payload, private_key)

**Validation Rules**:
- ✓ Verify signature (RS256: use public key)
- ✓ Check `exp` (not expired)
- ✓ Check `aud` (token audience matches your service)
- ✓ Check `iss` (issuer is trusted)
- ✗ Never trust unsigned tokens
- ✗ Never accept `alg: none`

**Example** (Python):
```python
import jwt

token = request.headers.get('Authorization').split(' ')[1]
try:
    payload = jwt.decode(token, public_key, algorithms=['RS256'])
    user_id = payload['sub']
except jwt.ExpiredSignatureError:
    return 401  # Token expired
except jwt.InvalidSignatureError:
    return 401  # Tampered token
```

### API Gateway & Rate Limiting

**Rate Limiting**: Prevent abuse. Track by IP, user_id, or API key.
```
100 requests per minute per user
1000 requests per minute per IP
```

**Backoff Strategy**: Return `429 Too Many Requests` with `Retry-After` header.

**API Key Scope**: Each key has explicit permissions (read-only, write, specific endpoints).

## Container & Kubernetes Security

### Image Scanning

**Build-time**: Scan during CI/CD. Block merge if critical CVEs.
**Registry-time**: Scan pushed images. Prevent deployment of vulnerable images.
**Runtime**: Monitor running containers for CVE updates.

**Tools**: Trivy, Anchore, Aqua, Clair

### Kubernetes RBAC

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: app-reader
rules:
- apiGroups: [""]
  resources: ["configmaps"]
  verbs: ["get", "list"]

---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: app-reader-binding
subjects:
- kind: ServiceAccount
  name: app-sa
roleRef:
  kind: Role
  name: app-reader
```

### Pod Security

**SecurityContext** (run as non-root, read-only filesystem):
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secure-app
spec:
  securityContext:
    runAsNonRoot: true
    runAsUser: 1000
    fsGroup: 2000
  containers:
  - name: app
    image: myapp:latest
    securityContext:
      readOnlyRootFilesystem: true
      allowPrivilegeEscalation: false
      capabilities:
        drop: ["ALL"]
```

### Secrets Management

**Never embed secrets in container images.**

**Approach**:
1. Store secrets in external vault (HashiCorp Vault, AWS Secrets Manager)
2. Inject secrets as environment variables or mounted files
3. Use Workload Identity (GCP) or IAM Roles for Service Accounts (AWS) or Managed Identity (Azure) to authenticate to vault

**Example: Sealed Secrets (Kubernetes)**
```bash
# Seal a secret
echo -n mysecret | kubectl create secret generic mysecret --dry-run -oyaml | kubeseal -f - > sealed-secret.yaml

# Kubernetes applies SealedSecret; Sealed Secrets controller decrypts it in-cluster
kubectl apply -f sealed-secret.yaml
```

## Encryption Patterns

**At Rest**: Database encryption (TDE), file encryption (object storage), backup encryption
**In Transit**: TLS/mTLS, VPN tunnels, secure APIs
**Key Derivation**: PBKDF2 (passwords), HKDF (keys from master key)
**Tokenization**: Replace sensitive data with tokens (e.g., PCI tokenization for card numbers)
**Hashing**: Passwords (bcrypt, Argon2); integrity checks (SHA-256 HMAC)

**Never**: Use MD5 or SHA1 for security, store plaintext passwords, use `localhost` SSL certificates in prod
