# HotWax Commerce Setup Guide

> **Scope:** This guide walks through initial configuration of a HotWax Commerce OMS instance covering three areas: **Shopify Shop**, **Product Store & General Settings**, and **Facility Setup**.

---

## Prerequisites

| Requirement | Detail |
|---|---|
| OMS instance URL | `https://{instance-name}.hotwax.io/` |
| Admin credentials | Available from the HotWax support team |
| Shopify store | HotWax Commerce integration app installed |

> [!IMPORTANT]
> Reset the default password immediately after first login. Create dedicated admin users and disable the default user for security.

---

## Step 1: Initial Login & DBIC Setup

### 1.1 Log In

1. Go to `https://{instance-name}.hotwax.io/commerce/control/main`
2. Enter credentials and log in
3. On first login, reset your password when prompted
4. Confirm that the **Sidebar** and **EXIM** screens load correctly

### 1.2 Add DBIC (Doing Business In Countries)

DBIC defines which countries this OMS instance operates in — affects tax, shipping, and address validation.

**Path:** Settings → DBIC Configuration

- Add only the countries relevant to this instance
- US-only retailers: add `USA`

> [!NOTE]
> When one OMS instance serves multiple countries, include only the relevant countries. Incorrect DBIC setup causes errors in tax and shipping flows.

### 1.3 Update Company Profile

The **Company Profile** holds the identity of the retailer's business within OMS — including company name, logo, primary address, and contact details. Update this immediately after first login so all system communications and documents reflect the correct brand identity.

**Direct URL:**
```
https://{instance}.hotwax.io/commerce/control/ViewParty?partyId=COMPANY
```

**Path in OMS:** Hamburger Menu → Party → COMPANY

#### What to Update

| Section | Field | Action |
|---|---|---|
| **Overview** | **Company Name** | Click the  edit icon next to `Default Company` — replace with the actual retailer name |
| **Overview** | **Company Logo** | Click **Change** under the logo — upload the retailer's brand logo |
| **Overview** | **Status** | Should remain `Enabled`  |
| **Overview** | **Primary Address** | Click **Add Primary Ad...** — enter the retailer's registered business address |
| **Overview** | **Emails and Phones** | Click **+** — add the company's primary email and phone number |
| **Overview** | **Add Role** | Click **+** — assign business roles if required (e.g., `INTERNAL_ORGANIZATIO`) |
| **Overview** | **Add Classification** | Click **+** — classify the company type if needed |
| **Overview** | **Add Identification** | Click **+** — add Tax ID, EIN, or other business identifiers |
| **Summary** | **Company Logo Path** | Click **Edit** — update the logo path (default: `/resources/uploads/images/company_logo.png`) |
| **Accounting Preferences** | Accounting settings | Configure currency and accounting preferences if applicable |

**Live example (dev environment):**

| Field | Value |
|---|---|
| Company Name | `Default Company` *(update to retailer name)* |
| Status | `Enabled` |
| Logo Path | `/resources/uploads/images/company_logo.png` |

> [!IMPORTANT]
> Always update the **Company Name** and **Company Logo** before going live. These values appear on packing slips, system emails, and internal OMS documents sent to store staff and customers.

---

## Step 2: Shopify Shop Setup

Installing the HotWax Commerce Shopify app **automatically creates a Shopify Shop record** in OMS. This section covers configuring and verifying that connection.

### 2.1 Navigate to Shopify Shop

**Path:** Hamburger Menu → Settings → Shopify → Find Shopify Connection

**Direct URL:**
```
https://{instance}.hotwax.io/commerce/control/ViewShopifyShop?shopId=<shopId>
```

**Live example — Shop ID: 10000 (`hc-sandbox`)**

#### Overview

| Field | Value |
|---|---|
| Phone | `41965584548`, `9826754548` |
| Email | `deepak.dixit@hotwax.co` |
| Store Name | `HC Demo Store` |
| Owner | `HotWax Commerce Shopify` |
| Plan | `partner_test` |
| Primary Location ID | `67890151588` |
| Shop Domain | `hc-sandbox.myshopify.com` |
| Weight Unit | `lb` |
| Currency | `USD` |
| Country Code | `US` |
| Time Zone | `America/New_York` |

