# Gap Analysis: Product Store Details

> This document outlines the comparative analysis between the **Legacy OMS Interface** and the **new Company App** for Product Store management. It identifies what is functional, what is read-only, and what is missing — along with the correct workaround for each gap.

---

## Part 1: What We Have in Legacy OMS

The legacy OMS (`/commerce/control/ViewStore`) provides a granular, tab-based configuration for every aspect of a Product Store.

### Core Identity

| Field | Editable | Notes |
|---|---|---|
| Product Store Name | Yes | |
| Product Store ID | No | Unique identifier, read-only after creation |
| External ID | Yes | Used for external system references |
| Company Name (Party ID) | Yes | Links to the Company Profile |

### Financials

| Field | Editable | Notes |
|---|---|---|
| Default Currency | Yes | USD, CAD, GBP, etc. |
| Visual Theme | Yes | Controls storefront appearance |

### Inventory & Order Logic

| Field | Editable | Notes |
|---|---|---|
| Reserve Inventory | Yes | Must remain `Y` — OMS is inventory system of record |
| Enable Brokering | Yes | Must remain `Y` |
| Auto Approve Order | Yes | Toggle for auto-approving imported orders |
| Explode Order Items | Yes | Critical for kit/bundle fulfillment |
| Allow Split | Yes | Allows orders to ship from multiple facilities |
| Pre-order Auto-Releasing | Yes | Automates pre-order release when inventory arrives |
| Sales Order ID Prefix | Yes | Prefix added to internal order IDs |

### Facility Management

| Capability | Available | Notes |
|---|---|---|
| Add Product Store Facility | Yes | Explicit "Add" button in Facility tab |
| Remove Facility from Store | Yes | |
| Set Primary Inventory Facility | Yes | Defines the "home" facility for inventory |
| Sequence Facilities for Brokering | Yes | Drag-and-drop priority ordering |

### Shipping & Fulfillment

| Capability | Available | Notes |
|---|---|---|
| Add Shipping Methods | Yes | Full carrier + method + gateway config |
| Configure Carrier Gateways | Yes | Per-method gateway setup |
| Set Tracking Requirements | Yes | Flags per shipping method |
| View/Manage Shipment Method List | Yes | All methods listed with carrier details |

### Operating Countries (DBIC)

| Capability | Available | Notes |
|---|---|---|
| View DBIC countries | Yes | Full list visible |
| Add country/geo | Yes | Add from dropdown |
| Remove country/geo | Yes | |

### Branding

| Capability | Available | Notes |
|---|---|---|
| Update Instance Logo | Yes | Via Company Profile → Change logo link |
| Update Company Name | Yes | Editable in Company Profile |

### Store Settings (Advanced)

Additional key-value settings configurable in Legacy OMS:

| Setting | Description |
|---|---|
| `AFFECT_QOH_ON_REJ` | Affect Quantity on Hand on order rejection |
| `BOPIS_PART_ODR_REJ` | Allow partial rejection of BOPIS orders |
| `BRK_SHPMNT_THRESHOLD` | Minimum order value threshold before splitting shipments |
| `CUST_ALLOW_CNCL` | Allow customers to cancel before fulfillment |
| `CUST_DLVRADR_UPDATE` | Allow delivery address changes |
| `CUST_DLVRMTHD_UPDATE` | Allow delivery method changes |
| `CUST_PCKUP_UPDATE` | Allow pickup location changes |
| `FF_DOWNLOAD_PICKLIST` | Download picklist as CSV vs PDF |
| `FF_COLLATERAL_REJ` | Collateral rejection behavior |
| `AUTO_REJ_IDLE_ORD` | Auto-reject idle orders after N minutes |
| `RATE_SHOPPING` | Enable carrier rate comparison |

---

## Part 2: What We Have in Company App

The Company App (`/product-store-details/<id>`) provides a modern, card-based interface focused on the most frequently used operational toggles.

### A. Functional (Working as Expected)

