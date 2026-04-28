# HotWax Commerce Setup Checklist

> Use this checklist to track progress during a new HotWax Commerce instance setup.
> Refer to the full setup guide for step-by-step instructions: [hotwax-commerce-setup-guide.md](hotwax-commerce-setup-guide.md)

---

## Phase 1: Initial Setup

| # | Task | Done |
|---|---|---|
| 1 | Log in to OMS and reset default password | [ ] |
| 2 | Create new admin users and disable the default user | [ ] |
| 3 | Update Company Profile — name, logo, primary address, contacts | [ ] |
| 4 | Add DBIC (Doing Business In Countries) for relevant operating countries | [ ] |
| 5 | Configure System Property Data — currency, country, weight units (non-US retailers only) | [ ] |

---

## Phase 2: Shopify Shop Setup

| # | Task | Done |
|---|---|---|
| 6 | Install HotWax Commerce integration app on Shopify store | [ ] |
| 7 | Verify Shopify Shop record is auto-created in OMS | [ ] |
| 8 | Set Shopify access scope to `Read and write` | [ ] |
| 9 | Link Shopify Shop to the correct Product Store | [ ] |
| 10 | Import and configure product type mappings (e.g. Gift Card → DIGITAL_GOOD) | [ ] |
| 11 | Import and configure sales channel mappings (e.g. web, pos, android) | [ ] |
| 12 | Import and configure payment method mappings (e.g. paypal, afterpay) | [ ] |
| 13 | Import and map Shopify shipping methods to OMS carrier shipment methods | [ ] |
| 14 | Import Shopify locations and create/map corresponding OMS facilities | [ ] |
| 15 | Trigger initial product download from Shopify | [ ] |
| 16 | Trigger initial order sync from Shopify | [ ] |

---

## Phase 3: Product Store & General Settings

| # | Task | Done |
|---|---|---|
| 17 | Update Product Store name to the retailer's brand name | [ ] |
| 18 | Configure currency, auto-approve order, and sales order ID prefix | [ ] |
| 19 | Confirm brokering (`Y`) and inventory reservation (`Y`) are not changed | [ ] |
| 20 | Add required advanced product store settings (BOPIS rejection, cancellation threshold, etc.) | [ ] |
| 21 | Verify General Settings — default country, currency, date formats | [ ] |
| 22 | Add shipping methods to the Product Store (carrier + shipment method + gateway) | [ ] |

---

## Phase 4: Facility Setup

| # | Task | Done |
|---|---|---|
| 23 | Create or bulk import all facilities (warehouse / retail store) | [ ] |
| 24 | Configure address, phone number, and zip code for each facility | [ ] |
| 25 | Add latitude and longitude for each facility (for brokering and BOPIS) | [ ] |
| 26 | Set operating hours and time zone for each facility | [ ] |
| 27 | Configure fulfillment settings — allow pickup, native fulfillment app, generate shipping labels, days to ship | [ ] |
| 28 | Set fulfillment capacity (unlimited / custom order count per day) | [ ] |
| 29 | Associate each facility with the correct Product Store | [ ] |
| 30 | Create BROKERING facility group and add eligible facilities in priority sequence | [ ] |
| 31 | Create CHANNEL FAC GROUP (online inventory) and add facilities | [ ] |
| 32 | Create PICKUP facility group (for BOPIS) and add facilities | [ ] |
| 33 | Add facilities to all other relevant groups (Same Day Shipping, OMS Fulfillment, Generate Shipping Label) | [ ] |
| 34 | Map each facility to its corresponding Shopify location via external mappings | [ ] |
| 35 | Verify all facilities exist in OMS and have locations created | [ ] |

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
