# WSO2 APIM 4.x — Basic Functionality Test Suite

![Static Badge](https://img.shields.io/badge/Tested-Pass-DarkGreen)
![APIM 4.4.0](https://img.shields.io/badge/WSO2_APIM-4.4.0-orange)
![APIM 4.5.0](https://img.shields.io/badge/WSO2_APIM-4.5.0-orange)
![APIM 4.6.0](https://img.shields.io/badge/WSO2_APIM-4.6.0-orange)

## Description

This Postman collection provides an automated **Basic Functionality Test Suite** for WSO2 API Manager 4.x. It validates the core API management workflows end-to-end across the Publisher, Developer Portal, Gateway, and Admin portals using the REST APIs.

> **Scope:** This suite validates the underlying REST APIs consumed by the WSO2 APIM portals (Publisher, Developer Portal, and Admin Portal). UI behaviour and business use case workflows are out of scope and should be validated separately.

The suite can be executed against:

- **Carbon Super tenant** — using the Carbon Super admin credentials
- **Any custom tenant** — by setting the `tenant` variable to the tenant domain and providing the corresponding tenant admin credentials

All requests are designed to run sequentially in folder order (`00` → `12`) using Newman or the Postman Collection Runner.

---

## Covered Test Cases

| # | Folder | Test Cases |
|---|--------|------------|
| 01 | **Publisher — View APIs** | Paginated listing of all Publisher APIs, Individual GET by API ID for every API |
| 02 | **DevPortal — View APIs** | Paginated listing of all DevPortal APIs, Individual GET by API ID for every API |
| 03 | **Publisher — Edit & Republish Existing API** | Fetch existing API by ID, Upadte existing API, Create revision, Deploy revision to gateway _(optional — skipped if `editable_api_id` is empty)_ |
| 04 | **Publisher — Create & Publish New API** | Create new API with unique timestamped name, Create initial revision, Deploy to gateway, Publish API |
| 05 | **DevPortal — Verify New API** | Search DevPortal for the newly published API and confirm visibility |
| 06 | **DevPortal — Applications & Keys** | Paginated listing of all existing applications, Fetch OAuth keys per application, Generate tokens for all existing app key types _(optional — controlled by `enable_existing_application_token_generation`)_, Create new test application, Generate PRODUCTION OAuth keys, Generate access token |
| 07 | **DevPortal — Subscriptions** | List all subscriptions for every DevPortal API with pagination, Subscribe new application to existing API _(skipped if `editable_api_id` is empty)_, Subscribe new application to new API |
| 08 | **Gateway — Invoke APIs** | Invoke existing API through gateway using new app token, Invoke new API through gateway using new app token |
| 09 | **Admin — Throttling Policies** | Create Advanced throttling policy; verify retrieval by ID, Create Subscription throttling policy, Verify retrieval by ID, Apply Advanced policy to new API, Apply Subscription policy to the new API's subscription |
| 10 | **DevPortal — Edit Application & Cleanup** | Update new application description, Remove subscription to existing API _(skipped if `editable_api_id` is empty)_, Remove subscription to new API, Delete new application |
| 11 | **Publisher — Delete New API** | Delete the newly created test API |
| 12 | **Admin — Delete Throttling Policies** | Delete Advanced throttling policy, Delete Subscription throttling policy |

---

## Prerequisites

### Node.js

**Newman requires Node.js. Install it via the NodeSource repository:**

```bash
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt-get install -y nodejs
```

**Verify:**

```bash
node --version
npm --version
```

> For non-Debian/Ubuntu distributions (RHEL, CentOS, Fedora):
```bash
curl -fsSL https://rpm.nodesource.com/setup_lts.x | sudo bash -
sudo yum install -y nodejs
```

### Newman

```bash
npm install -g newman
```

**Verify:**

```bash
newman --version
```

---

## Collection Tree Structure

```
WSO2 APIM 4.x - Basic Functionality Test Suite
│
├── 📁 00_Auth - DCR & Tokens
│   ├── 📄 Dynamic Client Registration
│   └── 📄 Get Access Token
│
├── 📁 01_Publisher - View APIs
│   ├── 📄 List All APIs in Publisher
│   └── 📄 Get API by ID in Publisher
│
├── 📁 02_DevPortal - View APIs
│   ├── 📄 List All APIs in DevPortal
│   └── 📄 Get API by ID in DevPortal
│
├── 📁 03_Publisher - Edit & Republish Existing API ← optional
│   ├── 📄 Get Existing API (for edit)
│   ├── 📄 Update Existing API
│   ├── 📄 Create Revision for Existing API
│   └── 📄 Deploy Revision for Existing API
│
├── 📁 04_Publisher - Create & Publish New API
│   ├── 📄 Create New API
│   ├── 📄 Create Revision for New API
│   ├── 📄 Deploy Revision for New API
│   └── 📄 Publish New API
│
├── 📁 05_DevPortal - Verify New API
│   └── 📄 Search New API in DevPortal
│
├── 📁 06_DevPortal - Applications & Keys
│   ├── 📄 List All Existing Applications
│   ├── 📄 Get Application OAuth Keys by ID
│   ├── 📄 Generate Token for Existing App ← optional
│   ├── 📄 Create New Application
│   ├── 📄 Generate Keys for New App
│   └── 📄 Generate Token for New App
│
├── 📁 07_DevPortal - Subscriptions
│   ├── 📄 List Subscriptions for Existing API
│   ├── 📄 Subscribe New App to Existing API
│   └── 📄 Subscribe New App to New API
│
├── 📁 08_Gateway - Invoke APIs
│   ├── 📄 Invoke Existing API
│   └── 📄 Invoke New API
│
├── 📁 09_Admin - Throttling Policies
│   ├── 📄 Create Advanced Throttling Policy
│   ├── 📄 Get Advanced Throttling Policy
│   ├── 📄 Create Subscription Throttling Policy
│   ├── 📄 Get Subscription Throttling Policy
│   ├── 📄 Get New API (for throttle policy edit)
│   ├── 📄 Test Advanced Throttle Policy - Apply to API
│   └── 📄 Test Subscription Throttle Policy - Apply to Subscription
│
├── 📁 10_DevPortal - Edit Application & Cleanup Subscriptions
│   ├── 📄 Update New Application
│   ├── 📄 Remove Subscription (New App - Existing API)
│   ├── 📄 Remove Subscription (New App - New API)
│   └── 📄 Remove New Application
│
├── 📁 11_Publisher - Delete New API
│   └── 📄 Delete New API
│
└── 📁 12_Admin - Delete Throttling Policies
    ├── 📄 Cleanup - Delete Advanced Throttling Policy
    └── 📄 Cleanup - Delete Subscription Throttling Policy
```

---

## Environment Variables

Open the file `WSO2_APIM_4_x_-_Basic_Functinality_Test_Suite.postman_environment.json` and fill in the values below before running.

> **Note:** Variables marked as *set by tester* must be configured before execution. Variables with a default value are pre-filled and can be left as-is unless you need to override them.

| Variable | Description | Set By | Default |
|----------|-------------|--------|---------|
| `callback_url` | Callback URL value for the Service Provider used in DCR | Tester | _(empty)_ |
| `application_name` | Name of the DCR application to register | Tester | _(empty)_ |
| `control_plane_hostname` | Full base URL of the WSO2 control plane (e.g. `https://localhost:9443`) | Tester | _(empty)_ |
| `keymanager-hostname` | Full base URL of the Key Manager (e.g. `https://localhost:9443`) | Tester | _(empty)_ |
| `gateway_hostname` | Full base URL of the API Gateway (e.g. `https://localhost:8243`) | Tester | _(empty)_ |
| `gateway_env` | Name of the gateway environment to deploy revisions to (e.g. `Default`) | Tester | _(empty)_ |
| `admin_username` | Username of the admin user | Tester | _(empty)_ |
| `admin_password` | Password of the admin user | Tester | _(empty)_ |
| `basicAuthToken` | Base64-encoded `username:password` — auto-generated by the collection pre-request | Auto | _(empty)_ |
| `client_id` | OAuth2 client ID — auto-populated after DCR | Auto | _(empty)_ |
| `client_secret` | OAuth2 client secret — auto-populated after DCR | Auto | _(empty)_ |
| `base64clientcredentials` | Base64-encoded `client_id:client_secret` — auto-generated before token request | Auto | _(empty)_ |
| `access_token` | Bearer token used for all API requests — auto-populated after token acquisition | Auto | _(empty)_ |
| `tenant` | Tenant domain to run the suite against | Tester | `carbon.super` |
| `editable_api_id` | ID of an existing published API to test edit & republish workflow | Tester | _(empty)_ |
| `editable_api_context` | Context path of the editable API (without leading `/`) | Tester | _(empty)_ |
| `editable_api_version` | Version of the editable API (e.g. `1.0.0`) | Tester | _(empty)_ |
| `editable_api_get_operation` | A valid GET resource path on the editable API (e.g. `menu`) | Tester | _(empty)_ |
| `new_api_name` | Base name prefix for the new API — a timestamp suffix is appended automatically | Tester | `AutoTestAPI` |
| `new_api_context` | Context path for the new API (without leading `/`) | Tester | `autotestapi` |
| `new_api_version` | Version for the new API | Tester | `1.0.0` |
| `new_app_name` | Base name prefix for the new application — a timestamp suffix is appended automatically | Tester | `AutoTestApp` |
| `enable_existing_application_token_generation` | Set to `"true"` to generate tokens for all existing applications and their key types | Tester | `false` |
| `adv_throttle_policy_name` | Name for the Advanced throttling policy created during the test run | Tester | `AutoTestAdvPolicy` |
| `sub_throttle_policy_name` | Name for the Subscription throttling policy created during the test run | Tester | `AutoTestSubPolicy` |

---

## Notes

### Token generation for existing applications

By default, token generation for existing applications (`06_DevPortal - Applications & Keys` → *Generate Token for Existing App*) is **disabled**. This avoids unnecessary token churn on production environments. To enable it, set:

```
enable_existing_application_token_generation = true
```

### Skipping the Edit & Republish workflow

Folder `03_Publisher - Edit & Republish Existing API` is **optional**. If `editable_api_id` is left empty, the collection will automatically skip folder 03 and proceed to folder 04. The related variables `editable_api_context`, `editable_api_version`, and `editable_api_get_operation` are also only required when `editable_api_id` is set.

### Tenant configuration

By default, the suite runs against the **Carbon Super** tenant. Set `admin_username` and `admin_password` to the Carbon Super admin credentials.

To run against a **specific tenant**, change the `tenant` variable to the tenant domain (e.g. `mytenant.com`) and set `admin_username` and `admin_password` to that tenant's admin credentials. The collection handles all tenant-specific URL construction (e.g. `t/<tenant_domain>`) internally — do not add it manually to any context or hostname variable.

### Context and version formatting

Ensure the following variables do **not** contain leading or trailing `/`:

- `editable_api_context` → ✅ `pizzashack` &nbsp; ❌ `/pizzashack` &nbsp; ❌ `pizzashack/`  
- `editable_api_version` → ✅ `1.0.0` &nbsp; ❌ `/1.0.0` &nbsp; ❌ `1.0.0/`
- `new_api_context` → ✅ `autotestapi` &nbsp; ❌ `/autotestapi` &nbsp; ❌ `autotestapi/`
- `new_api_version` → ✅ `1.0.0` &nbsp; ❌ `/1.0.0` &nbsp; ❌ `1.0.0/`

Tenant path prefixes (`t/<tenant_domain>`) are handled automatically by the collection and must **not** be included in the context variables.

---

## Running with Newman

```bash
newman run "WSO2_APIM_4_x_-_Basic_Functinality_Test_Suite_postman_collection.json" \
  --environment "WSO2_APIM_4_x_-_Basic_Functinality_Test_Suite.postman_environment.json" \
  --delay-request 1000 \
  --insecure \
  --reporters cli,json \
  --reporter-json-export results.json \
  --timeout-request 10000
```

| Flag | Description |
|------|-------------|
| `--delay-request 1000` | 1000ms delay between requests — recommended to allow gateway sync |
| `--insecure` | Disables SSL certificate verification — required for self-signed WSO2 certificates |
| `--reporters cli,json` | Outputs results to the terminal and saves a JSON report |
| `--reporter-json-export results.json` | Path where the JSON report is saved |
| `--timeout-request 10000` | Marks a request as failed if no response is received within 10 seconds |
