# PD Lite – Identity, SSO & OAuth (v1)

## Purpose

This document defines modern identity requirements for PD Lite, including SSO for end users/admins and OAuth for integrations and connectors. The goals are:

- Enterprise-ready authentication and provisioning
- Simple onboarding for SMBs
- Secure, least-privilege access for integrations
- Resilience when identity providers fail

---

## Design Principles

1. OIDC-first, SAML supported for compatibility
2. Provisioning must be automated (SCIM) for enterprise
3. Least privilege for all integration access
4. Break-glass access exists for incident continuity
5. All auth and permission changes are auditable

---

## User Authentication (SSO)

### Supported Protocols

- OIDC (preferred)
- SAML 2.0

### Core Requirements

- Tenant-level SSO enforcement
- Domain capture and email verification
- Configurable session lifetime
- Step-up authentication support (future)
- Multiple IdPs per tenant (future)

### Group and Role Mapping

- Map IdP groups to PD Lite roles
- Support default role assignment
- Support role overrides with audit logging

---

## Provisioning (SCIM)

### Requirements

- SCIM 2.0 support for:
  - Users
  - Groups
- Deprovisioning must revoke access promptly
- Support just-in-time provisioning as a fallback

---

## Connector and Integration Authentication (OAuth)

PD Lite must support modern OAuth flows for third-party systems.

### Supported OAuth Flows

- OAuth 2.0 Authorization Code with PKCE (preferred when an admin grants access)
- OAuth 2.0 Client Credentials (preferred for service-to-service enterprise integrations)

### Token Handling

- Encrypt tokens at rest
- Automatic refresh with safe retries
- Support token rotation
- Audit token creation, refresh failures, and revocations

### Secret Storage

- Secrets are referenced, not embedded in config files
- Integrate with secret managers (vault-style refs)
- Per-tenant secret isolation

---

## Resilience and Failure Modes

### IdP Outage

- Active sessions continue
- Break-glass accounts enable incident response
- Paging acknowledgements must still work via SMS/voice

### Token Expiry / Revocation

- Connector enters degraded mode using last-known-good snapshot
- Admin notified with clear remediation steps
n
---

## Auditability

The system must audit:

- SSO configuration changes
- User provisioning and deprovisioning
- Role and permission changes
- OAuth token grants, refreshes, and revocations
- Break-glass access usage

---

## Exit Criteria

This document is complete when:

- Enterprises can onboard with SSO and SCIM without services support
- Integrations use modern OAuth securely
- IdP failures do not block incident response
- All identity actions are auditable

