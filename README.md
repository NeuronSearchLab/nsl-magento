# NeuronSearchLab Recommendations for Magento 2

Download the free [Magento Composer ZIP](https://github.com/NeuronSearchLab/nsl-magento/releases/download/v1.0.1/nsl-magento-marketplace.zip) (version 1.0.1). Adobe Commerce Marketplace review is pending; this public download has **not** been certified by Adobe. It has passed a disposable Magento Open Source 2.4.9 Composer installation, module upgrade, dependency injection compilation, static asset deployment, reindex, and an HTTP storefront check for the home placement. These checks do not establish checkout, customer-isolation, Varnish, custom-theme, or Adobe Commerce edition compatibility.

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
