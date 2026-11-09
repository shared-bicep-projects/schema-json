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

---

## Cloud Operation (COP) schemas

The COP schemas validate the `test.parameters.json` files of the COP modules and the COP orchestrator. Modules reference them through jsDelivr:

```
https://cdn.jsdelivr.net/gh/shared-bicep-projects/schema-json@main/schema.<name>.json#
```

Each COP schema covers both the mg-scope and sub-scope deployments of its module (and, for the Virtual Machine and SQL Virtual Machine COPs, their extra entry points). Every COP takes `commonParameters` (required; identity/management subscription GUIDs, `location` and resource-group name overrides), optional `foundationResources` (names of the foundation UAMIs, Log Analytics workspace, action group, automation account) and an optional `cop<Name>Tags` object. The common tags (`Application`, `Environment`, `Owner`, ...) are documented, and any additional tag name is allowed, with a string value of up to 256 characters.

### PaaS COPs

`schema.cop-acs.json`, `schema.cop-afw.json`, `schema.cop-agw.json`, `schema.cop-apim.json`, `schema.cop-app.json`, `schema.cop-ca.json`, `schema.cop-cr.json`, `schema.cop-fd.json`, `schema.cop-lb.json`, `schema.cop-logic.json`, `schema.cop-sbns.json`, `schema.cop-sqldb.json`, `schema.cop-st.json`

Each one validates the matching COP module. They share the same parameters:

| Parameter | Type | Description |
|-----------|------|-------------|
| `policyAuthoringScope` | string | Management group holding the policy definitions. mg-scope only. |
| `managedScopeSet` | array of strings | Management groups receiving the policy assignments. An empty array resolves to `[policyAuthoringScope]`. mg-scope only. |
| `targetSubscriptionIds` | array of strings (uuid) | Subscriptions the policies are assigned to. sub-scope only. |
| `enableRemediation` | boolean | Default `true`: every policy assignment is paired with its remediation task. `false` deploys the assignments without the task. |

### `schema.cop-avd.json`

For the Azure Virtual Desktop COP. Takes the PaaS COP parameters above, plus:

| Parameter | Type | Description |
|-----------|------|-------------|
| `deployExtendedLayer` | boolean | Deploy the Extended-layer artifacts (Autoscale diagnostics policy, session-host alert). Default `true`. |
| `fallbackExpectedHosts` | integer (min 0) | Expected available session hosts for pools with no Autoscale record. Default `0`. |

### `schema.cop-cred.json`

For the Credentials COP. All settings are in one `copCredResources` object: DCE/DCR/alert locations, `lawTableRetentionDays` (4–730), `lawTableTotalRetentionDays` (4–4383), `targetSubscriptionIds` (GUIDs or `"*"`; `kvSubscriptionIds` is the deprecated alias), `collectionScheduleHour` (0–23), and `runbookContentUri` / `runbookContentHash` (uppercase SHA256). The last two have no default.

### `schema.cop-vm.json`

For the Virtual Machine COP and its four entry points: `mg-scope`, `sub-scope`, `backup` and `updates`. All settings are in one `copVmResources` object, and each entry point reads only its own keys:

| Entry point | Keys in `copVmResources` |
|-------------|--------------------------|
| mg-scope | `policyAuthoringScope`, `managedScopeSet`, the `deploy*` switches, `backupGeoCodes`, `alertSettings`, `runbookContentUri` / `runbookContentHash`, `policyAssignmentLocation`, `enableRemediation` |
| sub-scope | `dataCollectionRuleName` |
| backup | `regions` (required), `scheduleTime`, `instantRestoreDays` (1–30), `tiers`, `logAnalyticsWorkspaceResourceId` |
| updates | `patchWindows` (required), `scopeSubscriptionIds` (required), `avDefinitionWindows`, `startDate` |

### `schema.cop-sqlvm.json`

For the SQL Virtual Machine COP and its three entry points: `mg-scope`, `sub-scope` and `backup`. All settings are in one `copSqlvmResources` object:

| Entry point | Keys in `copSqlvmResources` |
|-------------|-----------------------------|
| mg-scope | `policyAuthoringScope`, `managedScopeSet`, the `deploy*` switches, `alertsEnabled`, `tempdbLogUsedPercent`, `runbookContentUri` / `runbookContentHash`, `policyAssignmentLocation`, `enableRemediation` |
| sub-scope | as mg-scope, with `targetSubscriptionIds` instead of the scope keys, plus `remediationDiscoveryMode` |
| backup | `regions` (required), `scheduleTime`, `logFrequencyMinutes`, `tiers`, `vaultRedundancy`, `logAnalyticsWorkspaceResourceId` |

### `schema.co.json`

For the COP orchestrator, which deploys every COP in one deployment.

| Parameter | Type | Description |
|-----------|------|-------------|
| `commonParameters` | object | Required. Forwarded to every COP. Includes a `cop<Name>RgName` override per COP. |
| `policyAuthoringScope` / `managedScopeSet` | string / array | Policy definition and assignment scopes. mg-scope only. |
| `targetSubscriptionIds` | array of strings (uuid) | Target subscriptions. sub-scope only. |
| `foundationResources` | object | Foundation resource names, forwarded to every COP. |
| `coResources` | object | Per-COP settings, keyed by COP abbreviation. `cred`, `vm` and `sqlvm` take that COP's own resources object; `vm` and `sqlvm` don't accept the scope keys, which co sets itself. `avd` takes `deployExtendedLayer` / `fallbackExpectedHosts`. Every PaaS COP key and `activityLog` take `enableRemediation`. |
| `copToggles` | object of booleans | One switch per COP (`acs` … `vm`, `sqlvm`). An absent key defaults to `true`. `sqlvm` requires `vm`. |
