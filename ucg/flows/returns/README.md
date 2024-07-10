---
description: >-
  Explore how Frank and Oak handles returns, leveraging Shopify's native
  function for in-store returns and Loop Returns for online returns.
---

# Returns

For in-store returns, the UCG faced a choice between the Loop Returns POS app and Shopify's native returns function. Opting for simplicity, they chose Shopify's native returns function, enabling staff to manage returns without navigating an additional interface. While Loop Returns continues to handle online returns, the decision was made to restrict customers from initiating returns for in-store purchases through online channels.

**In store returns:** Shopify

**Online / Self service returns:** Loop

To support the online and in-store return data flow, a custom adapter was crafted in Boomi. This adapter leverages the Order Management System's (OMS) native returns feed, converting it to align with the schema of existing NetSuite connectors for returns. This strategic choice avoids the need for extensive reworking of NetSuite-related transformation logic, preserving established business processes.

## Return Reasons

An additional layer was added to capture return reasons saved by in-store staff in Shopify. Although this information is stored on the return, it is not inherently included in the return JSON appended to the order JSON. To address this, a separate enrichment flow was introduced, facilitating the synchronization of return reasons from Shopify for recently created returns in the OMS.

## Cash Sales and Sales Orders

When syncing returns to NetSuite, Frank and Oak implemented two distinct flows—one for Sales Orders being returned and another for Cash Sales. Sales Orders are processed as standard refunds in NetSuite, while Cash Sales are treated as Cash Refunds. To clearly differentiate between these types of returns, the out-of-the-box Return feed was extended to include information necessary for identifying the origin order as a Cash Sale or Sales Order. The parameters are the requested shipping method of the orignal order item. If the method is POS\_COMPLETD that indicates that the order being returned was a Cash Sale.