#### Shopify Config

| Field | Value |
|---|---|
| Name | `10010 hc-sandbox` |
| Access Scope | `Shopify shop read and write access` |
| API Version | `2025-07` |
| Connect URL | `https://hc-sandbox.myshopify.com/` |
| Process Refund | `Y` |
| Client ID | `ec8cec8c4299d0ea17269da567eebc28` |
| Access Token | (hidden — copy via super user action) |
| Client Secret | (hidden — copy via super user action) |
| Last Updated | `07-12-2025 02:35 AM` |

#### Shopify Scripts (auto-loaded)

| Script ID | Type | Version | File Type | From Date | Shopify Tag ID |
|---|---|---|---|---|---|
| `10001` | Pre-order JS | v1.0.0 | application/javascript | 06-11-2024 09:00 AM | `280275648676` |
| `10003` | Pre-order CSS | v1.0.0 | text/css | 06-11-2024 09:00 AM | — |
| `10016` | BOPIS JS | v1.0.0 | application/javascript | 06-12-2024 08:45 AM | `280346198180` |
| `10017` | BOPIS CSS | v1.0.0 | text/css | 06-12-2024 08:45 AM | `280347050148` |

### 2.2 Quick Actions on the Shop Summary Card

| Action | Description |
|---|---|
| **Manage Shopify Access** | Set to `No access`, `Read only`, or `Read and write` |
| **Link Product Store** | Associate this shop with a Product Store (brand) |
| **Upload Refunds to Shopify** | Enable if HotWax returns should trigger Shopify refunds (super user only) |
| **Copy Credentials** | Copy shared secret and access token (super user only) |
| **Deactivate Shop** | Stops all communication with Shopify |
| **Favorite** | Mark as favorite for quick access in Job Manager |

> [!IMPORTANT]
> **Link the Shopify Shop to a Product Store before proceeding.** Without this link, orders and inventory will not sync correctly.

### 2.3 Products Sync

1. Configure **product type mappings** before downloading (e.g., map Shopify `Insurance` → HotWax `Digital`)
2. Ensure the bulk product download job is configured in OMS
3. Trigger the initial product download from the Shop page

> [!TIP]
> Always set up product type mappings **before** the first product download to avoid data discrepancies.

### 2.4 Orders Sync

1. Configure **sales channel mappings** between Shopify and OMS
2. Trigger the bulk order sync
3. OMS downloads only unfulfilled, open orders by default
4. Orders land in `Created` state — they need approval before fulfillment begins

### 2.5 Inventory Sync

Map the **default Shopify location** to the OMS brokering queue location. This is where inventory totals are posted on Shopify.

| Control | Purpose |
|---|---|
| **Instant Sync** | Immediately push current ATP to Shopify |
| **Deactivate** | Pause inventory sync for troubleshooting |

> For Shopify POS stores — deactivate online selling at POS stores to prevent double-counting.

### 2.6 Map Facilities to Shopify Locations

Required for POS inventory sync. Facilities and Shopify locations are mapped one-to-one.

**Steps:**
1. Shopify Shop page → **Facilities** section
2. Click **"Import Shopify locations in HotWax"**
3. Select locations → OMS creates matching facilities
4. Map each facility to its Shopify Location ID

**Live facility-location mappings (hc-sandbox):**

| Facility | Facility Type | Shopify Location ID |
|---|---|---|
| BROADWAY | RETAIL_STORE | `67890446500` |
| BROOKLYN | WAREHOUSE | `67890479268` |
| CENTERVILLE | RETAIL_STORE | `67890282660` |
| DIVRETAILSTORE | RETAIL_STORE | `111` |
| GARDEN_CITY | RETAIL_STORE | `67890544804` |
| INDORE | RETAIL_STORE | `23` |
| MIAMI2 | RETAIL_STORE | `Test20` |
| MIAMI3 | WAREHOUSE | `598456184` |
| OREM | RETAIL_STORE | `67890348196` |
| QUEENS | RETAIL_STORE | `67890577572` |