| Section | Field / Toggle | State |
|---|---|---|
| **Store Identity** | Product Store Name | Editable via dialog |
| **Store Identity** | Store ID | Read-only |
| **Store Identity** | Company | Read-only |
| **Administration** | Order Brokering | Toggle (on/off) |
| **Administration** | Order Reservations | Toggle (on/off) |
| **Orders** | Sales Order ID Prefix | Editable input |
| **Orders** | Save Billing Information | Toggle |
| **Orders** | Approve on Import | Toggle |
| **Orders** | Return Creation Deadline Days | Editable input |
| **Brokering** | Order Splitting | Toggle |
| **Brokering** | Minimum Shipment Threshold | Editable input |
| **Brokering** | Preselected Facility Tag | Editable input |
| **Brokering** | Shipping Facility Tag | Editable input |
| **Fulfillment** | Send Notification to Shopify | Toggle |
| **Fulfillment** | Auto Order Cancellations | Toggle |
| **Fulfillment** | Auto Cancellation Days | Editable input |
| **Store Pickup** | Partial Order Rejection | Toggle |
| **Inventory** | Show Systemic Inventory | Toggle |
| **Inventory** | Hold Pre-order Physical Inventory | Toggle |
| **Reroute Fulfillment** | Allow Cancel Before Fulfillment | Toggle |
| **Reroute Fulfillment** | Allow Delivery Address Change | Toggle |
| **Reroute Fulfillment** | Allow Delivery Method Change | Toggle |
| **Reroute Fulfillment** | Allow Pickup Location Change | Toggle |
| **Product** | Global Product Identifier | Selectable |

### B. Non-Functional / Read-Only

| Feature | Current State | Issue |
|---|---|---|
| **Store ID** | Read-only | Expected — unique identifier |
| **Company Name** | Read-only | Expected |
| **Operating Countries** | Displays count only (e.g., "2 countries") | Cannot add or remove countries from this view |
| **Facilities button** | Visible on store list page | Non-functional — does not open a facility management view |
| **Shipping Methods button** | Visible on store list page | Non-functional — does not open shipping method management |

---

## Part 3: What Is Missing in Company App

These configurations are **absent from the Company App** and require switching back to Legacy OMS. Each gap includes the correct workaround.

| # | Missing Feature | Impact | Workaround (Where to Configure) |
|---|---|---|---|
| 1 | **Add / Remove Product Facilities** | Cannot associate new stores or warehouses with this Product Store | Use **Facilities App** → Facility Details → Product Stores card |
| 2 | **Add / Configure Shipping Methods** | Cannot set up carrier-method-gateway combinations | Use **Fulfillment App** or Legacy OMS → Product Store → Shipment Methods tab |
| 3 | **DBIC / Operating Countries** | Cannot add or view which countries this store is active in | Use Legacy OMS → Settings → DBIC Configuration |
| 4 | **Update Instance / Company Logo** | Company branding not manageable in Company App | Use Legacy OMS → `ViewParty?partyId=COMPANY` → Change logo |
| 5 | **Currency** | Store's functional currency is not visible or configurable | Use Legacy OMS → Product Store → Edit → Default Currency field |
| 6 | **Primary Inventory Facility** | Cannot define the "Home" facility used for inventory availability calculations | Use Legacy OMS → Product Store → Facility tab |
| 7 | **Explode Order Items (Kit/Bundle Logic)** | Multi-part product fulfillment behaves incorrectly if not set | Use Legacy OMS → Product Store → Edit → Explode Order Items |
| 8 | **Pre-order Auto-Releasing** | Pre-orders are not automatically released when inventory arrives | Use Legacy OMS → Product Store → Edit |
| 9 | **Carrier Gateway Configuration** | Cannot set up or change shipping carrier integrations | Use Legacy OMS → Product Store → Shipment Methods tab |
| 10 | **Tracking Requirement Flags** | Cannot mark specific shipping methods as requiring tracking | Use Legacy OMS → Product Store → Shipment Methods tab |
| 11 | **Advanced Store Settings** | Settings like `AFFECT_QOH_ON_REJ`, `AUTO_REJ_IDLE_ORD`, `RATE_SHOPPING` etc. not exposed | Use Legacy OMS → Product Store → Settings tab |

---

## Summary

| Category | Legacy OMS | Company App |
|---|---|---|
| Core store identity | Full edit | Partial (name only) |
| Currency | Configurable | Missing |
| Inventory toggles | Full control | Functional |
| Order handling toggles | Full control | Mostly functional |
| Facility association | Full add/remove/sequence | Not available (use Facilities App) |
| Shipping method setup | Full add/configure | Not available (use OMS/Fulfillment App) |
| DBIC / Countries | Full add/remove | Read-only count only |
| Kit/Bundle logic | Available | Missing |
| Pre-order automation | Available | Missing |
| Carrier gateways | Available | Missing |
| Instance logo/branding | Via Company Profile | Not available |
| Advanced store settings | Full key-value config | Not exposed |

> [!NOTE]
> The Company App is designed for day-to-day operational management. **Initial setup** (currency, facilities, shipping methods, DBIC, logo, kit logic) must still be completed via the **Legacy OMS** before the store is handed to operations teams.
