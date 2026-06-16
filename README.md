# schema-json

A collection of JSON Schema files used to validate configuration and parameter files for Azure Bicep modules.

## Schemas

### `schema.version.json`

Validates version files for Bicep modules, following [Azure Verified Modules (AVM)](https://azure.github.io/Azure-Verified-Modules/) versioning conventions.

**Reference URL:**
```
https://raw.githubusercontent.com/schema-json/schema-json/main/schema.version.json#
```

**Required fields:**

| Field | Type | Description |
|-------|------|-------------|
| `$schema` | string | Must reference this schema file. |
| `version` | string | Semantic version string (`major.minor.patch[-prerelease]`). |

**Example:**
```json
{
  "$schema": "https://raw.githubusercontent.com/schema-json/schema-json/main/schema.version.json#",
  "version": "1.0.0"
}
```

---

### `schema.landingzone.json`

Validates ARM parameter files for the `iac-bicep-landing-zone` Bicep module. Defines the expected structure for management group hierarchies, Azure subscriptions, and Azure AD principal assignments across a Landing Zone deployment.

**Reference URL:**
```
https://raw.githubusercontent.com/schema-json/schema-json/main/schema.landingzone.json#
```

**Required top-level parameters:**

| Parameter | Description |
|-----------|-------------|
| `rootCompany` | Root management group, with optional `reader`/`contributor` principal arrays. |
| `landingzone` | Landing zone management group (no subscriptions). |
| `prod` | Production management group with optional subscriptions. |
| `platform` | Platform management group (no subscriptions). |
| `connectivity` | Connectivity management group with optional subscriptions. |
| `identity` | Identity management group with optional subscriptions. |
| `management` | Management management group with optional subscriptions. |

**Optional parameters:** `test`, `avd`, `decomissioned`

Each management group object supports:
- `subscriptions` — array of Azure subscription IDs (where applicable)
- `reader` — array of Azure AD principal (group/user/SP) object IDs
- `contributor` — array of Azure AD principal (group/user/SP) object IDs

---

### `schema.foundation-platform-ag.json`

Validates ARM parameter files for the `iac-bicep-foundation-platform-ag` module. Defines the expected structure for deploying an Azure Monitor Action Group in the platform foundation.

**Reference URL:**
```
https://raw.githubusercontent.com/schema-json/schema-json/main/schema.foundation-platform-ag.json#
```

**Parameters** (all optional at the top level):

| Parameter | Type | Description |
|-----------|------|-------------|
| `subscriptionGuId` | string (uuid) | Target subscription ID. |
| `resourceGroupName` | string | Resource group name. |
| `customerNavId` | string | Customer navigation identifier. |
| `actionGroupName` | string | Display name of the action group. |
| `actionGroupShortName` | string (max 12) | Short name of the action group (max 12 characters). |
| `webhookBaseUri` | string | Base URI for the webhook receiver. |
| `functionCode` | string | Function authorization code for the webhook. |
| `tags` | object | Tags to apply to deployed resources. |

---

### `schema.foundation-platform-id.json`

Validates ARM parameter files for the `iac-bicep-foundation-platform-identity` module. Defines User-Defined Managed Identities (UDMIs) and role assignments across platform subscriptions.

**Reference URL:**
```
https://raw.githubusercontent.com/schema-json/schema-json/main/schema.foundation-platform-id.json#
```

**Required parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `identitySubscriptionGuid` | string (uuid) | Subscription ID of the ALZ Identity subscription (Platform MG). |
| `managementSubscriptionGuid` | string (uuid) | Subscription ID of the ALZ Management subscription (Platform MG). |

**Optional parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `location` | string | Azure region for UDMI resources (e.g. `westeurope`). |
| `identityRgName` | string | Resource Group name for platform UDMIs in the Identity subscription. |
| `targetSubscriptionGuids` | array of strings (uuid) | All subscription IDs that should receive role assignments. Minimum 1 item. |
| `tags` | object | Tags applied to all UDMI resources. |

---

### `schema.foundation-platform-law.json`

Validates ARM parameter files for the `iac-bicep-foundation-platform-law` module. Defines the expected structure for deploying a Log Analytics Workspace in the platform foundation.

**Reference URL:**
```
https://raw.githubusercontent.com/schema-json/schema-json/main/schema.foundation-platform-law.json#
```

**Required parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `location` | string | Azure region to deploy the workspace. |

**Optional parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `subscriptionGuid` | string (uuid) | Target subscription ID. |
| `resourceGroupName` | string | Resource group name. |
| `workspaceName` | string | Name of the Log Analytics Workspace. |
| `retentionInDays` | integer (30–730) | Data retention period in days. |
| `dailyQuotaGb` | integer (min -1) | Daily ingestion cap in GB. Use `-1` for no limit. |
| `tags` | object | Tags to apply to deployed resources. |

---

### `schema.prepacked-connectivity.json`

Validates ARM parameter files for the `iac-bicep-prepacked-connectivity` module. Defines the hub-and-spoke connectivity topology including per-scope subscription assignments and tags.

**Reference URL:**
```
https://raw.githubusercontent.com/schema-json/schema-json/main/schema.prepacked-connectivity.json#
```

**Required parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `companyPrefix` | string (2–5 chars) | Prefix for all deployed resource names. |
| `connectivity` | object | Connectivity scope. Must include a `subscription` GUID. |

**Optional parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `identity` | object | Identity scope with `subscription` (UUID or empty string) and optional `tags`. |
| `management` | object | Management scope with `subscription` (UUID or empty string) and optional `tags`. |
| `avd` | object | AVD scope with `subscription` (UUID or empty string) and optional `tags`. |
| `prod` | object | Production scope with `subscription` (UUID or empty string) and optional `tags`. |
| `test` | object | Test scope with `subscription` (UUID or empty string) and optional `tags`. |
| `tags` | object | Tags applied to all deployed resources. Scope-level tags take precedence on conflict. |

---

### `schema.prepacked-network.json`

Validates ARM parameter files for the `iac-bicep-prepacked-network` module. Extends the connectivity schema with an option to deploy a basic Azure Firewall in the hub.

**Reference URL:**
```
https://raw.githubusercontent.com/schema-json/schema-json/main/schema.prepacked-network.json#
```

**Required parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `companyPrefix` | string (2–5 chars) | Prefix for all deployed resource names. |
| `connectivity` | object | Connectivity scope. Must include a `subscription` GUID. Supports `deployBasicFW` (boolean, default `true`) to control basic firewall deployment. |

**Optional parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `identity` | object | Identity scope with `subscription` (UUID or empty string) and optional `tags`. |
| `management` | object | Management scope with `subscription` (UUID or empty string) and optional `tags`. |
| `avd` | object | AVD scope with `subscription` (UUID or empty string) and optional `tags`. |
| `prod` | object | Production scope with `subscription` (UUID or empty string) and optional `tags`. |
| `test` | object | Test scope with `subscription` (UUID or empty string) and optional `tags`. |
| `tags` | object | Tags applied to all deployed resources. Scope-level tags take precedence on conflict. |
