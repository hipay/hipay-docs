# Installation

Before starting the installation, please read all instructions and make sure you've gone through the [[Prerequisites and recommendations]] and [[Mirakl account configuration]] sections. 

Be aware that **all relative paths are relative to the root directory of the installation**.

## Application deployment

This section describes how to install the **HiPay Wallet cash-out integration for Mirakl** using Git and [Composer](https://getcomposer.org/), the PHP package manager. The software being installed is based on the [Silex PHP micro-framework](http://silex.sensiolabs.org/).

1. Connect to your web server using SSH

2. Go in your web projects directory (example: `$ cd /var/www` or `$ cd /srv`)
	
5. Download and install Composer (if you haven't already done so): 

	`$ curl -sS https://getcomposer.org/installer | php -- --filename=composer -- --install-dir=/usr/local/bin`
	
6. Download the project using Git:

	`$ git clone https://github.com/hipay/hipay-wallet-cashout-mirakl-integration hipay_mirakl`

7. Go in the project directory: `cd hipay_mirakl`

8. Install the project's dependencies using Composer:

	`$ composer install` 
	
	This step may take a few minutes to complete as the project and its dependencies are being downloaded and configured.

During the installation, Composer will ask you to provide some parameters, including your HiPay Wallet account credentials and your Mirakl API credentials. Please go to the [[Prerequisites and recommendations]] section if you need more information about these parameters.

## HiPay cash-out notifications

This section describes how to provide HiPay with information on how to reach your server in order to automatically inform your application upon transaction status update (for example, when a withdrawal has been validated). HiPay notifies your application through HTTP requests.

1. Configure your web server so that HiPay can reach the `web/index.php` through HTTP. This configuration is beyond the scope of the present guide and depends on the web server software you rely on (Apache, Nginx, etc.).

2. Note the URL from which the `web/index.php` is reachable (example: `https://cashout.merchant-example.com/index.php`). Then, [contact HiPay's Business IT Services](http://help.hipay.com/) to configure this URL as your marketplace notification URL.

## Initialization and final check

Go in the project directory: 

	$ cd hipay_mirakl

Run the following command:

	$ php bin/console orm:schema-tool:update --dump-sql --force

This command will initialize the database with the needed tables. You should get this database schema:
	
### Vendors

| Field    | Type         | Null | Key | Default | Extra          |
|----------|--------------|------|-----|---------|----------------|
| id       | int(11)      | NO   | PRI | NULL    | auto_increment |
| miraklId | int(11)      | NO   | UNI | NULL    |                |
| email    | varchar(255) | NO   | UNI | NULL    |                |
| hipayId  | int(11)      | NO   | UNI | NULL    |                |

### Operations

| Field          | Type         | Null | Key | Default | Extra          |
|----------------|--------------|------|-----|---------|----------------|
| id             | int(11)      | NO   | PRI | NULL    | auto_increment |
| miraklId       | int(11)      | YES  |     | NULL    |                |
| hipayId        | int(11)      | YES  |     | NULL    |                |
| amount         | double       | NO   |     | NULL    |                |
| cycleDate      | datetime     | NO   |     | NULL    |                |
| withdrawId     | varchar(255) | YES  | UNI | NULL    |                |
| transferId     | varchar(255) | YES  | UNI | NULL    |                |
| status         | int(11)      | NO   |     | NULL    |                |
| updatedAt      | datetime     | NO   |     | NULL    |                |
| paymentVoucher | varchar(255) | NO   |     | NULL    |                |


To verify that your software is properly installed and configured, please go to the URL you sent to HiPay (example: `https://cashout.merchant-example.com/index.php`). **A white page with the "*Hello World*" text message should be displayed**. If that's not the case, make sure that you properly configured the `parameters.yml` file and that all the information provided in it is correct.