### 2.7 Data Mappings

Configure all mapping types from the Shopify Shop page. Import from Shopify first, then map each value to its OMS equivalent. Mappings can be edited anytime via the overflow menu.

#### Sales Channel Mappings

Maps Shopify sales channels to OMS internal channel types.

| Shopify Value | OMS Mapped Value |
|---|---|
| `android` | `PHONE_SALES_CHANNEL` |
| `exchange` | `EXCHG_SALES_CHANNEL` |
| `iphone` | `PHONE_SALES_CHANNEL` |
| `Marketplace` | `MKTP_SALES_CHANNEL` |
| `pos` | `POS_SALES_CHANNEL` |
| `shopify_draft_order` | `CSR_SALES_CHANNEL` |
| `web` | `WEB_SALES_CHANNEL` |

> [!IMPORTANT]
> Missing sales channel mappings cause order processing errors — orders from unmapped channels will not be handled correctly.

#### Payment Method Mappings

Maps Shopify payment method names to OMS payment type IDs.

| Shopify Value | OMS Mapped Value |
|---|---|
| `afterpay` | `EXT_SHOP_AFTRPAY` |
| `afterpay_north_america` | `EXT_SHOP_AFTRPAY_NA` |
| `American Express` | `EXT_SHOP_AMEX` |
| `Discover` | `EXT_SHOP_DISCOVER` |
| `Klarna` | `EXT_SHOP_KLARNA` |
| `Mastercard` | `EXT_SHOP_MASTERCARD` |
| `paypal` | `EXT_SHOP_PAYPAL` |

#### Product Type Mappings

Maps Shopify product types to OMS product types. Configure **before** the first product download.

| Shopify Value | OMS Mapped Value |
|---|---|
| `donation` | `DONATION` |
| `Gift Card` | `DIGITAL_GOOD` |
| `Gift Cards` | `DIGITAL_GOOD` |
| `Loyalty Card` | `DIGITAL_GOOD` |

#### Additional Mapping Types

| Mapping Type | Purpose |
|---|---|
| **Order Item Association** | Maps Shopify SKU variants to OMS product IDs |
| **Order Item Group Association** | Groups order items for combined fulfillment |
| **Product Tag** | Maps Shopify product tags to OMS attributes |
| **Order Customer Classification** | Classifies customers based on Shopify order data |

### 2.8 Shipping Method (Carrier Shipment) Mappings

Maps Shopify shipping method names to OMS carrier + shipment method combinations.

**Steps:**
1. Shopify Shop page → **Shopify Shop Carrier Shipment** section
2. Click **"Import Shopify shipping methods in HotWax"**
3. Match each Shopify shipping method to an OMS carrier and shipment method
4. Save — mappings can be changed at any time

**Live carrier shipment mappings (hc-sandbox):**

| Carrier | OMS Shipment Method | Shopify Shipping Method |
|---|---|---|
| FedEx | 2 Day FedEx Shipping | `2 Day FedEx Shipping` |
| Default | Next Day | `Expedited` |
| FedEx | 2 Day FedEx Shipping | `FedEx 2-Day Test` |
| FedEx | BLINK DELIVERY | `SAME_DAY_BLINKIT_SHOPIFY` |
| FedEx | Third Day | `Standard` |

> [!NOTE]
> Required when generating shipping labels for predefined methods (without rate shopping).

---

## Step 3: Product Store & General Settings

The **Product Store** is the central brand configuration in OMS. It governs order handling, brokering, fulfillment, inventory, and Shopify integration.

### 3.1 Navigate to Product Store

**Path:** Hamburger Menu → Settings → Store

**Direct URL:**
```
https://{instance}.hotwax.io/commerce/control/ViewStore?productStoreId=<storeId>
```

**Live example — Product Store ID: `100010` (Gorjana_demo)**

