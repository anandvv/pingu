# PD Lite – Service Catalog Connector Spec (Config-Driven) (v1)

## Purpose

Define a versioned, config-driven connector specification that allows PD Lite to integrate with service catalogs and CMDB systems with minimal code changes. The spec supports API polling, webhook ingestion, field mapping, relationship extraction, and normalization into canonical PD Lite service events.

This spec is designed so a new source can be added primarily by adding a new config file.

---

## Design Goals

- Declarative first; configuration should cover most sources
- Stable canonical output; downstream services consume only canonical events
- Safe by default; least privilege and auditability
- Observable; every connector is measurable and debuggable
- Multi-tenant; config is per tenant and isolated

---

## Canonical Output Events

All connectors must normalize into these events:

- `service.upsert`
- `service.delete`
- `dependency.upsert`
- `dependency.delete`
- `ownership.update`

### Canonical Event Envelope

Each canonical event must include:

- `event_type`
- `event_id` (idempotency key)
- `source` (connector name)
- `tenant_id`
- `timestamp_utc`
- `payload` (normalized object)
- `raw_ref` (optional pointer to raw payload stored for audit/debug)

---

## Canonical Service Model (Minimum Fields)

### Service

Required:
- `service_id` (stable internal ID)
- `external_refs` (list of `{system, external_id, url}`)
- `name`
- `owner_team_id` (or `owner_team_name` if ID not available)

Optional:
- `description`
- `tier` (1 to 4 or custom)
- `criticality` (low, medium, high, mission)
- `on_call_ref` (link to schedule, if available)
- `runbook_urls` (list)
- `repo_urls` (list)
- `tags` (list)
- `compliance_classification`

### Dependency

Required:
- `from_service_id`
- `to_service_id`
- `dependency_type` (upstream, downstream, consumes, produces, calls)

Optional:
- `confidence` (0 to 1)
- `source_relationship_type` (raw type)

### Ownership Update

Required:
- `service_id`
- `owner_team_id` or `owner_team_name`

Optional:
- `secondary_owners` (list)
- `escalation_policy_ref`

---

## Connector Spec File Format

The connector config is YAML (or JSON) and is validated against a published schema.

### Top-Level Schema

```yaml
api_version: v1
kind: ServiceCatalogConnector
metadata:
  name: backstage
  display_name: Backstage Catalog
  description: Ingest Backstage entities and relationships
spec:
  auth: {}
  polling: {}
  webhooks: {}
  entities: []
  relationships: []
  mapping: {}
  normalization: {}
  controls: {}
  observability: {}
```

---

## Auth Block

Supported types:
- `api_token`
- `oauth2_client_credentials`
- `basic`
- `mtls`

Credentials are references, not raw secrets.

```yaml
auth:
  type: api_token
  credential_ref: vault://connectors/backstage/token
  headers:
    Authorization: "Bearer {{secret}}"
```

---

## Polling Block

```yaml
polling:
  enabled: true
  schedule:
    full_sync_cron: "0 */12 * * *"        # every 12 hours
    incremental_sync_every_seconds: 300     # every 5 minutes
  delta:
    strategy: cursor                        # cursor | updated_since | etag
    cursor_key: "nextCursor"               # where to find cursor in response
    cursor_storage: connector_state         # persisted per tenant
  rate_limits:
    max_requests_per_minute: 120
    max_concurrent_requests: 4
  retries:
    max_attempts: 5
    backoff_ms: 500
    max_backoff_ms: 10000
    retry_on_status: [429, 500, 502, 503, 504]
  timeouts:
    connect_ms: 2000
    read_ms: 10000
```

---

## Webhooks Block

```yaml
webhooks:
  enabled: true
  endpoint_path: "/connectors/backstage/webhook"
  verification:
    type: hmac_sha256
    header: "X-Signature"
    secret_ref: vault://connectors/backstage/webhook_secret
    timestamp_header: "X-Timestamp"
    max_skew_seconds: 300
  idempotency:
    event_id_path: "$.id"
  routes:
    - name: entity_changed
      match:
        json_path: "$.type"
        equals: "entity.changed"
      transform_ref: "entity_changed_transform"
```

Notes:
- Webhook intake must support replay protection using timestamp windows.
- All webhook events must be deduped using `event_id`.

---

## Entities Block

Defines which objects are fetched and how.

```yaml
entities:
  - name: backstage_entities
    type: service
    fetch:
      method: GET
      url: "{{base_url}}/api/catalog/entities"
      query:
        filter: "kind=Component"
      pagination:
        type: cursor
        cursor_query_param: "cursor"
        limit_query_param: "limit"
        limit: 200
      extract:
        items_json_path: "$.items"
```

