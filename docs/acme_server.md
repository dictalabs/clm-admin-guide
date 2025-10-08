# Managing ACME Server

The ACME Server module allows administrators to manage Automatic Certificate Management Environment (ACME) profiles, process certificate orders, and monitor enrollment activity.

## Accessing ACME Server

From the sidebar menu, navigate to **Protocols > ACME Server**.

The ACME Server page opens, displaying an overview of all configured ACME profiles.

![ACME Server Page Overview](images/acme_server_page_overview.png)

### ACME Server Overview

At the top of the page, administrators can view summary information displayed in cards:

- **Total Orders** – The total number of certificate orders processed through ACME.
    
- **Success Rate** – The percentage of successfully completed certificate orders.
    
- **Active Orders** – The number of ongoing or pending ACME orders.
    
- **Failed Orders** – The number of certificate orders that failed during processing.
    
- **Avg Processing** – The average time taken to process an ACME order.
    
- **Active Accounts** – The total number of active ACME accounts registered.
## Search and Filter

 Below the summary cards, a **Search and Filter** section allows administrators to:

- Search ACME server profiles by name, tenant, or keyword.
    
- Apply filters (e.g., by order status, account, or date range).

## ACME Profiles List

The ACME server profiles list table provides detailed information about each configured profile, typically including:

- **Profile Name**
    
- **Directory Endpoint**
    
- **Challenges**
    
- **Status (Active/Inactive)**
    
- **Rate Limits**
    
- **Connector**
    
- **Actions** (e.g., View, Edit, Disable, or Delete)
    

This centralized view helps administrators manage ACME-based certificate automation efficiently across multiple tenants.

## Creating a New ACME Profile

To configure a new ACME profile in CLM, follow these steps:

### 1. Navigate to the ACME Server Page

From the sidebar menu, select **Protocols > ACME Server**.

On the top-right corner of the page, click the **Add ACME Profile** button.
![Create ACME Profile Form](images/create_acme_profile_form.png)
### 2. Fill in the ACME Profile Form

A form will open with the following fields:

- **Endpoint Name** – Enter a unique name for the ACME endpoint.
    
- **Directory Path** – Specify the ACME directory path used for order and account requests.
    
- **Connector (Dropdown)** – Select the connector associated with this ACME profile.
    
- **Website URL** – Provide the URL of the website or service linked to this ACME configuration.
    
- **Challenge Type (Dropdown)** – Select the ACME challenge type (e.g., HTTP-01, DNS-01).
    
- **Status (Dropdown)** – Choose whether the ACME profile is Active or Inactive.


### 3. Save the Profile

After completing the form, click the **Create Profile** button.

### 4. Post-Creation

The new ACME profile will appear in the **ACME Server Profiles List** with its details.