### 3.2 Core Product Store Fields

#### Configure These Fields

| Field | Description | Live Value (Example) |
|---|---|---|
| **Store Name** | Brand name — update before anything else | `Gorjana_demo` |
| **Currency** | Transaction currency from client onboarding form | `USD` |
| **Auto Approve Order** | Auto-approve on import if no pre-approval workflow needed | `Y` |
| **Auto Cancel Days** | Days until unallocated order is auto-cancelled (`0` = disabled) | `0` |
| **Sales Order ID Prefix** | Prefix added to internal order IDs (e.g., `HC-`) | `TEST-` |
| **Allow Split** | Allow orders to split across multiple shipments | `Y` (default) |
| **Product Identifier** | Primary product ID used internally in OMS | `SKU` / `UPCA` |

#### Do Not Change These Fields

| Field | Required Value | Why |
|---|---|---|
| **Enable Brokering** | `Y` | Core OMS routing capability |
| **Reserve Inventory** | `Y` | OMS must be inventory system of record |
| **Explode Order Items** | `Y` | Required for single-quantity item flows |

### 3.3 Product Store Settings (Advanced)

These are additional key-value settings on the Product Store. Live values from `100010`:

| Setting ID | Name | Value | Purpose |
|---|---|---|---|
| `AFFECT_QOH_ON_REJ` | Affect QOH on Rejection | `N` | Don't adjust quantity on hand when rejecting orders |
| `BARCODE_IDEN_PREF` | Barcode Identifier Preference | `SKU` | SKU used as primary barcode |
| `BOPIS_PART_ODR_REJ` | BOPIS Partial Order Rejection | `true` | Allow partial rejection of BOPIS orders |
| `BRK_SHPMNT_THRESHOLD` | Brokering Shipment Threshold | `10` | Orders under $10 are not split |
| `CUST_ALLOW_CNCL` | Allow Customer Cancel | `true` | Customers can cancel before fulfillment |
| `CUST_DLVRADR_UPDATE` | Allow Delivery Address Change | `true` | Customers can update delivery address |
| `CUST_DLVRMTHD_UPDATE` | Allow Delivery Method Change | `false` | Delivery method cannot be changed |
| `CUST_PCKUP_UPDATE` | Allow Pickup Location Change | `true` | Customers can change pickup location |
| `FF_COLLATERAL_REJ` | Collateral Rejection | `N` | Reject only the specific item, not collateral items |
| `FF_DOWNLOAD_PICKLIST` | Download Picklist as CSV | `false` | Opens picklist as PDF |

**Other available settings** (add as needed):

| Setting ID | Purpose |
|---|---|
| `AUTO_REJ_IDLE_ORD` | Auto-reject idle orders after N minutes |
| `RATE_SHOPPING` | Enable carrier rate comparison |
| `FULFILL_FORCE_SCAN` | Require scanning every item during packing |
| `REJ_ITM_CC_CRT` | Create cycle count for items rejected with variance |
| `DISABLE_SHIPNOW` | Hide "Ship Now" button in Fulfillment App |
| `RTN_RSTCK_FAC` | Default facility for restocking returns |
| `DEFAULT_CARRIER` | Default shipping carrier for new orders |

### 3.4 General Settings

**Path:** Hamburger Menu → Settings → General Settings

**Direct URL:**
```
https://{instance}.hotwax.io/commerce/control/GeneralSettings
```

**Live values from dev environment:**

| Property | Value |
|---|---|
| `country.geo.id.default` | `USA` |
| `currency.uom.id.default` | `USD` |
| `mail.debug.on` | `Y` |
| `mail.notifications.enabled` | `Y` |
| `date.format.default` | `MM-dd-yyyy hh:mm a` |
| `default.commerce.date.format` | `MM/dd/yyyy` |
| `baseUrl` | `https://dev-oms.hotwax.io` |
| `image.management.url` | `/images/products/management` |

**Shopify Webhook URLs (auto-configured per shop):**

