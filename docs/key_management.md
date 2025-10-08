# Key Management

The **Key Management** module allows administrators to manage cryptographic keys used for document signing, TLS authentication, and data encryption.

## Accessing Key Management

From the **sidebar menu**, navigate to **Key Management**.

The Key Management page opens, displaying an overview of all configured keys.
![Key Management Page Overview](images/key_management_page_overview.png)


### Key Management Overview

At the top of the page, administrators can view summary information displayed in cards:

- **Total Keys** – The total number of cryptographic keys managed.
    
- **Active Keys** – The number of keys currently active and available for use.
    
- **RSA Keys** – The number of keys using the RSA algorithm.
    
- **ECDSA Keys** – The number of keys using the ECDSA algorithm.

### Search and Filter

Below the summary cards, a **Search and Filter** section allows administrators to:

- Search keys by name, type, or tenant.
    
- Apply filters (e.g., by algorithm, status, or usage).

### Keys List

The **keys list table** provides detailed information about each key, typically including:

- **Key Name**
    
- **Algorithm** (e.g., RSA, ECDSA)
    
- **Key Size / Curve**
    
- **Associated Crypto Source**
    
- **Usage** (e.g., Signing, Encryption, TLS)
    
- **Status** (Active/Inactive)
    
- **Tenant Association**
    
- **Actions** (e.g. Delete)
    

This centralized view enables administrators to securely manage all cryptographic keys across the CLM system.

## Creating a New Key

To define a new cryptographic key in CLM, follow these steps:

1. **Navigate to the Key Management Page**
    

From the sidebar, select **Key Management**.

On the top-right corner of the page, click the **Generate Key** button.

2. **Fill in the Create Key Form**
    

A form will open with the following fields:

- **Key Name** – Enter a unique name for the key.
    
- **Key Password** – Provide a password to protect the key material (if required by the crypto source).
    
- **Description** – Enter a short description of the key’s purpose.
    
- **Key Purpose (Dropdown)** – Select the intended purpose of the key (e.g., Signing, Encryption, TLS/Authentication, Multi-purpose).
    
- **Crypto Source (Dropdown)** – Select the configured crypto source (e.g., HSM, AWS KMS, Azure Key Vault) where the key will be stored or generated.
    
- **Algorithm (Dropdown)** – Choose the algorithm for the key (e.g., RSA, ECDSA).
    
- **Key Size (Dropdown)** – Select the key size or curve, depending on the chosen algorithm (e.g., 2048/3072/4096 for RSA, or P-256/P-384 for ECDSA).

![Create Key Form](images/create_key_form.png)

3. **Save the Key**
    

- After completing the form, click the **Create Key** button.
    

4. **Post-Creation**
    

- The new key will be added to the **Keys List Table**
  










