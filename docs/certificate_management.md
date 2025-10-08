# Certificate Management

The **Certificate Management** module allows administrators to view, monitor, and manage all issued certificates across tenants. This ensures that active certificates remain compliant, expiring certificates are renewed on time, and any certificates requiring attention are handled proactively.

## Accessing Certificate Management

From the **sidebar menu**, navigate to **Certificate Management**.

The Certificate Management page opens, displaying an overview of all certificates in the system.
![Certificate Management Page Overview](images/certificate_management_page_overview.png)

### Certificate Management Overview

At the top of the page, administrators can view summary information displayed in cards:

- **Total Tenants** – The number of tenants that currently have issued certificates.
    
- **Total Certificates** – The total number of certificates issued and stored in the system.
    
- **Active Certificates** – The number of currently valid and active certificates.
    
- **Need Attention** – The number of certificates that are nearing expiry, revoked, or have compliance issues.

## Search and Filter

Below the summary cards, a **Search and Filter** section allows administrators to:

- Search certificates by domain, tenant, serial number, or status.
    
- Apply filters (e.g., Active, Expiring Soon, Revoked, Expired).


## Certificates List

The **Certificates List Table** provides detailed information about each certificate, typically including:

- **Certificate Name / Common Name**
    
- **Tenant**
    
- **Serial Number**
    
- **Type** (e.g., SSL/TLS, Client, Code Signing)
    
- **Issued On / Expiry Date**
    
- **Status** (Active, Expired, Revoked, Needs Attention)
    
- **Actions** (View, Renew, Revoke, Download)
    

This centralized view helps administrators ensure full visibility into the lifecycle of all certificates managed by the CLM system.

## Creating a New Certificate

To issue a new certificate in CLM:

1. **Navigate to the Certificates Page**  
    From the sidebar, select **Certificates**.  
    On the top-right corner of the page, click the **Issue Certificate** button.
![Create Certificate Form](images/create_certificate_form.png)
2. **Fill in the Certificate Form**  
    A form will appear with the following fields:
    

- **Certificate Purpose (Dropdown)** – Select the purpose of the certificate (e.g., TLS Authentication, Document Signing, Encryption).
    
- **Certificate Authority (Dropdown)** – Choose the Certificate Authority that will issue this certificate.
    
- **Certificate Request (Dropdown)** – Select the related certificate request that this certificate will be issued against.
    

3. **Create the Certificate**  
    After completing the form, click the **Create Certificate** button.
    
4. **Post-Creation**  
    The new certificate will be added to the Certificates List.  
    Certificates can later be renewed, revoked, or reissued as required.


## Managing Certificates

After a certificate has been generated in CLM, administrators can perform additional actions such as downloading, renewing, revoking, and reviewing certificate history.

![Manage Certificates](images/manage_certificates.png)

1. **Viewing Certificate Details**  
    From the Certificates List, click the **View** button on a certificate.  
    A popup will open displaying detailed certificate information.  
    In this popup, admins can also access the **Download Certificate** button to download the issued certificate.
    
2. **Renewing a Certificate**  
    To renew a certificate, click the **Renew** button from the certificate actions.  
    The renewal process will generate a new certificate based on the original request and configuration.
    
3. **Revoking a Certificate**  
    To revoke a certificate, click the **Revoke** button.  
    A popup will appear with the following fields:
    

- **Select Reason (Dropdown)** – Choose the reason for revocation.
    
- **Certificate Authority (Dropdown)** – Select the CA responsible for revocation.
    
- **Reason Message** – Optionally provide additional details.
    

After filling in the details, click the **Revoke Certificate** button to complete the revocation.
![Certificate Details Page](images/certificate_details_page.png)

4. **Viewing Certificate History**  
    Each certificate maintains a full history of actions (creation, renewal, revocation, etc.).  
    Admins can open the **Certificate History** section to review all events associated with the certificate.
    ![Certificate Renewal Workflow](images/certificate_renewal_workflow.png)







