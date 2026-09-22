# NeuronSearchLab Recommendations

User guide for Magento Open Source 2.4, module version 1.0.1

## What the module does

The module adds recommendation locations to the home, product, category, search, and cart pages. It reads the page's product, category, or search context from Magento. Basket and signed-in customer context come from Magento's private customer-data section, so they are not rendered into shared page-cache HTML. The checkout success page reports an order using its Magento order number for deduplication.

The extension is free to install. A NeuronSearchLab workspace and a publishable Website embed key are required for recommendations to appear. The workspace must contain catalogue items; a new, empty workspace cannot return product cards. NSL service access is separate from the extension.

## Requirements

- Magento Open Source or Adobe Commerce 2.4.x with a supported PHP version.
- Composer access to the extension package and permission to run Magento CLI commands.
- A NeuronSearchLab workspace containing catalogue items.
- A publishable Website embed key (`nsl_pk_...`) with each storefront origin allowed.

## Install

After obtaining the extension through Commerce Marketplace, install its Composer package in your Magento project:

```
composer require neuronsearchlab/module-recommendations
bin/magento module:enable NeuronSearchLab_Recommendations
bin/magento setup:upgrade
bin/magento setup:di:compile
bin/magento setup:static-content:deploy -f
bin/magento cache:flush
```

Deploy the changed project using your normal release process. The extension is disabled until an embed key is configured.

## Configure

1. In the NSL console, open Developers > Website embed. Create a publishable key and allow every storefront origin, including staging and production.
2. In Magento Admin, open Stores > Configuration > NeuronSearchLab > Recommendations.
3. Set Enabled to Yes and paste the publishable key into Embed key. Leave Widget version at `v1` for automatic updates, or pin a tested version. Save the configuration and flush the Magento cache.
4. Open a product page and check the browser's network panel for the NSL embed and recommendation request. The console's Website embed page reports rejected origins.

Never put an NSL client secret or server API key into Magento's Embed key field. This field appears in page source and must contain only a publishable key.

## Placement and context

With the module enabled, the default theme receives six-card strips on the home, product, category, search, and cart pages. The placement names are `home-feed`, `pdp-related`, `category-feed`, `search-feed`, and `cart-feed`. Configure each placement's presentation in the NSL console. A theme may move or remove these blocks through Magento layout XML; the package includes no hidden product tags to maintain.

The product page sends its SKU. Category and search pages send their current category or query. Cart quantities and prices are read from the active Magento quote. The order is reported on the checkout success page. The module does not import Magento's product catalogue or order history into NSL; arrange a catalogue feed or supported import before expecting recommendations.

## Check a staging store

Use a staging origin on the embed key. Test a simple and configurable product page, category, search, empty and populated cart, guest and signed-in customer, and a completed checkout. Verify that two browser sessions never show one another's cart or customer context. Check that product and category pages remain cacheable with your production cache configuration. Confirm the storefront and checkout remain usable if the NSL CDN or API is unavailable.

## Troubleshooting

- No strip: confirm Enabled, the publishable key, an allowed origin, and that the theme has not removed the layout block.
- Empty strip: confirm catalogue items exist in the NSL workspace and the requested product or placement is configured. Check the browser network request and console's Website embed diagnostics.
- Old cart or customer: reload the page, inspect Magento's `nsl-context` customer-data section, and test login/logout and cart changes in separate browser sessions.
- Checkout event missing: use a completed Magento checkout success page; verify the order contains visible line items and the embed key allows the origin.

## Disable or remove

Set Enabled to No and flush cache to stop storefront delivery. To uninstall the module, run `bin/magento module:disable NeuronSearchLab_Recommendations`, remove the Composer package, run `bin/magento setup:upgrade`, then flush cache. Existing NSL workspace data remains subject to the workspace's own retention settings.

Support: use the support channel associated with your NSL workspace or open an issue at https://github.com/NeuronSearchLab/nsl-platforms/issues.