---

## Relationships Block

Relationships can be fetched from a dedicated endpoint or derived from entity payloads.

```yaml
relationships:
  - name: backstage_relations
    type: dependency
    source: derived_from_entities
    from_entity_ref: backstage_entities
    extract:
      rules:
        - from_json_path: "$.metadata.name"
          to_json_path: "$.relations[?(@.type=='dependsOn')].targetRef"
          dependency_type: downstream
```

---

## Mapping Block

Mapping defines how to convert source fields into the canonical service model.

```yaml
mapping:
  service:
    id:
      strategy: stable_hash
      inputs:
        - "backstage"
        - "$.metadata.uid"
    external_refs:
      - system: "backstage"
        external_id: "$.metadata.uid"
        url: "{{base_url}}/catalog/default/component/{{$.metadata.name}}"
    name: "$.metadata.name"
    description: "$.metadata.description"
    owner_team_name: "$.spec.owner"
    tags: "$.metadata.tags"
    runbook_urls:
      - "$.metadata.annotations['pagerduty.com/runbook']"
    repo_urls:
      - "$.metadata.annotations['backstage.io/source-location']"
```

Supported mapping features:
- JSONPath extraction
- Default values
- Simple transforms (lowercase, split, join)
- Stable ID derivation strategies

---

## Normalization Block

Normalization specifies how mapped entities become canonical events.

```yaml
normalization:
  emit_on:
    polling:
      full_sync: true
      incremental_sync: true
    webhooks:
      enabled: true
  service_upsert:
    event_id_strategy: content_hash
    content_paths:
      - "$.service_id"
      - "$.name"
      - "$.owner_team_id"
      - "$.tags"
```

---

## Controls Block

```yaml
controls:
  source_of_truth:
    service_fields:
      owner_team: "connector"          # connector | manual | priority_list
      tier: "manual"
  manual_overrides:
    enabled: true
    require_reason: true
  drift_detection:
    enabled: true
    notify_on_drift: true
  scope:
    allow_namespaces: ["default"]
    allow_tags: ["production", "tier:1", "tier:2"]
```

---

## Observability Block

```yaml
observability:
  metrics:
    - name: last_successful_sync_timestamp
    - name: items_processed
    - name: errors_by_type
    - name: api_latency_ms
    - name: webhook_lag_seconds
  logs:
    include_raw_payload_refs: true
  alerts:
    - name: connector_stale
      condition: "now() - last_successful_sync_timestamp > 3600"
      severity: high
```

---

## Example Connector: Backstage

```yaml
api_version: v1
kind: ServiceCatalogConnector
metadata:
  name: backstage
  display_name: Backstage Catalog
  description: Ingest Backstage components and relationships
spec:
  base_url: "https://backstage.example.com"
  auth:
    type: api_token
    credential_ref: vault://connectors/backstage/token
    headers:
      Authorization: "Bearer {{secret}}"

  polling:
    enabled: true
    schedule:
      full_sync_cron: "0 */12 * * *"
      incremental_sync_every_seconds: 300
    delta:
      strategy: cursor
      cursor_key: "nextCursor"
      cursor_storage: connector_state
    rate_limits:
      max_requests_per_minute: 120
      max_concurrent_requests: 4
    retries:
      max_attempts: 5
      backoff_ms: 500
      max_backoff_ms: 10000
      retry_on_status: [429, 500, 502, 503, 504]

  webhooks:
    enabled: false

  entities:
    - name: components
      type: service
      fetch:
        method: GET
        url: "{{base_url}}/api/catalog/entities"
        query:
          filter: "kind=Component"
        pagination:
          type: cursor
          cursor_query_param: "cursor"
          limit_query_param: "limit"
          limit: 200
        extract:
          items_json_path: "$.items"

  relationships:
    - name: depends_on
      type: dependency
      source: derived_from_entities
      from_entity_ref: components
      extract:
        rules:
          - from_json_path: "$.metadata.uid"
            to_json_path: "$.relations[?(@.type=='dependsOn')].targetRef"
            dependency_type: downstream

  mapping:
    service:
      id:
        strategy: stable_hash
        inputs: ["backstage", "$.metadata.uid"]
      external_refs:
        - system: "backstage"
          external_id: "$.metadata.uid"
          url: "{{base_url}}/catalog/default/component/{{$.metadata.name}}"
      name: "$.metadata.name"
      description: "$.metadata.description"
      owner_team_name: "$.spec.owner"
      tags: "$.metadata.tags"
      runbook_urls:
        - "$.metadata.annotations['pagerduty.com/runbook']"

  normalization:
    emit_on:
      polling:
        full_sync: true
        incremental_sync: true
      webhooks:
        enabled: false

  controls:
    manual_overrides:
      enabled: true
      require_reason: true

  observability:
    alerts:
      - name: backstage_connector_stale
        condition: "now() - last_successful_sync_timestamp > 3600"
        severity: high
```