| Webhook | URL Pattern |
|---|---|
| Inventory Levels Update | `${baseUrl}/shopify/inventoryLevelUpdateFromShopify` |
| Orders Cancelled | `${baseUrl}/shopify/cancelOrderShopifyWebhook` |

> [!NOTE]
> For **non-US retailers**, update `country.geo.id.default` and `currency.uom.id.default` and configure System Property Data to match the retailer's region.

### 3.5 System Property Data (Non-US Retailers)

Navigate to:
```
https://{instance}.hotwax.io/commerce/control/ImportData?configId=SETUP_SYSTEM_PROPERTY
```

Update:
- **Currency** (e.g., `GBP`, `EUR`, `AUD`)
- **Country** (e.g., `GBR`, `AUS`)
- **Shipment Weight Units** (e.g., `kg` instead of `lbs`)

### 3.6 Shipping Methods on Product Store

**Live shipping methods (Product Store 100010):**

| Method | Carrier | Notes |
|---|---|---|
| Ship To Store | FedEx | — |
| Two Day | FedEx | — |
| Standard | DHL | — |
| Two Day | DHL | — |
| BLINK DELIVERY | DHL | — |
| Same Day | ABC Trans | — |
| Next Day | DTDC | — |
| Third Day | DTDC | — |

**To add a shipping method:**
1. Product Store → **Shipping Method** tab → **Add Shipping Method**
2. Select: **Carrier** → **Shipment Method** → **Gateway Config**
3. Save

### 3.7 Associate Facilities with Product Store

**Facilities associated with Product Store 100010 (live):**
`BOPIS_ORDER`, `FLORIDA`, `100612`, `100663`, `100714`, `100765`, `MIAMI`, `123EDFVB`, `HUNTINGTON_STATION`, `AUSTIN,TX`

To add a facility:
1. Product Store → **Facility** tab → **Add Facility**
2. Select facility → Save

---

## Step 4: Facility Setup

Facilities are physical locations (warehouses, stores) where inventory is stored and orders are fulfilled.

### 4.1 Two Ways to Create Facilities

#### Option A: Bulk Import via CSV *(Recommended for initial setup)*

1. Download template:
   ```
   https://{instance}.hotwax.io/commerce/control/ImportData?configId=IMP_FACILITY
   ```
2. Click **"Download Sample CSV template"**
3. Fill in columns:

| Column | Description | Example |
|---|---|---|
| **Facility ID** | Unique alphanumeric ID — **use same as Shopify store name** | `NYC-STORE-01` |
| **External Facility ID** | Same as Facility ID | `NYC-STORE-01` |
| **Facility Name** | Match name exactly as in Shopify | `New York City Store` |
| **Facility Type ID** | `RETAIL_STORE` or `WAREHOUSE` | `RETAIL_STORE` |
| **Address** | Full address | `123 Broadway, New York, NY` |
| **Phone Number** | Required for shipping carrier integrations | `+1 212 555 0100` |
| **Facility Group** | Online sales inclusion flag | `FAC_GRP` |
| **Product Store** | Product Store ID (`STORE` for single brand) | `STORE` |

4. Import the filled CSV back at the same URL

#### Option B: Create via Facilities App *(For individual facilities)*

1. **Launchpad → Facilities App**

> [!NOTE]
> Only users in the **Administration** security group can log into the Facilities app.

2. Facilities homepage → click **+** (bottom right)
3. Select type: **Warehouse** or **Store**
4. Enter:
   - **Name** — must match Shopify location name
   - **Internal ID** — auto-generated (or manually enter)
   - **External ID** — use same as Internal ID
5. Click **Create Facility**
6. Add **address + geolocation** on next screen
7. Set **configurations** (product store, sell online, allow pickup, native fulfillment app)
8. Click **Save Configurations**

### 4.2 Configure Facility Details

After creating a facility, configure the following from the **Facility Details** page:

#### Address & Contact

