# Claude.md — Schema Repository Instructions

## Purpose

This repository (`schema-json`) hosts the published JSON Schema files used to validate ARM parameter files for Azure Bicep modules. Schemas are served via jsDelivr CDN and referenced by `test.parameters.json` files across the bicep modules repo.

---

## Bicep Modules Source

The authoritative source for schema content is the bicep modules repository:

```
C:\CloudPowerHouse\IaC\Github-IaC-Bicep-Modules\modules
```

Each module follows this structure:

```
modules/
  iac-bicep-<type>-<name>/
    test/
      mg/           (or mg-scope/)
        default/
          schema.json           ← source for mg-scope default schema
          test.parameters.json
        full-parameters/
          schema.json           ← source for mg-scope full-parameters schema
          test.parameters.json
      sub/          (or sub-scope/)
        default/
          schema.json           ← source for sub-scope default schema
          test.parameters.json
        full-parameters/
          schema.json           ← source for sub-scope full-parameters schema
          test.parameters.json
```

---

## Creating or Updating Schema Files

### When `schema.json` is present in the test folder

The module has **not yet been migrated**. The schema file in this repo should be created or updated by consolidating the content from those local `schema.json` files.

Steps:
1. Locate all `schema.json` files under the module's `test/` folder.
2. Use the `full-parameters` variant as the primary source — it contains the most complete schema.
3. Create or update the corresponding file in this repo using the naming convention below.

### When `schema.json` is NOT present in the test folder

The module has been **fully migrated** to this repo. The `test.parameters.json` in the module now references the CDN URL directly:

```json
{
  "$schema": "https://cdn.jsdelivr.net/gh/shared-bicep-projects/schema-json@main/schema.<name>.json#"
}
```

This confirms the schema is already live in this repo and no local `schema.json` exists in the module's test folder.

---

## Naming Convention

Schema files in this repo are named after the module type:

| Module folder                          | Schema file in this repo                   |
|----------------------------------------|--------------------------------------------|
| `iac-bicep-cop-cred`                   | `schema.cop-cred.json`                     |
| `iac-bicep-cop-ca`                     | `schema.cop-ca.json`                       |
| `iac-bicep-cop-agw`                    | `schema.cop-agw.json`                      |
| `iac-bicep-cop-apim`                   | `schema.cop-apim.json`                     |
| `iac-bicep-cop-app`                    | `schema.cop-app.json`                      |
| `iac-bicep-cop-cr`                     | `schema.cop-cr.json`                       |
| `iac-bicep-cop-cs`                     | `schema.cop-cs.json`                       |
| `iac-bicep-cop-fd`                     | `schema.cop-fd.json`                       |
| `iac-bicep-cop-lb`                     | `schema.cop-lb.json`                       |
| `iac-bicep-cop-sb`                     | `schema.cop-sb.json`                       |
| `iac-bicep-cop-sqldb`                  | `schema.cop-sqldb.json`                    |
| `iac-bicep-cop-st`                     | `schema.cop-st.json`                       |
| `iac-bicep-cop-vm`                     | `schema.cop-vm.json`                       |
| `iac-bicep-foundation`                 | `schema.foundation.json`                   |
| `iac-bicep-foundation-platform-aa`     | `schema.foundation-platform-aa.json`       |
| `iac-bicep-foundation-platform-ag`     | `schema.foundation-platform-ag.json`       |
| `iac-bicep-foundation-platform-id`     | `schema.foundation-platform-id.json`       |
| `iac-bicep-foundation-platform-law`    | `schema.foundation-platform-law.json`      |
| `iac-bicep-landing-zone`               | `schema.landingzone.json`                  |
| `iac-bicep-prepacked-network`          | `schema.prepacked-network.json`            |

---

## CDN URL Pattern

Once a schema is published to this repo, it is available via jsDelivr at:

```
https://cdn.jsdelivr.net/gh/shared-bicep-projects/schema-json@main/schema.<name>.json#
```

Examples:
- `https://cdn.jsdelivr.net/gh/shared-bicep-projects/schema-json@main/schema.cop-cred.json#`
- `https://cdn.jsdelivr.net/gh/shared-bicep-projects/schema-json@main/schema.cop-ca.json#`

---

## Migration Checklist

When migrating a module's schema from its test folder to this repo:

1. Copy the consolidated schema content into a new file in this repo using the correct naming convention.
2. Update the `$schema` field in every `test.parameters.json` under the module's `test/` folder to point to the CDN URL.
3. Delete all `schema.json` files from the module's `test/` folder.
4. Verify `test.parameters.json` files validate correctly against the new CDN-hosted schema.

---

## Currently Migrated Modules

The following modules no longer have `schema.json` in their test folders — their schemas are live in this repo:

- `iac-bicep-cop-cred` → `schema.cop-cred.json`
- `iac-bicep-cop-agw` → `schema.cop-agw.json`
- `iac-bicep-cop-apim` → `schema.cop-apim.json`
- `iac-bicep-cop-app` → `schema.cop-app.json`
- `iac-bicep-cop-ca` → `schema.cop-ca.json`
- `iac-bicep-cop-cr` → `schema.cop-cr.json`
- `iac-bicep-cop-cs` → `schema.cop-cs.json`
- `iac-bicep-cop-fd` → `schema.cop-fd.json`
- `iac-bicep-cop-lb` → `schema.cop-lb.json`
- `iac-bicep-cop-sb` → `schema.cop-sb.json`
- `iac-bicep-cop-sqldb` → `schema.cop-sqldb.json`
- `iac-bicep-cop-st` → `schema.cop-st.json`
- `iac-bicep-cop-vm` → `schema.cop-vm.json`
- `iac-bicep-foundation` → `schema.foundation.json`
- `iac-bicep-foundation-platform-aa` → `schema.foundation-platform-aa.json`
- `iac-bicep-foundation-platform-ag` → `schema.foundation-platform-ag.json`
- `iac-bicep-foundation-platform-id` → `schema.foundation-platform-id.json`
- `iac-bicep-foundation-platform-law` → `schema.foundation-platform-law.json`
- `iac-bicep-landing-zone` → `schema.landingzone.json`
- `iac-bicep-prepacked-network` → `schema.prepacked-network.json`

## Pending Migration

All known `iac-bicep-cop-*` and `iac-bicep-foundation*` modules have been migrated. No modules currently have local `schema.json` files pending migration.