---

## Example Connector: Generic REST CMDB (ServiceNow-like)

This template assumes:
- A CI table endpoint
- A relationship endpoint
- Updated-since deltas
- Optional webhooks

```yaml
api_version: v1
kind: ServiceCatalogConnector
metadata:
  name: generic_cmdb
  display_name: Generic CMDB
  description: Ingest CIs, ownership, and dependencies from a REST CMDB
spec:
  base_url: "https://cmdb.example.com"

  auth:
    type: oauth2_client_credentials
    token_url: "{{base_url}}/oauth_token"
    client_id_ref: vault://connectors/cmdb/client_id
    client_secret_ref: vault://connectors/cmdb/client_secret
    scopes: ["cmdb.read"]

  polling:
    enabled: true
    schedule:
      full_sync_cron: "0 2 * * *"                 # daily at 2am
      incremental_sync_every_seconds: 600          # every 10 minutes
    delta:
      strategy: updated_since
      updated_since_param: "sys_updated_on>="
      updated_since_storage: connector_state
    rate_limits:
      max_requests_per_minute: 60
      max_concurrent_requests: 2
    retries:
      max_attempts: 6
      backoff_ms: 750
      max_backoff_ms: 15000
      retry_on_status: [429, 500, 502, 503, 504]

  webhooks:
    enabled: true
    endpoint_path: "/connectors/generic_cmdb/webhook"
    verification:
      type: hmac_sha256
      header: "X-Signature"
      secret_ref: vault://connectors/cmdb/webhook_secret
      timestamp_header: "X-Timestamp"
      max_skew_seconds: 300
    idempotency:
      event_id_path: "$.event_id"
    routes:
      - name: ci_changed
        match:
          json_path: "$.type"
          equals: "ci.changed"
        transform_ref: "ci_changed_transform"

  entities:
    - name: cis
      type: service
      fetch:
        method: GET
        url: "{{base_url}}/api/now/table/cmdb_ci_service"
        query:
          sysparm_limit: 500
          sysparm_fields: "sys_id,name,short_description,owned_by,criticality,sys_updated_on"
        pagination:
          type: offset
          offset_query_param: "sysparm_offset"
          limit_query_param: "sysparm_limit"
          limit: 500
        extract:
          items_json_path: "$.result"

  relationships:
    - name: ci_relations
      type: dependency
      source: api
      fetch:
        method: GET
        url: "{{base_url}}/api/now/table/cmdb_rel_ci"
        query:
          sysparm_limit: 1000
          sysparm_fields: "parent,child,type,sys_updated_on"
        pagination:
          type: offset
          offset_query_param: "sysparm_offset"
          limit_query_param: "sysparm_limit"
          limit: 1000
        extract:
          items_json_path: "$.result"
      map:
        from_external_id: "$.parent.value"
        to_external_id: "$.child.value"
        dependency_type: "$.type.value"

  mapping:
    service:
      id:
        strategy: stable_hash
        inputs: ["generic_cmdb", "$.sys_id"]
      external_refs:
        - system: "generic_cmdb"
          external_id: "$.sys_id"
          url: "{{base_url}}/nav_to.do?uri=cmdb_ci_service.do?sys_id={{$.sys_id}}"
      name: "$.name"
      description: "$.short_description"
      owner_team_name: "$.owned_by.display_value"
      criticality: "$.criticality"

  normalization:
    emit_on:
      polling:
        full_sync: true
        incremental_sync: true
      webhooks:
        enabled: true

  controls:
    source_of_truth:
      service_fields:
        owner_team: "connector"
        criticality: "connector"
        tier: "manual"

  observability:
    alerts:
      - name: cmdb_connector_stale
        condition: "now() - last_successful_sync_timestamp > 7200"
        severity: high
```

---

## Implementation Notes

- Connector configs are stored per tenant and require admin permissions to modify.
- All config changes are audited and versioned.
- A validation step runs before activation, including auth checks and dry-run fetch.
- Webhook intake endpoints are isolated per connector and tenant.
- Raw payloads should not be stored by default; store references when needed for audit/debug.

---

## Exit Criteria

This spec is complete when:

- A new REST service catalog can be added via config only
- Webhook events can be verified, deduped, and normalized safely
- Polling and reconciliation produce consistent canonical events
- Connector behavior is observable and debuggable

