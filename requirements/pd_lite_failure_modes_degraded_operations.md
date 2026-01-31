# PD Lite – Failure Modes & Degraded Operations (v1)

## Design Principles

1. Incidents must remain manageable even when dependencies fail
2. Paging and notifications outrank all other functionality
3. Degraded behavior must be explicit and predictable
4. AI failure must never block human action
5. Incident state must be durable and append-only

---

## Core Invariants

The following must always hold true, even during severe degradation:

- Incident state is never lost
- Responders can always be paged
- Humans can escalate manually
- Timeline events are append-only
- Break-glass access is always available

---

## Failure Mode Matrix

### Chat Platform Outage (Slack / Teams)

**Scenario**  
Chat platform APIs unavailable

**System Behavior**  
- Incident control shifts to Web and Mobile UI
- Notifications and paging continue
- Outbound chat messages queued for replay

**User Experience**  
- Clear banner indicating chat outage
- IC can manage incident via web or mobile

---

### AI System Degradation or Outage

**Scenario**  
LLM or internal AI services unavailable

**System Behavior**  
- AI features disabled
- Manual workflows and templates enabled

**User Experience**  
- Indicator showing AI unavailable
- No blocked actions

---

### Primary Telephony Provider Failure

**Scenario**  
Provider API unreachable or elevated error rates

**System Behavior**  
- Circuit breaker trips
- Automatic failover to secondary provider
- Delivery attempts logged

**User Experience**  
- No visible disruption
- Delivery confirmation preserved

---

### Multiple Telephony Providers Degraded

**Scenario**  
Regional messaging disruption

**System Behavior**  
- Prioritize voice, push, email
- Accelerate escalation policies

**User Experience**  
- Transparent retry messaging
- Manual escalation available

---

### Partial PD Lite Service Outage

**Scenario**  
Single internal service degraded or read-only mode

**System Behavior**  
- Writes queued
- Read-only fallbacks enabled
- Notification pipeline prioritized

**User Experience**  
- Clear degraded-mode banners
- No silent failures

---

### Full Regional Cloud Outage

**Scenario**  
Primary region unavailable

**System Behavior**  
- Region failover
- Telephony independent of region

**User Experience**  
- Short disruption
- Incident context preserved

---

### Identity Provider Outage

**Scenario**  
SSO unavailable

**System Behavior**  
- Break-glass accounts enabled
- Active sessions preserved

**User Experience**  
- ICs continue managing incidents
- Temporary access issuance possible

---

### Web UI Unavailable

**Scenario**  
Frontend or CDN failure

**System Behavior**  
- Chat and mobile control planes remain active

**User Experience**  
- Incident managed via chat

---

## Degraded Mode Summary

| Dependency Down | Incident Mgmt | Paging | Chat | AI | Admin |
|---|---|---|---|---|---|
| Slack | Web/Mobile | Yes | No | Yes | Yes |
| AI | Manual | Yes | Yes | No | Yes |
| Telephony | Yes | Yes (Failover) | Yes | Yes | Yes |
| IdP | Yes (Break-glass) | Yes | Yes | Yes | Limited |
| Region | Yes (Failover) | Yes | Yes | Yes | Yes |

---

## Service Level Objectives

- Paging delivery attempt initiated: under 30 seconds
- Chat command acknowledgment: under 1 second
- Incident state durability: zero loss
- Provider failover detection: under 60 seconds
- Regional failover RTO: under 5 minutes

