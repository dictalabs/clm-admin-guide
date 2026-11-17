# Settings

**Settings** covers system-wide configuration: general application details, UI branding, and scheduled/automated tasks.

## General

From the sidebar, select **Settings → General**.

![General Settings](images/settings_page_overview.png)

- **Application Name**, **Company Name**
- **Application URL** — the public URL of your CLM instance.
- **Support Email**
- **Choose Logo** — upload a logo file.

Click **Save Changes** to apply.

## Branding

From the sidebar, select **Settings → Branding**.

![Branding Settings](images/Branding_configuration.png)

Branding is organized into six sections, each with one or more color pickers:

| Section | Field(s) |
|---|---|
| Header Section | Navbar Background Color |
| Menu Section | Sidebar Background Color |
| Main Section | Main Content Background Color |
| Dialogs Section | Dialog Background Color |
| Buttons Section | Primary Color, Secondary Color |
| Text Section | Primary Text, Secondary Text |

Click **Save Changes** to apply, or **Reset to Default** to discard your customizations.

## Scheduler

**Scheduler** manages and automates routine tasks — most commonly, the recurring runs created when you enable **Auto Discovery** on a [Discovery](discovery.md) task.

From the sidebar, select **Settings → Scheduler**.

![Schedulers overview](images/schedulers_page_overview.png)

Summary cards show **Active Schedulers**, **Running Schedulers**, **Failed Schedulers**, and **Total Schedulers**.

### Search and filter

- **Search Schedulers** — by name or description.
- **Frequency**, **Scheduler Type** — narrow the list.
- **Clear All** — resets all filters.

### Schedulers list

| Column | Description |
|---|---|
| Name | Scheduler name — auto-discovery schedulers are named `Auto Discovery: <task name>`. |
| Scheduler Type | e.g. `Auto Discovery`. |
| Frequency | e.g. `Daily`. |
| Enabled | Active or Inactive. |
| Actions | Row-level actions. |

Use **Sync to Beat** (top right) to push the current scheduler configuration to the background task runner.

### Adding a scheduler

1. Click **Add Scheduler** (top right).
2. Fill in the form:

    ![Add Scheduler form](images/schedulers_creating.png)

    - **Name***
    - **Scheduler Type*** — e.g. Certificate Renewal, Log Archiver, Compliance Check, Active Directory Sync, Auto Discovery, Intune Device Certificate Renewal/Sync/Subscription Renewal, or **Scheduled Report** (see below).
    - **Frequency*** — Daily, Weekly, or Monthly. (Scheduled Report does not offer an hourly frequency.)
    - **Time** (UTC), **Day** (for Weekly), **Date** (for Monthly)
    - **Description**
    - **Task Status** — enabled or disabled.

3. Click **Create Scheduler** to save.

!!! tip
    In practice, most schedulers you'll see here are created automatically when an administrator enables **Auto Discovery** on a scan in [Discovery](discovery.md), rather than being hand-built from this screen.

### Scheduled Report

Choosing **Scheduler Type → Scheduled Report** emails one of the [Reports](reports.md) on a recurring basis. It requires permission to manage schedulers and to view reports and certificates — without report access, the schedule's timing and name can still be edited, but not what it sends.

- **Report** — SSL certificates, SSL expiry, CA / connector usage, Key strength, Renewal outcomes, or EJBCA connector usage.
- **Format** — CSV or PDF.
- **Filters** — the same filters available on the live report (e.g. Search, Issuer, Key Algorithm; Expiring within (days) for SSL expiry; Minimum/Maximum key size for Key Strength; Status and Requested in the last (days) for Renewal Outcomes).
- **Recipients*** — search CLM users by name or email and select up to 10; each recipient gets their own email. Only users of the schedule's organization who can view reports are selectable.
- **Mail connector** — **Automatic** (the organization's own SMTP connector, or a shared one) or a specific SMTP connector.

A report-delivery row in the Schedulers list shows a **Last Run** status (Succeeded, Failed, Running, or Not run yet) with a timestamp. Its **⋮** menu adds:

- **Run now** — run the schedule immediately.
- **Run history** — the latest runs, newest first, each with status, timestamp, rows included, how many of the recipients were actually sent, and — on failure — a plain-language reason (e.g. no recipients left who qualify, no usable mail connector, attachment too large, send failed). If a scheduled run fails, its owner also gets an in-app/email notification.

!!! tip
    You can also create a report schedule directly from [Reports](reports.md): open a report's **⋮** menu and choose **Schedule…** — it opens the same fields with the report already selected.
