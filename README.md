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
