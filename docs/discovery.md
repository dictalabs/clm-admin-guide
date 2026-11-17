# Discovery

**Discovery** finds certificates that already exist in your environment — on network hosts, filesystems, SSH servers, Active Directory, proxies, and cloud key stores — so they can be inventoried, tracked for expiry, and brought under CLM's management.

Each discovery type has its own dedicated page under the **Discovery** sidebar group:

- [SSL Discovery](discovery_ssl.md) — certificates presented on a host's listening ports.
- [File Scan (File-based Discovery)](discovery_filescan.md) — certificate files on the CLM server's local filesystem.
- [SSH-based Discovery](discovery_ssh.md) — certificates on Unix/Linux hosts, read over an SSH session.
- [Active Directory / AD CS Discovery](discovery_adcs.md) — user, computer, service, and CA-issued certificates from AD over LDAP.
- [Proxy Server Discovery](discovery_proxy.md) — certificates seen via a corporate proxy.
- [AWS ACM Discovery](discovery_aws.md) — certificates in AWS Certificate Manager.
- [Azure Key Vault Discovery](discovery_azure.md) — certificates in one or more Azure Key Vaults.
- [GCP Certificate Manager Discovery](discovery_gcp.md) — certificates in Google Cloud Certificate Manager.

Every page is a self-contained task form with an on-screen **Help** panel, a **Discover** button to run the scan immediately, and an **Auto Discovery** toggle to run it on a recurring schedule instead (scheduled discovery tasks then appear under [Settings → Scheduler](settings.md)). Certificates found by any discovery type appear in [Certificates](certificates.md) tagged with protocol `DISCOVERY IMPORT`, and every run is recorded in [Audit → Discovery Logs](audit.md).

!!! note
    All credential fields across every discovery type are encrypted at rest and are never returned in API responses once saved.