| Field | Note |
|---|---|
| Address Line, City, State, Country, Zip | **Zip code is critical** — used for zone-based order routing |
| Phone Number | Required for shipping carrier label generation |
| Shipping Name | Shown on shipment labels (can differ from facility name) |
| Directions | Landmarks to help staff/customers locate the facility |

> [!WARNING]
> A missing or incorrect **Zip Code** will cause brokering failures — orders won't route to this facility.

#### Latitude & Longitude

Required for:
- **Distance-based brokering** — nearest store to customer
- **BOPIS store lookup** on Shopify PDP

Steps:
1. Facility Details → **Latitude & Longitude** card → **Add**
2. Enter manually **or** click **Generate** to derive from address
3. Save

#### Operating Hours & Time Zone

1. **Operating Hours** card → select existing calendar or **Custom Schedule**
2. Set start/end time; enable **Daily Timings** for day-by-day hours
3. Set **Time Zone** via **Change** button

### 4.3 Online Fulfillment Settings

| Setting | Description |
|---|---|
| **Allow Pickup** | Enable for BOPIS — facility appears as pickup option on Shopify PDP |
| **Use Native Fulfillment App** | Enable for HotWax Fulfillment App; disable if using WMS |
| **Generate Shipping Labels** | Enable for HotWax carrier-integrated label generation |
| **Days to Ship** | Minimum days after brokering before order must ship |

**Fulfillment Capacity:**
1. **Online Order Fulfillment** card → overflow menu
2. Choose: `Unlimited`, `No capacity`, or `Custom` (max orders/day)

### 4.4 Associate Facility with Product Store

1. Facility Details → **Product Stores** card → **Add**
2. Select Product Store(s) → Save
3. Overflow menu → **Mark as Primary** for the main brand

### 4.5 Facility Groups

Facility Groups define what role a facility plays in OMS omnichannel operations.

**Path:** Facilities App → **Groups Tab**

#### System Group Types

| Group Type | Purpose |
|---|---|
| **PICKUP** | Facilities available for BOPIS. Appear on Shopify PDP as pickup options. |
| **BROKERING** | Facilities eligible to receive brokered orders. Required for routing rules. |
| **CHANNEL FAC GROUP** | Facilities contributing inventory to online ATP for a sales channel. |
| **Generate Shipping Label** | Facilities that generate labels via HotWax. |
| **Same Day Shipping** | Facilities capable of same-day fulfillment. |
| **OMS Fulfillment** | Facilities visible in the HotWax Fulfillment App. |

#### Create a Facility Group

1. Groups Tab → **+** button
2. Fill in:

| Field | Notes |
|---|---|
| **Name** | Descriptive name (e.g., `East Coast Brokering`) |
| **Internal ID** | Auto-generated — **cannot be changed after creation** |
| **Group Type** | Select from system types above |
| **Product Store** | Required for brokering groups (links to routing rules) |
| **Description** | Optional but recommended |

3. Save

#### Add Facilities to a Group

1. On Group card → click the **facility count number**
2. On **Manage Facilities** page:
   - **+** to add individual facilities
   - **INCLUDE ALL** to add all at once
   - **Drag and drop** to set routing priority sequence
   - **−** to remove a facility
3. Save

> [!TIP]
> In **Brokering** groups, sequence matters — use drag-and-drop to prioritize which facilities are considered first during order routing runs.

### 4.6 Map Facilities to Shopify Locations

1. Facility Details → **External Mappings** tab
2. Click **Map Facility to an External System** → select **Shopify**
3. Choose the Shopify store and enter the **Location ID** from Shopify Admin
   - Find it: Shopify Admin → Settings → Locations → click location → ID in URL
4. Save

> [!IMPORTANT]
> Without external mappings, inventory will not sync to the correct Shopify POS location.

### 4.7 Verify Facilities and Locations

1. Hamburger Menu → **Warehouse → Facilities** → Find Facility page
2. Search for each facility to confirm it exists
3. Click facility → scroll to bottom → verify **Locations** in this format:

| warehouse-id | area-id | aisle-id | section-id | level-id | position-id |
|---|---|---|---|---|---|
| `{facilityID}` | `TL` | `AI` | `AA` | `II` | `1` |

