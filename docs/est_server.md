# Managing EST Server

## Overview
*(Paste the full EST Server Overview from the PDF — describe what EST (Enrollment over Secure Transport) is, how it functions in CLM, and its role in secure certificate enrollment.)*

## Accessing EST Server
*(Insert the detailed instructions for navigating to the EST Server module in the CLM Admin Portal.)*
![EST Server Page Overview](images/est_server_page_overview.png)

## Search and Filter
*(Paste the content explaining how to use the search and filter options for EST profiles.)*
- Use the search bar to locate specific EST profiles by name or associated CA.  
- Apply filters for status, creation date, or profile attributes.  
![EST Server Search and Filter](images/est_server_search_filter.png)

## EST Profiles List
*(Paste the section from the PDF describing the EST profiles list view — include table columns and actions.)*
- Profile Name  
- Associated CA  
- Authentication Method  
- Status  
- Actions (View, Edit, Delete)
![EST Profiles List](images/est_profiles_list.png)

## Creating a New EST Profile
*(Paste the complete step-by-step process for creating a new EST profile, replacing numbered steps with bullets.)*
- Click **Add Profile** or **Create New EST Profile**.  
- Enter the profile name and select the issuing CA.  
- Configure authentication and certificate issuance parameters.  
- Define allowed key usages and validity period.  
- Save to create the EST profile.  
![Create EST Profile Form](images/create_est_profile_form.png)

## Navigate to the EST Server Page
*(Include any detailed navigation description or screenshot from the PDF.)*
![Navigate to EST Server Page](images/navigate_est_server_page.png)

## Fill in the EST Profile Form
*(Paste detailed breakdown of the configuration fields.)*
- Profile Name  
- Description  
- Associated CA  
- Authentication Type (Basic, TLS, or None)  
- Allowed Key Usages  
- Validity Period  
![EST Profile Form Fields](images/est_profile_form_fields.png)

## Save the EST Profile
*(Include instructions for validation and saving.)*
- Review the profile settings.  
- Click **Save** to confirm.  
- The profile appears in the list view after creation.  
![EST Profile Saved Confirmation](images/est_profile_saved_confirmation.png)

## Post-Creation
*(Paste the section describing post-creation verification, testing, or integration.)*
- Test the profile using an EST client.  
- Monitor logs for enrollment requests.  
- Verify certificate issuance flow.  
![EST Profile Test Results](images/est_profile_test_results.png)
