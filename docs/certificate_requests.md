# Certificate Requests

The **Certificate Requests** module allows administrators to view, approve, or reject certificate enrollment requests submitted by tenants or integrated systems. This ensures that all certificate issuance remains controlled and compliant with organizational policies.

## Accessing Certificate Requests

From the **sidebar menu**, navigate to **Certificate Requests**.

The Certificate Requests page opens, displaying an overview of all pending and processed requests.

![Certificate Requests Page Overview](images/certificate_requests_page_overview.png)

### Certificate Requests Overview

At the top of the page, administrators can view summary information displayed in cards:

- **Total Tenants** – The number of tenants that have submitted certificate requests.
    
- **Total Requests** – The total number of certificate requests received in the system.
    
- **Pending Approval** – The number of requests awaiting administrator review.

### Search and Filter

Below the summary cards, a **Search and Filter** section allows administrators to:

- Search certificate requests by request ID, tenant, domain, or status.
    
- Apply filters (e.g., by status or Tenant).

### Certificate Requests List

The **Certificate Requests List Table** provides detailed information about each request, including:

- **Name**
    
- **Tenant**
    
- **Key**
    
- **Protocol**
    
- **Status** (Pending, Approved, Rejected)
    
- **Creation Date**
    
- **Actions** (e.g., View, Delete)
    

This centralized view helps administrators efficiently manage and control certificate issuance workflows across the CLM system.

## Creating a New Certificate Request
To create a new certificate request in CLM, follow these steps:

1. **Navigate to the Certificate Requests Page**
    

From the sidebar, select **Certificate Requests**.

On the top-right corner of the page, click the **New Request** button.
![Create Certificate Request Form](images/create_certificate_request_form.png)

2. **Fill in the Certificate Request Form**
    

A form will open with the following fields:

- **Common Name** – Enter the primary domain or identifier for the certificate (e.g., `example.com`).
    
- **Select Key (Dropdown)** – Choose the cryptographic key that will be used with this certificate.
    
- **Target Tenant (Dropdown)** – Select the tenant for which the certificate will be issued.
    
    The certificate will be issued for the selected tenant.
    
- **Organization** – Enter the legal name of the organization.
    
- **Organizational Unit** – Specify the department or unit within the organization.
    
- **City** – Enter the city where the organization is based.
    
- **State** – Enter the state or province of the organization.
    
- **Country** – Provide the two-letter ISO country code (e.g., US, PK).
    

3. **Submit the Request**
    

After completing the form, click the **Submit Request** button.

4. **Post-Creation**
    

The new request will appear in the **Certificate Requests List Table** with the status **Pending Approval**.  
An administrator can then review the request and take the appropriate action (Approve or Reject).


