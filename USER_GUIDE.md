# NeuronSearchLab Recommendations

User guide for Magento Open Source and Adobe Commerce 2.4, module version 1.1.0

## What the module does

The module adds personalised product recommendations to the home, product, category, search and cart pages. It connects your store to NeuronSearchLab from Magento admin. It also sends your catalogue to NeuronSearchLab and keeps it up to date, and gives the recommendation widget the page, product, category, search, basket and order context it needs. You write no tagging, and you copy no keys.

Basket and signed-in customer context come from Magento's private customer-data section, so they are never rendered into shared page-cache HTML. The checkout success page reports the order using its Magento order number, so a reload is not counted twice.

## Requirements

- Magento Open Source or Adobe Commerce 2.4.x with a supported PHP version.
- Composer access to the extension package and permission to run Magento CLI commands.
- Outbound HTTPS from the Magento server to console.neuronsearchlab.com.
- Magento cron running, for the hourly catalogue sync. Without cron, use **Sync catalogue now** or the CLI command after catalogue changes.

The extension is free. Connecting creates a NeuronSearchLab workspace on the free plan, which includes 10,000 storefront recommendation requests a month. Larger plans are billed by NeuronSearchLab in its console; the module never charges through Magento.

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

Deploy the changed project using your normal release process. Recommendations stay off until you connect the store.

## Connect

1. In Magento Admin, open **Stores > Configuration > NeuronSearchLab > Recommendations**.
2. Under **Connection**, select **Connect to NeuronSearchLab**.

That single step:

- creates a NeuronSearchLab workspace for the store;
- creates a storefront embed key that accepts every store view's base URL, writes it into **Embed key**, and sets **Enabled** to Yes;
- sends the catalogue: every enabled product that is visible in the catalogue or search, with its name, short description, URL, image, final price, currency, categories and stock status.

Recommendations appear once NeuronSearchLab has imported the catalogue, usually within a few minutes. The **Connection** panel then shows how many products were sent and how many are being recommended, and when the storefront was last seen.

From the command line, `bin/magento neuronsearchlab:connect` does the same, for scripted deployments.

## Keep the catalogue in sync

The catalogue syncs every hour through Magento cron (job `neuronsearchlab_catalogue_sync`). Each sync sends the whole catalogue. A product that is disabled, hidden, deleted or out of stock stops being recommended, and returns when it is available again.

To sync immediately, select **Sync catalogue now**, or run `bin/magento neuronsearchlab:catalogue:sync`. `bin/magento neuronsearchlab:status` prints the connection and catalogue status.

Stores up to 50,000 products are supported. Staging and development stores sync the same way as production, because the module sends the catalogue to NeuronSearchLab rather than NeuronSearchLab fetching it.

## Placements and the console

With the module connected, the default theme receives six-card strips on the home, product, category, search and cart pages. The placement names are `home-feed`, `pdp-related`, `category-feed`, `search-feed` and `cart-feed`. A theme may move or remove these blocks through Magento layout XML.

To configure what each placement serves, add ranking rules or see analytics, select **Open NeuronSearchLab console** in the **Connection** panel. Sign in to NeuronSearchLab, or create a free account. The store's workspace is then added to your account. The link is valid for 30 minutes and can be used once; select the button again for a new one.

## What the storefront sends

The product page sends its SKU, which matches the synced catalogue. Category and search pages send their current category or query. Cart quantities and prices are read from the active Magento quote. The order is reported on the checkout success page. A signed-in customer's ID is sent and hashed per workspace before storage; no name, email address or address is sent.

## Check a staging store

Connect the staging store separately; it gets its own workspace. Test a simple and a configurable product page, a category, a search, an empty and a populated cart, a guest and a signed-in customer, and a completed checkout. Verify that two browser sessions never show one another's cart or customer context. Check that product and category pages remain cacheable with your production cache configuration. Confirm that the storefront and checkout remain usable if NeuronSearchLab is unavailable.

## Troubleshooting

- **Connect fails:** the Magento server needs outbound HTTPS to console.neuronsearchlab.com. The error message in admin names the cause.
- **No strip:** confirm **Enabled** is Yes and the store is connected, and that the theme has not removed the layout block. Add `?nsl_debug=1` to any page for the widget's diagnostic overlay.
- **Empty strip:** check the **Connection** panel shows products being recommended. If not, select **Sync catalogue now** and wait a few minutes.
- **Old cart or customer:** reload the page, inspect Magento's `nsl-context` customer-data section, and test login/logout and cart changes in separate browser sessions.
- **Checkout event missing:** use a completed checkout success page with visible order lines.

## Disconnect or remove

Select **Disconnect** in the **Connection** panel, or run `bin/magento neuronsearchlab:disconnect`. This switches recommendations off, revokes the embed key and stops catalogue sync. Add `--purge` to the command to also delete the catalogue and events NeuronSearchLab holds for the store.

To uninstall the module, disconnect first, then run `bin/magento module:disable NeuronSearchLab_Recommendations`, remove the Composer package, run `bin/magento setup:upgrade`, and flush the cache.

Support: email support@neuronsearchlab.com, or open an issue at https://github.com/NeuronSearchLab/nsl-magento/issues.
