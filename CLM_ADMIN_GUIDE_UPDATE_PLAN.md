# CLM Admin Guide — Update Plan

**Status:** Working plan for this update pass. Not part of the published guide — kept separate per the task instructions.
**Method:** The live application (`https://dev.clm.certinium.com/`, admin login) was driven with a scripted Chromium browser (Playwright) to log in and visit every sidebar route, open every primary "Create/Add" form, and capture full-page screenshots plus DOM field dumps (labels, placeholders, dropdown options, buttons). The existing guide's 24 markdown files were independently read and catalogued in full. Backend (`D:\clm\clm-server`) and frontend (`D:\clm\clm-web-app`) source were consulted for anything not fully verifiable from the UI (permission strings, version, deployment/env details). Nothing was submitted/saved in the live app — all forms were opened, inspected, and cancelled, so no test data was created.

**Headline finding:** The application has changed substantially since the old guide was written. Navigation, terminology, and functionality have all moved. Roughly a third of the current app (Approvals, most of Discovery, MDM, Active Directory publishing, CSC Configuration as its own page, several connector/crypto-source types, post-quantum key algorithms, API Key Logs, Common/CA Reports) has **no documentation at all** today. This is a rewrite, not a copy-edit.

---

## 1. Old vs. current navigation

| Old guide (mkdocs.yml nav) | Current live app sidebar | Change |
|---|---|---|
| Home | Dashboard | Content changed (see §4) |
| Install CLM | *(no in-app equivalent — ops runbook)* | Rewrite from source, see §3 |
| RBAC → Tenants | Organizations | Renamed; form fields simplified (no more Domain/API Access toggle) |
| RBAC → Roles | Roles | Now organization-scoped only (no "Global vs Tenant" split); full permission tree confirmed |
| RBAC → Users | Operators | Renamed; form now has SSO / Mutual TLS auth options, phone-with-country-code, force-password-change |
| — | **Approvals** | **New page**, not documented at all |
| Connectors | Connectors | Kept, but connector types expanded from 3 to 9 |
| Crypto Sources | Crypto Sources | Kept, store types unchanged (PKCS11 / AWS KMS / Azure Key Vault) |
| Key Management | Key Management | Kept, but now includes post-quantum algorithms (ML-DSA) not in old guide |
| Certificate Requests | Certificate Requests | Kept, form fields changed (Requires Approval flag added) |
| Certificate Management | **Certificates** | Renamed; dashboard cards changed |
| Discovery *(single SSL-only page)* | **Discovery → SSL / File Scan / SSH / AD CS / Proxy / AWS / Azure / GCP** | Massively expanded: 1 page → 8 dedicated scan types |
| Compliance | Compliance Profiles | Renamed; 3-tab structure (Profiles/Rules/Analytics) confirmed accurate in old guide, needs field updates |
| Protocols → SCEP/ACME/EST/CMP | Protocols → SCEP/ACME/EST/CMP | Kept, all 4 profile forms have significantly more fields now (Publish to AD, Require Manual Approval, EAB, rate limits, mTLS, etc.) |
| — | **MDM → Enrollment Profiles, Device Certificates** | **New section** (Microsoft Intune integration), not documented at all |
| — | **Active Directory** | **New page** — certificates published to AD/LDAP identities, not documented at all |
| API Integration → CSC Configuration | **CSC Configuration** (own sidebar page) | Split out of "API Integration"; fields changed |
| API Integration → API Keys | **API Keys** (own sidebar page) | Split out; permission list is now a large fixed checklist (60+ scoped permissions), not free-text |
| Reports → SSL Reports, CA Reports | Reports → **Certificate Reports, CA Reports, Common Reports** | Restructured into a report-launcher pattern; "Common Reports" (Key Strength, Renewal Outcomes) is new |
| Notifications *(+ orphan `alert.md`)* | **Alerts** | The app only has "Alerts" — confirms `alert.md` was the accurate/current file and `notifications.md` is obsolete duplicate content |
| Audit Logs → Operation, Discovery | Audit → Operation Logs, Discovery Logs, **API Key Logs** | "API Key Logs" is new |
| Settings → General, Branding, Schedulers | Settings → General, Branding, Scheduler | Kept; General fields changed (added Application URL, Support Email); Branding is now 8 colour pickers in 6 sections (not 6 flat fields); Scheduler is now mostly auto-populated by Discovery auto-discovery tasks, "Add Scheduler" form confirmed |
| User Profile | Profile (via account menu) | Kept; **Timezone field no longer exists**; 2FA is now a "Set up" button, not a toggle; mobile number has a country-code selector |

