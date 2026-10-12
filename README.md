# NeuronSearchLab Recommendations for Magento 2

Personalised product recommendations for Magento Open Source and Adobe Commerce 2.4. Connect the store from Magento admin and the module sets itself up: it creates your NeuronSearchLab workspace, switches the storefront integration on and keeps your catalogue in sync. There is no tagging to write and no key to copy.

Download the free [Magento Composer ZIP](https://github.com/NeuronSearchLab/nsl-magento/releases/download/v1.1.0/nsl-magento-marketplace.zip) (version 1.1.0; SHA-256 `3903d60f013963f835d48397d153d8728d37e54d660a626db0774af284c682b3`).

Version 1.1.0 is tested in CI on a disposable Magento Open Source 2.4.9. The tests install the module, compile dependency injection, render the admin connection panel, connect to NeuronSearchLab, sync a catalogue and confirm its products are recommended, then disconnect and delete the test data. Version 1.0.1 is in Adobe Commerce Marketplace review, so the extension is **not yet Marketplace-certified**. Please test on staging before production.

## Install

Download the ZIP into a directory outside your Magento project, then register that directory as a Composer artifact repository:

```bash
composer config repositories.nsl-artifact artifact /path/to/zip-directory
composer require neuronsearchlab/module-recommendations:1.1.0
bin/magento module:enable NeuronSearchLab_Recommendations
bin/magento setup:upgrade
bin/magento setup:di:compile
bin/magento setup:static-content:deploy -f
bin/magento cache:flush
```

## Connect

In Magento Admin, open **Stores → Configuration → NeuronSearchLab → Recommendations** and select **Connect to NeuronSearchLab**, or run `bin/magento neuronsearchlab:connect`. Recommendations appear on home, product, category, search and cart pages once the catalogue has been imported, usually within a few minutes. The catalogue syncs hourly through Magento cron; **Sync catalogue now** and `bin/magento neuronsearchlab:catalogue:sync` sync on demand.

A connected store starts on NeuronSearchLab's free plan (10,000 storefront recommendation requests a month). Placements, ranking rules, analytics and plans are in the NeuronSearchLab console: select **Open NeuronSearchLab console** in the connection panel.

See the [user guide](USER_GUIDE.md) or [PDF guide](NeuronSearchLab-Magento-User-Guide.pdf) for catalogue sync, staging checks, troubleshooting and removal. Support: support@neuronsearchlab.com or [GitHub issues](https://github.com/NeuronSearchLab/nsl-magento/issues).
