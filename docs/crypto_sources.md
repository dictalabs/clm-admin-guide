# Crypto Sources

**Crypto Sources** define where cryptographic keys are actually generated and stored — a software store, a PKCS#11-compatible HSM, or a cloud key management service. Every key you generate in [Key Management](key_management.md) is created on a specific crypto source.

## Accessing Crypto Sources

From the sidebar, select **Crypto Sources**.

![Crypto Sources page overview](images/crypto_sources_page_overview.png)

### Crypto Sources overview

Summary cards show **Total Crypto Sources**, **Total Store Types**, and **Total Connectors**.

### Search and filter

- **Search Crypto Sources** — by name or store type.
- **Connectors**, **Store Type**, **Organization** — narrow the list.
- **Clear All** — resets all filters.

### Crypto Sources list

| Column | Description |
|---|---|
| Name | The crypto source's name. A colored status indicator (shown after running **Test Connection**) reflects the last reachability check. |
| Store Type | `PKCS#11`, `AWS KMS`, `Azure Key Vault`, or `SOFTWARE`. |
| Connector | The [connector](connectors.md) (typically a Crypto Engine connector) backing this store. |
| Actions | **Test Connection**, **Edit** (PKCS#11 stores only), Delete. |

## Creating a new crypto source

1. Click **Add Crypto Source** (top right).
2. Fill in the base fields:

    ![Add Crypto Source form](images/crypto_source_configuration.png)

    - **Store Name***
    - **Connectors*** — the connector this store operates through.
    - **Store Type*** — `PKCS11`, `AWS KMS`, or `Azure Key Vault`.

3. Selecting a Store Type reveals its specific fields:

    **PKCS11**

    ![PKCS11 crypto source fields](images/crypto_source_pkcs11.png)

    - **Name**, **Password**, **Library** (path to the PKCS#11 library on the crypto engine host), **Vendor**, **Slot**.

    **AWS KMS**

    ![AWS KMS crypto source fields](images/crypto_source_aws.png)

    - **Region**, **Access Key**, **Secret Key**.

    **Azure Key Vault**

    ![Azure Key Vault crypto source fields](images/crypto_source_azure.png)

    - **Key Vault URL**.

4. Click **Add Crypto Source** to save. The new store then appears as an option in the [Generate Key](key_management.md) form's Crypto Source dropdown. CLM automatically runs a connection test right after the store is created.

## Testing a crypto source

From a crypto source row's **⋮** menu, click **Test Connection**. CLM checks connectivity to the store and shows the result both as a toast and as a status indicator next to the source's name:

- **Reachable** — store is accessible (OK).
- **Reachable with warnings** — store responded, but with a warning condition.
- **Not reachable** — store could not be reached (FAILED).

## Editing a crypto source

Only **PKCS#11** stores can be edited (the row action is hidden for other store types, since their credentials are managed by the connector). From the row's **⋮** menu, click **Edit** to open the **Edit Crypto Source** dialog:

- **Store Name**, **Store Type**, **Token Name**, **Vendor**, and **Slot** are read-only — they cannot be changed after creation.
- **Store's PIN** — leave blank to keep the stored PIN; enter a new value to change it. The current PIN is never shown or pre-filled.
- **Library Path** — the path to the PKCS#11 library on the crypto engine host; pre-filled with the current value and editable.

Click **Update Crypto Source** to save. CLM automatically re-runs a connection test afterward to confirm the new PIN/library path work.
