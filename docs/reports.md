# Reports

**Reports** provides pre-built, filterable reports across your certificate inventory, organized into three categories: **Certificate Reports**, **CA Reports**, and **Common Reports**. Each report is presented as a launcher card — click **Open report** to view it.

## Certificate Reports

From the sidebar, select **Reports → Certificate Reports**.

![Certificate Reports](images/reports_page_overview.png)

- **Certificate Report** — generates a list of certificates discovered, imported, or created.
- **Certificate Expiry Report** — certificates expiring within N days.

## CA Reports

From the sidebar, select **Reports → CA Reports**.

![CA Reports](images/reports_dashboard_3.png)

- **CA / Connector Usage** — per-connector certificate counts and expiries.
- **EJBCA Connector Usage** — a focused view for EJBCA connectors specifically.

## Common Reports

From the sidebar, select **Reports → Common Reports**.

![Common Reports](images/reports_dashboard_4.png)

- **Key Strength** — key algorithms and sizes across certificates.
- **Renewal Outcomes** — requested, completed, and failed renewals.

## Using a report

Click **Open report** on any card to view it:

![Certificate report opened](images/reports_dashboard_1.png)

- Use the **Search** box and report-specific filters (e.g. Issuer, Key Algorithm) to narrow the results.
- Click **Export** (top right) to download the current results, or use the **⋮** menu on a report's launcher card for additional options.

## Scheduling a report by email

Open a report's **⋮** menu on its launcher card and choose **Schedule…** to have CLM generate and email it on a recurring basis, instead of exporting it by hand each time.

- **Schedule name** — defaults to "`<report>` report".
- **Frequency** — Daily, Weekly (pick a **Day**), or Monthly (pick a **Day of month**, 1–28).
- **Time** — UTC.
- **Report**, **Format** (CSV/PDF), and the report's own filters — preset to the report you launched this from, but changeable.
- **Recipients*** — up to 10 CLM users, searched by name or email.
- **Mail connector** — **Automatic**, or a specific SMTP connector.

Click **Create schedule** to save. The schedule then appears on [Settings → Scheduler](settings.md#scheduled-report), where you can edit it, run it on demand, or review its run history.