> [!NOTE]
> Locations are usually auto-created with the facility. If missing, add them manually — OMS requires locations for inventory tracking.

---

## Setup Checklist

| # | Task | Done |
|---|---|---|
| **Initial Setup** | | |
| 1 | Log in and reset password |  |
| 2 | Create admin users, disable default user |  |
| 3 | Update Company Profile (name, logo, address, contacts) |  |
| 4 | Add DBIC (operating countries) |  |
| 5 | Configure System Property Data (non-US only) |  |
| **Shopify Shop** | | |
| 5 | Install HotWax Commerce app on Shopify |  |
| 6 | Verify Shopify Shop auto-created in OMS |  |
| 7 | Set access scope to `Read and write` |  |
| 8 | Link Shopify Shop to Product Store |  |
| 9 | Configure product type mappings |  |
| 10 | Configure sales channel mappings |  |
| 11 | Configure payment method mappings |  |
| 12 | Import and map shipping methods |  |
| 13 | Import Shopify locations → create OMS facilities |  |
| 14 | Trigger initial product download |  |
| 15 | Trigger initial order sync |  |
| **Product Store & General Settings** | | |
| 16 | Update Product Store name |  |
| 17 | Configure currency, auto-approve, sales order prefix |  |
| 18 | Verify brokering and inventory reservation are `Y` |  |
| 19 | Configure advanced product store settings |  |
| 20 | Verify General Settings (country, currency, date formats) |  |
| 21 | Add shipping methods to Product Store |  |
| **Facility Setup** | | |
| 22 | Create / import facilities |  |
| 23 | Configure address, phone, zip for each facility |  |
| 24 | Add latitude & longitude for each facility |  |
| 25 | Set operating hours and time zone |  |
| 26 | Configure fulfillment settings per facility |  |
| 27 | Set fulfillment capacity |  |
| 28 | Associate facilities with Product Store |  |
| 29 | Create BROKERING facility group |  |
| 30 | Create CHANNEL FAC GROUP (online inventory) |  |
| 31 | Create PICKUP facility group (BOPIS) |  |
| 32 | Add facilities to relevant groups with correct sequence |  |
| 33 | Map facilities to Shopify locations (external mappings) |  |
| 34 | Verify all facilities and locations exist in OMS |  |

---

## Quick Reference URLs

| Page | URL |
|---|---|
| OMS Login | `https://{instance}.hotwax.io/commerce/control/main` |
| Company Profile | `https://{instance}.hotwax.io/commerce/control/ViewParty?partyId=COMPANY` |
| Shopify Shop | `https://{instance}.hotwax.io/commerce/control/ViewShopifyShop?shopId=<id>` |
| Product Store | `https://{instance}.hotwax.io/commerce/control/ViewStore?productStoreId=<id>` |
| General Settings | `https://{instance}.hotwax.io/commerce/control/GeneralSettings` |
| Import Facilities CSV | `https://{instance}.hotwax.io/commerce/control/ImportData?configId=IMP_FACILITY` |
| System Property Setup | `https://{instance}.hotwax.io/commerce/control/ImportData?configId=SETUP_SYSTEM_PROPERTY` |

## Reference Documentation

| Topic | Link |
|---|---|
| Deployment Overview | https://docs.hotwax.co/everything/hotwax-deployment-and-versions/deployment |
| Company App (Product Store & Shopify) | https://docs.hotwax.co/documents/administration-company-administration-company-administration-company/administration/company |
| Manage Shopify Shop | https://docs.hotwax.co/documents/administration-company-administration-company-administration-company/administration/company/manage-shopify-shop |
| Configure Product Store | https://docs.hotwax.co/documents/administration-company-administration-company-administration-company/administration/company/manage-product-store |
| Learn Shopify Integration | https://docs.hotwax.co/documents/learn-shopify |
| Facilities Administration | https://docs.hotwax.co/documents/administration-company-administration-company-administration-company/administration/facilities |