Login itself changed: it is now a **two-step flow** (enter username → Next → enter password → Sign in), not a single combined form as the old guide's screenshot shows.

---

## 2. File-by-file disposition

| File | Action | Notes |
|---|---|---|
| `index.md` | **Rewrite** | Login flow (2-step), dashboard cards/panels all changed (Quick Actions row is new: Issue Certificate / Review CSRs / Manage Operators / View Alerts). Fix "Certificate Life Management" vs "Certificate Lifecycle Management" inconsistency — confirm official name from source (§3) and use consistently. Drop placeholder URL `https://clm.companyname.com`. |
| `install.md` | **Rewrite from source** | Confirmed rough: contains a leftover "vScrawl" product-name artifact, literal internal test URLs (`test.*.dictalabs.com`), a stale package filename, and a malformed code block. Replace with deployment facts pulled from `docker-compose.yml`, `README.md`, `env-dev` in both repos (§3). Keep it a runbook (no screenshots needed, consistent with old file), but verified and de-contaminated. |
| `tenants.md` | **Rewrite → `organizations.md`** | Rename file to match new nav label. Drop Domain/Plan/API-Access fields (don't exist in current Create Organization form — now just Name + Description). Update summary cards and list columns from live screenshots. |
| `roles.md` | **Rewrite** | Replace vague "e.g. Certificates, Tenants..." permission list with the confirmed real module list (Dashboard, Certificates, Certificate Requests, Operators, Organizations, Key Management, Crypto Sources, Connectors, Protocols, Compliance Profiles, CSC Configuration, Reports, Notifications, Log, Discovery, Settings, Scheduler) and note roles are organization-scoped (Organization is a required field). |
| `users.md` | **Rewrite → `operators.md`** | Rename to match nav. Document the three auth methods now offered (Password / SSO / Mutual TLS), phone number with country selector, force-password-change-at-next-login checkbox, and the Edit/Resend email/Delete row actions. |
| `connectors.md` | **Rewrite** | Type list grows from 3 to 9 (SMTP Server, Crypto Engine, EJBCA, Microsoft CA, Dictalabs CA, Microsoft Intune, Active Directory/LDAP, SYSLOG, Single Sign-On). Document the common base fields (now include Compliance Profiles + Require Approval) plus 2–3 representative type configs (SMTP, EJBCA, Microsoft CA) with real field screenshots already captured; note the rest follow the same "select type → type-specific Configuration section appears" pattern rather than exhaustively documenting all 9. Fix old caption bugs (duplicate "Connector Details Page" captions, mislabeled "Connector Logs" image). |
| `crypto_sources.md` | **Rewrite** | Store types confirmed unchanged (PKCS11 / AWS KMS / Azure Key Vault) — old guide was accurate here, just needs fresh screenshots and consistent captions (old file reused "Create Crypto Source Page" three times). Verify/clarify the save-button-says-"Create Connector" question directly against the live form text. |
| `key_management.md` | **Rewrite** | Add Post-Quantum key support (ML-DSA algorithm family confirmed live, "Post-Quantum Keys" is now its own dashboard stat alongside "Classical Keys") — this is the single biggest functional gap in the old file. Update dropdowns/fields from the captured Generate Key form. |
| `certificate_requests.md` | **Rewrite** | Add the "Requires Approval" toggle and "Import CSR" flow (separate from Generate Request) which the old file didn't cover as a distinct action. |
| `certificate_management.md` | **Rewrite → `certificates.md`** | Rename to match nav label "Certificates". Fix the broken Obsidian-wiki-link image embed (`![[...]]`) — confirmed this does not use standard MkDocs syntax and needs to become a normal `![]()` reference to a real Certificates screenshot. Update dashboard cards, Import/Issue flows, row actions (confirmed: kebab menu only has **View** and **Delete** — Renew/Revoke must be reached from the certificate's View/detail page, not the list row; verify exact detail-page buttons during screenshot pass). |
| `compliance.md` | **Rewrite → `compliance_profiles.md`** | Rename to match nav label. Old 3-tab structure (Profiles/Rules/Analytics) and Create Rule / Create Profile two-button pattern is confirmed still accurate — good news, this file ages best. Update field list: Create Rule now shows Execution Phase (Pre-Request/Post-Issuance), Severity (Warning/Blocking), Condition — verify whether "Action on Failure" and the old Parameters sub-fields still exist as separate fields or were folded into Condition during the screenshot pass. Fix the placeholder-caption problem (every image was captioned "Compliance Dashboard" or "Compliance Policies List" regardless of actual content). |
| `discovery.md` | **Rewrite into `discovery.md` (or split per type)** | Biggest expansion in the whole guide: old file only covered a single generic "SSL Discovery" form. Live app has 8 distinct discovery pages, each a dedicated task-creation screen with its own fields and an on-page Help panel: SSL, File Scan (local filesystem, format checklist, keystore passwords), SSH, AD CS, Proxy, AWS ACM (region/access key/secret/assume-role), Azure Key Vault, GCP Certificate Manager. All share the Auto Discovery → Scheduler integration (confirmed: Discovery auto-discovery tasks are what populate the Scheduler page). Recommend one page with a subsection per discovery type (keeps parity with the single sidebar group), rather than 8 separate files, to match the guide's existing one-file-per-nav-group convention elsewhere (e.g. Protocols). |
| `scep_server.md` | **Rewrite** | New fields confirmed: Publish to AD (toggle), Challenge Validation (None/Static password) replacing a bare password field, Require Manual Approval. |
| `acme_server.md` | **Rewrite** | Confirmed the richest protocol form: Basic Settings, Rate Limits (per-domain/account/IP/week), Supported Challenges, Key Types & Security (approval mode, Terms Acceptance, External Account Binding). Also document the 4 tabs on the ACME Server page itself (Configurations / Orders / Accounts / External Account Keys) — only "Configurations" (profile list) was covered in the old file. |
| `est_server.md` | **Rewrite** | Old file was suspiciously thin (4 fields) — confirmed the live form actually has Publish to AD, Require Manual Approval, Client authentication mode, and a Client certificate trust anchor (PEM) upload for mutual-TLS EST. The old file's thinness was a real gap, not a doc error. |
| `cmp_server.md` | **Rewrite** | "Java Base URL" old label confirmed now just "Base URL" in the live form — the old label was leftover implementation jargon as the cataloguing pass suspected. Add Auto Push Config and Require Manual Approval fields. |
| — | **New file: `mdm.md`** | Document Enrollment Profiles (Protocol, Certificate Type, SCEP Profile / CA Connector / Intune Connector linkage, Auto-Renew, and the 3 Response Policy revocation toggles with their real UI help text) and Device Certificates (Intune-synced device list, compliance/lifecycle filters, Sync Now/Export). |
| — | **New file: `active_directory.md`** | Document the AD/LDAP certificate-publishing view (Directory Identity, UPN, Account, Type, Status, Published columns) and Run Directory Sync action. Keep short — it's a narrow, single-purpose page. |
| `integrations.md` | **Split into `csc_configuration.md` + fold API Keys into `api_keys.md`** | CSC Configuration and API Keys are now two separate sidebar items, not one "API Integration" group — split accordingly. CSC form fields changed (Name, Region, Logo **URL** not upload, Language, Authentication Type, Description). API Keys: the Permissions field is now a large fixed checklist of ~60 scoped permission strings (`view:certificates`, `create:csr`, `manage:roles`, etc., captured in full) rather than a vague "checkbox list" — document the categories, not all 60 individually. Verify the CSC signing endpoints (`/csc/v2/...`) are still current against backend `api/` routes before reusing that content as-is. |
| `reports.md` | **Rewrite** | Restructured from 2 categories to 3: Certificate Reports (Certificate Report, Certificate Expiry Report), CA Reports (CA/Connector Usage, EJBCA Connector Usage), and new **Common Reports** (Key Strength, Renewal Outcomes). Each is a launcher card with "Open report" → a dedicated filtered report view with Export. Fix the `reports_dashboard_2.png` numbering gap and generic captions from the old file. |
| `notifications.md` | **Delete, merge into `alerts.md`** | Confirmed duplicate/superseded by the "Alerts" feature; the live Create Alert form matches the orphaned `alert.md`'s field set almost exactly (Name, Description, Provider, Alert Type, Days, Daily Send Time, Notification Emails, Status), not `notifications.md`'s. |
| `alert.md` (orphan, not in nav) | **Promote → `alerts.md`, add to nav** | This file was more accurate than the nav-linked `notifications.md` but was never wired into `mkdocs.yml` and its 2 referenced images don't exist on disk. Rewrite with fresh screenshots and add to nav in the "Alerts" position. |
| `logs.md` | **Rewrite → `audit.md`** | Add the third log type, **API Key Logs** (Method, Endpoint, Status, Response Time, IP, User Agent, Date/Time), not covered at all previously. Confirm/replace the Operation Logs column list (now Sr#, Action, Target, Date/Time, IP Address, Operator Agent, Status, View — a "View" opens a Log Details modal with the raw request JSON, which is new/undocumented). Fix typos ("Veiw", "Ip Address"). |
| `settings.md` | **Rewrite** | General now has Application URL + Support Email in addition to Application Name/Company Name/Logo. Branding is confirmed to be 8 colour pickers across 6 sections (Header, Menu, Main, Dialogs, Buttons, Text), not the old file's flat 6-field list. Scheduler: confirmed "Add Scheduler" (not "Create Scheduler") is the button text; note that most schedulers in a live system are auto-created by Discovery's Auto Discovery toggle rather than hand-built, and add a "Sync to Beat" action that wasn't documented before. |
| `user_profile.md` | **Rewrite** | Drop Timezone (field no longer exists). Update: Operator (username, read-only), Email (read-only), First Name*, Last Name, Mobile Number (now has a country-code selector), Department, Job Title, Select Language*. 2FA is a "Set up" button + status badge, not a toggle. Add at least one screenshot — old file had none. |

---

## 3. Source-code verification needed

The following are being cross-checked against `D:\clm\clm-server` and `D:\clm\clm-web-app` (backend models, `role_permissions_enum.py`, deployment files, auth modules) in parallel with this plan, and will be folded into the relevant pages before final write-up:

- Exact permission string list per module (to make `roles.md` and the API Keys permission section precise rather than illustrative).
- Confirmed product name to use consistently ("CLM" vs "Certinium" vs both) — the live app itself shows "CERTINIUM STAGING" / "Certificate Lifecycle Management" in the header, which conflicts with the old guide's exclusive use of "CLM".
- Current, accurate deployment/installation facts (services, ports, required environment variables, licensing mechanism) to replace `install.md`'s contaminated content — without inventing anything not actually in the source.
- Whether an Operator can hold more than one Role (the live form shows a single Role dropdown) and whether Roles/Organizations have a seeded default on install.
- How 2FA actually behaves at login for a user who has it enabled (not verified live, since the admin test account has 2FA "Not enabled").

No source files are being modified — this is read-only verification support for the docs.

---

## 4. Screenshots

**Backup:** the entire current `docs/images/` folder will be copied to `docs/images_backup_before_update/` before any image is added, replaced, or removed. Nothing will be deleted from the backup.

**New screenshots required** (already captured via the scripted browser session and staged in the working scratchpad, pending final selection/cropping/renaming into `docs/images/`):

- Login (2-step: username screen, password screen)
- Dashboard (full, showing Quick Actions row)
- Certificates: list/overview, Issue Certificate form, Import Certificate, row actions, detail/view
- Certificate Requests: overview, Generate Request form, Import CSR form
- Approvals: overview, status cards
- Discovery ×8: SSL, File Scan, SSH, AD CS, Proxy, AWS, Azure, GCP (each a full task-creation screen with Help panel)
- Key Management: overview (showing Post-Quantum stat), Generate Key form
- Crypto Sources: overview, Add form (base + PKCS11/AWS/Azure variants)
- API Keys: overview, Generate API Key form
- Connectors: overview, Add form (base + SMTP + EJBCA + Microsoft CA variants)
- Protocols ×4: SCEP/ACME/EST/CMP overview + Add Profile forms; ACME's 4 tabs
- MDM: Enrollment Profiles overview + Add Profile form; Device Certificates overview
- Active Directory: overview
- CSC Configuration: full form
- Compliance Profiles: overview, all 3 tabs, Create Rule form, Create Profile form
- Alerts: overview, Add Alert form
- Reports ×3 categories: launcher pages + one opened report example
- Audit ×3: Operation Logs (+ Log Details modal), Discovery Logs, API Key Logs
- Organizations, Operators, Roles: overview + respective create forms
- Settings: General, Branding, Scheduler (+ Add Scheduler form)
- Profile page

**Screenshots that can be reused/carried forward:** none as-is — even structurally-unchanged pages (Compliance Profiles, Crypto Sources) have visibly different card layouts, colors, and data in the current build, so every screenshot is being retaken for visual consistency across the guide.

---

## 5. Proposed final nav ordering

Broadly mirrors the current live sidebar order, since that now reflects the product's own information architecture better than the old guide's grouping:

1. Home
2. Install CLM
3. Login & Dashboard *(folds into Home, or stays separate — keeping as-is under Home)*
4. Certificates
5. Certificate Requests
6. Approvals
7. Discovery
8. Key Management
9. Crypto Sources
10. API Keys
11. Connectors
12. Protocols (SCEP / ACME / EST / CMP)
13. MDM
14. Active Directory
15. CSC Configuration
16. Compliance Profiles
17. Alerts
18. Reports
19. Audit
20. Organizations
21. Roles
22. Operators
23. Settings
24. Profile

This keeps the RBAC pages (Organizations/Roles/Operators) together at the end, matching the live sidebar's own placement, rather than forcing them back into an "RBAC" nav group the app no longer visually groups them under.

---

## 6. Open items / things flagged rather than guessed

- Whether "Certificate Life Management" or "Certificate Lifecycle Management" (or just "Certinium") is the name to standardize on — resolving via source-code README check, will default to matching what the app UI itself displays if source is ambiguous.
- Exact Renew/Revoke button locations and labels on the Certificate detail/view page — confirmed they are **not** on the list-row kebab menu (only View/Delete are), need one more targeted screenshot pass on an actual certificate's detail page.
- Whether Compliance Rule's "Action on Failure" and Parameters fields (present in the old guide) still exist as-is or were consolidated into the single "Condition" field seen in the Create Rule form — needs a closer look at the Rule Logic subsection.
- CSC signing REST endpoint documentation (`/csc/v2/...`) in `integrations.md` was not independently re-verified against the live API in this pass (no API client used, browser-only) — flagged for a backend route check rather than carried forward unverified.
- Deployment/install specifics (ports, env vars, licensing) are sourced from the backend/frontend repos rather than a live production deployment, since this task only provides one staging URL — the rewritten `install.md` will be clearly scoped as based on the current source configuration.
