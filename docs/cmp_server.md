# Managing CMP Server Profiles

The CMP (Certificate Management Protocol) Server module allows administrators to view and manage CMP profiles used for secure certificate enrollment and lifecycle operations.

## Accessing CMP Server Profiles

From the sidebar menu, navigate to **Protocols > CMP Server**.

The CMP Server page opens, displaying an overview of all configured CMP profiles.

![CMP Server Page Overview](images/cmp_server_page_overview.png)

### CMP Server Overview

At the top of the page, administrators can view summary information displayed in cards:

- **Total Orders** – The total number of certificate enrollment orders processed through CMP.
    
- **Success Rate** – The percentage of successful CMP requests.
    
- **Active Orders** – The number of currently active CMP orders.
    
- **Failed Orders** – The number of failed CMP operations.
    
- **Average Processing Time** – The average time taken to process CMP requests.
    
- **Active Accounts** – The number of accounts actively using CMP profiles.

## Search and Filter

Below the summary cards, a **Search and Filter** section allows administrators to:

- Search CMP profiles by name or associated tenant.
    
- Apply filters (e.g., by status, order success rate, or connector).

## CMP Profiles List
The CMP Profiles List table provides detailed information about each CMP profile, including:

- **Profile Name**
    
- **Associated Connector**
    
- **Status (Active/Inactive)**
    
- **Tenant Association**
    
- **Orders (Success/Failed/Total)**
    
- **Actions** (e.g., View, Edit, Delete)
    

This centralized view helps administrators efficiently manage all CMP-based certificate enrollments across the CLM system.

## Creating a New CMP Profile

To add a new CMP profile in CLM, follow these steps:

### 1. Navigate to the CMP Server Page

From the sidebar, select **Protocols > CMP Server**.

On the top-right corner of the page, click the **Add CMP Profile** button.

![Navigate to CMP Server Page](images/navigate_cmp_server_page.png)
### 2. Fill in the CMP Profile Form

A form will appear with the following fields:

- **Name** – Enter a unique name for the CMP profile.
    
- **Connector (Dropdown)** – Select the connector associated with this CMP profile.
    
- **Java Base URL** – Provide the base URL for the CMP server.
    
- **Endpoint Path** – Enter the endpoint path for CMP requests.
    
- **Description** – Provide a brief description of the profile’s purpose.
    
- **Status (Dropdown/Toggle)** – Set the profile status as Active or Inactive.

### 3. Save the CMP Profile

After completing the form, click the **Create CMP Profile** button.

The new CMP profile will be saved and added to the CMP Profiles List.

### 4. Post-Creation

The profile will appear in the CMP Profiles table with its details.








