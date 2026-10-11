# Logiqra 1.2.0 — Website and Server Connection Guide

This is a practical setup guide for people installing **Logiqra 1.2.0** for the first time. It covers every automatic connection method shown in the **Websites** connection wizard, plus offline manual import. You do **not** need a separate Logiqra website or online account.

**Important:** Logiqra connects to **web access logs**, not directly to your site's login page or database. The website address identifies the site; the chosen log source supplies the requests that Logiqra can analyse. You must own the source or have permission to monitor it. No access logs means no request evidence to analyse.

## 1. Choose the right connection method

| Your hosting situation | Select in Logiqra | What you need |
| --- | --- | --- |
| Apache/XAMPP, Nginx, or IIS access logs already accessible on your Windows PC | **Local log file** | The actual readable access-log file |
| VPS/dedicated server or hosting with SSH access | **SFTP / SSH** | Host, port, read-only account, remote log path, verified SSH host fingerprint |
| Hosting with explicit secure FTP access to raw logs | **FTPS / cPanel Raw Access** | FTPS endpoint, account, password, accessible remote log path |
| Shared hosting offering PHP and Cron Jobs but no direct log retrieval | **Cron Micro-Collector** | PHP 8.1+, permission to read the access log, Cron Jobs and HTTPS hosting |
| Website served through Firebase Hosting and logged to Google Cloud | **Google Cloud Logging / Firebase** | Google Cloud project ID, Firebase Hosting hostname, read-only service-account JSON key |
| Request logs in AWS | **Amazon AWS** | CloudWatch log group or S3 log bucket, region, read-only IAM credentials |
| Request logs in Azure | **Microsoft Azure** | Log Analytics workspace + reader identity, or Blob container + read/list SAS |
| HTTP request logs available through Cloudflare Log Explorer | **Cloudflare** | Zone ID, suitable plan and API token with Log Explorer read permissions |
| One archived/downloaded log, or no supported automatic connection | **Manual Import** (separate screen) | Local copy of a supported log file |

Not every hosting package exposes raw request logs. **Ask your host whether access logs are available and by which method before creating credentials.** Having cPanel does not automatically mean FTPS can read the server's raw access logs.

## 2. The four steps shared by automatic connections

1. Open **Logiqra → Websites → Connect Website**.
2. In **Website**, enter a recognisable **Website name**, its **Website address** (for example, `https://example.com`), the **Web server**, and **Access-log format**. Choose **Auto-detect** when offered. The address alone does not grant log access.
3. Under **Connection method**, choose one of the eight methods below, then select **Next**.
4. In **Connection details**, enter the fields for that method. Set **Check for new entries every** to an appropriate interval (the wizard accepts 10–86,400 seconds).
5. At **Test and save**, click **Test Connection**. Read the result. If the read-only test succeeds, click **Save Website**.
6. Start or resume Logiqra's monitoring. Check **Websites** for connection health and **Live Activity** for new requests. A successful connection test can mean the source is reachable even if no recent matching requests exist.

Logiqra's setup test is designed to read the selected log source without modifying it. **Running**, **Paused**, **Retrying**, and **Needs attention** are different connection/monitoring states; investigate problems rather than assuming silence means protection.

## 3. Local log file — Apache, XAMPP, Nginx or IIS

**Best for:** a log already stored or accessible on the Windows computer running Logiqra.

1. Find the web server's *access log* (not an error log). Some examples are `C:\xampp\apache\logs\access.log`, `/var/log/apache2/access.log` and `/var/log/nginx/access.log`. Locations depend on the server configuration. IIS commonly uses W3C access-log files under a `LogFiles` directory.
2. If the web server is on another machine, arrange authorised access to the log file on Windows, or consider **SFTP / SSH** instead. Selecting a Linux-only path in the Windows file picker will not fetch it remotely.
3. Choose the appropriate **Web server** and **Access-log format** in the wizard (for IIS, choose **Microsoft IIS** and its W3C format).
4. Select **Local log file**, click **Browse**, and select the real readable file.
5. Click **Test Connection**, then **Save Website**, and check monitoring.

**If testing fails:** verify the path, file permissions, that the file is a regular accessible log, and that the parser matches the file format. A non-access log or a file with a custom schema might not produce usable requests.

## 4. SFTP / SSH — VPS and servers with SSH access

**Best for:** a remote Linux/Unix web server that permits authorised read-only SFTP access.

1. Obtain the **SSH/SFTP hostname**, **port** (often 22), and a dedicated user with read access to the appropriate web-server access log. Ask the administrator for the **full remote access-log path**.
2. Independently confirm the server's **OpenSSH SHA-256 host-key fingerprint** with your hosting administrator. Do not copy an unverified fingerprint from a connection-warning message.
3. In Logiqra choose **SFTP / SSH**. Fill **SFTP host and port**, **Username**, **Remote access-log path**, and **Confirmed OpenSSH SHA-256 host-key fingerprint**.
4. Select an available **Authentication** mode: **Password**, **SSH agent or standard local key**, or **Specific identity file**. If choosing an identity file, browse to the private key on your PC and supply its passphrase if required.
5. Click **Test Connection**. When verified, **Save Website** and begin monitoring.

