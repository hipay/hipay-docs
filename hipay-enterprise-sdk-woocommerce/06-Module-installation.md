# Plugin installation 

## Get the installation package

Download the ZIP package of the latest release at this address [https://github.com/hipay/hipay-enterprise-sdk-woocommerce/releases](https://github.com/hipay/hipay-enterprise-sdk-woocommerce/releases).

The ZIP package is named **hipay-enterprise-sdk-woocommerce-{version-number}.zip**

## Upload the plugin via the WordPress plugin management page

To install the plugin, log in to your WordPress dashboard: go to "_Plugins -> Add New_" and click on "Upload plugin".  

Choose the package and click on "_Install Now_".

![legend](images/upload_plugin.png)

## Upload the plugin via FTP (SFTP)

You must have a file transfer software like "_FileZilla_" for example.

1. Open your software and connect to your FTP (SFTP).
2. Go to the root of your WordPress project.
3. Transfer the "_woocommerce_hipayenterprise_" source plugin in the "_/wp-content/plugins/_" folder.
4. Add write permissions recursively to the folder "woocommerce_hipayenterprise" (766).

## Activate the plugin 

1. Go to "_Plugins -> Installed Plugins_". 
2. Search for the "_WooCommerce HiPay Enterprise_" plugin.
3. Verify the plugin version. 
3. Click on "_Activate_" in the plugin description.

![legend](images/activate-plugin.png)
