# NeuronSearchLab Recommendations for Magento 2

Download the free [Magento Composer ZIP](https://github.com/NeuronSearchLab/nsl-magento/releases/download/v1.0.1/nsl-magento-marketplace.zip) (version 1.0.1; SHA-256 `44fd484554534d84132c1aab3b5262c481022014e3d8164d787534681762d3d4`). The public archive was updated on 11 October 2026 with the ACL correction submitted to Adobe. Adobe's Code Sniffer, Installation & Varnish, and MFTF automated tests passed on 24 September; manual QA and marketing review are still pending, so the extension is **not yet Marketplace-certified**. A disposable Magento Open Source 2.4.9 Composer installation, module upgrade, dependency injection compilation, static asset deployment, reindex, and HTTP storefront checks for home, search, and cart placements also passed. A merchant still needs to import a catalogue into NSL and configure a publishable embed key. These checks do not establish a full customer checkout, customer isolation on a production cache, custom-theme, or Adobe Commerce edition compatibility.

The extension is free. Recommendations require a separately provisioned NeuronSearchLab service workspace, a publishable Website embed key, and catalogue items imported into NSL. Service charges are separate from the extension. This extension does not sync the Magento catalogue or order history into NSL.

## Install

Download the ZIP into a directory outside your Magento project, then register that directory as a Composer artifact repository:

```bash
composer config repositories.nsl-artifact artifact /path/to/zip-directory
composer require neuronsearchlab/module-recommendations:1.0.1
bin/magento module:enable NeuronSearchLab_Recommendations
bin/magento setup:upgrade
bin/magento setup:di:compile
bin/magento setup:static-content:deploy -f
bin/magento cache:flush
```

In Magento Admin, open **Stores → Configuration → NeuronSearchLab → Recommendations**. Enable the extension and enter your publishable NSL embed key. Allow your storefront origin in the NSL console. The extension adds placements on home, product, category, search, and cart pages.

See the [user guide](USER_GUIDE.md) or [PDF guide](NeuronSearchLab-Magento-User-Guide.pdf) for configuration, staging checks, troubleshooting, and removal. Please test on staging before production. Support and bug reports: [GitHub issues](https://github.com/NeuronSearchLab/nsl-magento/issues).