**If testing fails:** check network reachability, SSH permissions, key authentication, the remote file path, and the pinned fingerprint. **Do not disable host-key verification to get past an error.** A server administrator may need to grant read permission without giving the account write or root access.

## 5. FTPS / cPanel Raw Access — supported hosting accounts

**Best for:** a provider that explicitly permits **FTPS (FTP over TLS)** access to the raw access-log file.

1. Ask your host whether your account can read **raw access logs through FTPS**. Get its FTPS hostname, port (often 21 for **explicit FTPS**), username, password and the **remote log path**. An example of a server-dependent path is `/etc/apache2/logs/domlogs/ACCOUNT/example.com`; do not assume your provider uses it.
2. Choose **FTPS / cPanel Raw Access** in Logiqra.
3. Enter **Explicit FTPS host and port**, **Username**, **Remote access-log path**, and **Hosting-account password**.
4. Click **Test Connection**, then **Save Website** if it passes.

Logiqra requires TLS certificate/hostname verification and protected FTPS data connections; **ordinary unencrypted FTP is not supported**. If the connection fails, ask the host about FTPS access, TLS certificates, firewall/passive-mode requirements and whether the log path is actually readable to your account.

**Only have cPanel's Raw Access download button?** That is a different workflow. In cPanel open **Metrics → Raw Access** (if your host exposes it), download the domain's log archive and use **Manual Import** in Logiqra. A downloaded archive is **not** an automatic FTPS connection. See [cPanel's Raw Access instructions](https://docs.cpanel.net/cpanel/metrics/raw-access/).

## 6. Cron Micro-Collector — shared hosting with PHP and scheduled jobs

**Best for:** hosting that lets you run PHP and configure **Cron Jobs**, but doesn't provide a suitable SFTP/FTPS log connection.

**Requirements:** PHP 8.1 or newer, an accessible *server-side* access-log path, a private folder **outside `public_html`**, a public HTTPS endpoint, and permission to run Cron Jobs.

1. In Logiqra choose **Cron Micro-Collector**. Fill **Protected HTTPS endpoint** (an HTTPS URL you control), **Collector ID**, **Log source ID**, **Access-log path on the hosting account**, and **Private collector directory outside public_html**.
2. Generate the **Collector secret** using the wizard; do not share it. **Start with new entries only on the first Cron run** is selected by default. Keep it selected unless you intentionally need available history.
3. Click **Generate Deployment ZIP** and save the package privately on your Windows PC.
4. Extract the generated ZIP. **Read its `INSTALL.txt`** for the exact paths and commands configured for your hosting account.
5. Upload the *contents* of the package's **private** directory to the specified private hosting directory, **outside `public_html`**. Upload only the package's `public/endpoint.php` to the web path matching the configured HTTPS endpoint.
6. In the hosting control panel's **Cron Jobs** feature, add the generated PHP command shown in `INSTALL.txt`. The generated instructions use a **once-per-minute** schedule. Run it once if possible.
7. Return to Logiqra, click **Test Connection**, then **Save Website** when the test succeeds.

**Security:** the generated ZIP and `private/config.php` contain a sensitive collector secret. Keep them out of the public website folder and GitHub. The public endpoint must run over HTTPS. If the source path is wrong or PHP cannot read the log, the collector will not deliver useful records even if the public page exists. Never copy the private configuration to `public_html`.

## 7. Google Cloud Logging / Firebase Hosting

**Best for:** a website actually served by **Firebase Hosting** whose request logs are available in **Google Cloud Logging**. This connection is more specific than "any Firebase app": Logiqra 1.2.0 looks for Firebase Hosting **web request** entries.

### A. Prepare Google Cloud (one-time setup)

1. Sign in to the [Google Cloud Console](https://console.cloud.google.com/) and select the **project serving your Firebase Hosting website**. Copy its **Project ID** (not merely the displayed project name).
2. Open **Logging → Logs Explorer** and confirm that actual **Firebase Hosting request logs** are present. Logiqra's current Firebase connector looks for the `firebase_domain` resource and the `firebasehosting.googleapis.com/webrequests` log, with a matching hostname. If these logs are not being generated or retained, setup cannot make them appear.
3. In **APIs & Services**, confirm that **Cloud Logging API** is enabled for the project. [Google's instructions](https://docs.cloud.google.com/logging/docs/api/enable-api).
4. In **IAM & Admin → Service Accounts**, create or select a **dedicated service account** with the least access required. Grant **Logs Viewer** (`roles/logging.viewer`) for the project/log access you need; additional view-specific permissions can depend on where logs are stored. Avoid broad Owner/Editor roles. [Google's access-control guide](https://docs.cloud.google.com/logging/docs/access-control).
5. If your organisation's policy permits service-account JSON keys, generate a **JSON key** for that account and securely store it **on your Windows computer**. The current Logiqra connector uses a local JSON key file, not an interactive Google sign-in. Treat the key as a password, restrict local file access, and revoke/rotate it if exposed.

### B. Connect in Logiqra

1. Open **Websites → Connect Website**. Select **Structured / JSON** as the web server type (the wizard selects this automatically for Google Cloud). Select **Google Cloud Logging / Firebase**.
2. Enter the **Google Cloud project ID**, your exact **Firebase Hosting hostname** (for example, `www.example.com`, without `https://`), and the path to the **Read-only service-account JSON key** using **Browse**.
3. Click **Test Connection**. If the result says the API is reachable but no matching request was found, check Logs Explorer for the exact project, Firebase log type and hostname.
4. Click **Save Website**, then start monitoring and visit your own website normally to generate a permitted test request. Check **Live Activity** after new entries are available.

**No extra Python bridge, gcloud command, or inbound port is required in Logiqra 1.2.0.** The desktop connector calls the official Cloud Logging API directly. The JSON key remains in the selected local file; do not upload it to GitHub.

**Important limitation:** this specific connector is for Firebase Hosting request entries. A website using only Firestore/Auth, or a server hosted elsewhere without compatible Firebase Hosting request logs, should use the relevant log source instead.

## 8. Amazon AWS — CloudWatch Logs or S3

**Best for:** access-request logs that your organisation already collects in **CloudWatch Logs** or an **S3 bucket**.

1. In AWS, identify the correct **region** and either a **CloudWatch log group** or an **S3 bucket containing access-log objects**. Confirm that the log contents actually record HTTP requests in a compatible format.
2. Have an administrator provision a least-privilege **IAM identity**. For CloudWatch, Logiqra reads through `FilterLogEvents` (requiring `logs:FilterLogEvents` on the relevant resource). For S3, grant the permissions needed to **list objects** and **read the log objects** (typically `s3:ListBucket` and `s3:GetObject`). Avoid write permissions.
3. In Logiqra choose **Amazon AWS**, then **AWS log source**: **CloudWatch Logs** or **Amazon S3 access-log bucket**.
4. Fill **AWS region**, **CloudWatch log group or S3 bucket**, **IAM access key ID**, **IAM secret access key**, and, if using temporary credentials, **Session token**.
5. Use **S3 prefix (optional)** to limit object retrieval, or **CloudWatch filter pattern (optional)** to restrict events as appropriate. Click **Test Connection**, then **Save Website**.

A successful API connection does not guarantee the selected log group or bucket contains parsable web requests. For CloudWatch permissions, see the [AWS permissions reference](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/permissions-reference-cwl.html).

## 9. Microsoft Azure — Log Analytics or Blob Storage

**Best for:** website request events already being sent to **Azure Monitor Log Analytics**, or compatible log files saved in **Azure Blob Storage**.

### Option A: Log Analytics

1. Find the **Log Analytics workspace ID** that stores the request data; identify the appropriate **KQL table or base query** (the UI starts with `AzureDiagnostics`, which may not be the correct table for your workspace).
2. Ask the Azure administrator for a least-privilege **Microsoft Entra application/service principal** allowed to query that workspace. Obtain its **Tenant ID**, **Client ID**, and **Client secret** through an approved secure channel.
3. In Logiqra choose **Microsoft Azure → Log Analytics**. Enter **Workspace ID or Blob container URL** (your workspace ID), **Tenant ID**, **Client ID**, **Client secret**, and **Log Analytics base query/table**.
4. Select **Structured / JSON** when applicable; Logiqra uses this profile for Log Analytics. Click **Test Connection**, then **Save Website**.

### Option B: Azure Blob Storage

1. Obtain the **HTTPS container URL** in the form `https://ACCOUNT.blob.core.windows.net/CONTAINER`, and the **optional prefix** used for relevant access-log files.
2. Ask an authorised administrator for a suitably scoped, time-limited **Blob SAS token** granting **read and list** permissions. A broad account-owner credential is unnecessary.
3. In Logiqra choose **Microsoft Azure → Azure Blob Storage**, enter the container URL, optional **Blob prefix**, and **Blob SAS token**. Click **Test Connection**, then **Save Website**.

For workspace access and API details, see [Azure Monitor query documentation](https://learn.microsoft.com/en-us/azure/azure-monitor/logs/api/request-format). For list/read access to blobs, see [Microsoft's Blob API reference](https://learn.microsoft.com/rest/api/storageservices/list-blobs).

## 10. Cloudflare — Log Explorer

**Best for:** a website whose **Cloudflare account/plan provides Log Explorer HTTP request logs** and API access. Not all plans expose this feature.

1. In the Cloudflare dashboard choose the intended website/zone and find its **Zone ID**.
2. Confirm that **Log Explorer** is available for that zone and contains request data. If the feature isn't available on your plan, choose another log source.
3. Create a **limited API token** with the permissions required to query Log Explorer for that zone. Do not use a full account-wide credential when narrower permissions are possible.
4. In Logiqra choose **Cloudflare** and enter **Cloudflare zone ID**, **Hostname filter (optional)**, and **API token**. The app uses the **Structured / JSON** profile for this connector.
5. Click **Test Connection**; if successful, **Save Website** and check Live Activity after traffic is recorded.

Cloudflare's [Log Explorer API guide](https://developers.cloudflare.com/log-explorer/api/) describes feature access, token permissions and query support.

## 11. Manual Import — if you cannot connect automatically

Manual Import is **not** a ninth automatic connector. It imports a selected file for historical/offline analysis.

1. In your hosting control panel, download an authorised access-log file (for example through **cPanel → Metrics → Raw Access**), or obtain a copy from the server administrator. Keep it confidential.
2. In Logiqra open **Manual Import**.
3. Under **Website**, pick an existing site or **Create a manual-only website**, and fill the new website fields if needed.
4. Choose the appropriate **Server type** and **Log format**. Click **Browse** and select a supported log file. The file picker accepts forms including `.log`, `.txt`, `.json`, `.ndjson`, `.jsonl` and `.gz`.
5. Click **Import and Analyse**. Review the result, **Open findings**, and any **unsupported entries** reported.

Manual Import does not keep fetching future traffic. To monitor continuously, configure a supported automatic source.

## 12. Troubleshooting: the common problems

| What you see | What to check |
| --- | --- |
| **Test Connection** fails immediately | Missing/incorrect field, wrong server type or format, invalid local path, or blocked network access |
| **SFTP host key mismatch** | Reconfirm the server's identity with the administrator; never turn off fingerprint checking |
| **FTPS certificate or connection error** | Explicit FTPS vs plain FTP, certificate hostname, port, passive-mode firewall, and log access |
| **Google Cloud permission denied** | Project ID, service-account key, enabled Cloud Logging API, and Logs Viewer access |
| **Google Cloud connects but shows no requests** | Actual Firebase Hosting webrequests logs, correct hostname, timestamp and recent website traffic |
| **Cron Collector cannot connect** | PHP version, `INSTALL.txt` instructions, HTTPS public endpoint, private directory, correct log path, scheduled job and shared secret |
| **AWS/Azure/Cloudflare test is reachable but empty** | Log stream/bucket/workspace/zone really contains suitable request logs, query/prefix/filter, plan and data retention |
| **Files read but no detections appear** | Check parser format, log completeness, request fields, and normal/benign traffic; no alerts does not mean an attack was blocked |
| **Monitoring paused or needs attention** | Resume monitoring and inspect website connection events and health before expecting new records |

### Connection succeeds, but Live Activity is empty?

First generate an **ordinary authorised request** to your own monitored website, wait for the log producer to record it and for the Logiqra polling interval to pass, then check the selected source and time filters in **Live Activity**. Some cloud logging pipelines have delayed delivery. Never attack a third-party site just to test whether Logiqra is connected.

## 13. Security and privacy checklist

- Use **read-only access**, a dedicated identity, and the minimum permissions needed. Do not use root, administrator or owner credentials unless there is genuinely no safer authorised alternative.
- Never commit cloud JSON keys, FTP passwords, SSH private keys, AWS tokens, Azure client secrets, collector ZIPs, raw logs or Logiqra databases to a public repository.
- Verify TLS certificates and SSH fingerprints instead of bypassing warnings.
- Logiqra stores supported passwords, tokens and secrets with **Windows Credential Manager**; Google service-account JSON keys are referenced from your local file and should be secured there.
- Connection access and visibility depend on the hosting provider, plan, log configuration and available fields. Logiqra **analyses evidence**; it does **not** block attacks or fix a vulnerable server.

## Need more help?

When asking for help, mention **Logiqra version**, your selected **connection method**, which wizard step failed, the **exact safe error message**, and whether the log source exists. You can share a **redacted screenshot**, but **never share credentials, private keys, access tokens, or real customer request logs** in a public GitHub issue.

For installation, see the [Installation and Upgrade Guide](LOGIQRA_INSTALLATION_GUIDE.md). For everyday monitoring, see the [User Guide](LOGIQRA_USER_GUIDE.md).

*This guide describes the fields and supported connection methods in Logiqra 1.2.0. Cloud consoles and hosting plans may change; consult the linked provider documentation where their screens differ.*
