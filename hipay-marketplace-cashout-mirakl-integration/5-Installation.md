# Installation

Before starting the installation, please read all instructions and make sure you've gone through the [Prerequisites and recommendations](https://developer.hipay.com/doc/hipay-marketplace-cashout-mirakl-integration/#prerequisites-and-recommendations) and [Mirakl account configuration](https://developer.hipay.com/doc/hipay-marketplace-cashout-mirakl-integration/#mirakl-account-configuration) sections. 

Please note that **all relative paths are relative to the root directory of the installation**.

## Application deployment

This section describes how to install the **HiPay Marketplace cash-out integration for Mirakl** using Git and [Composer](https://getcomposer.org/), the PHP package manager. The software being installed is based on the [Silex PHP micro-framework](http://silex.sensiolabs.org/).

1. Connect to your web server using SSH.

2. Go to your web project directory (example: `$ cd /var/www` or `$ cd /srv`).
	
3. Download and install Composer (if you haven't already done so): 

	`$ curl -sS https://getcomposer.org/installer | php -- --filename=composer -- --install-dir=/usr/local/bin`
	
4. Download the project using Git:

	`$ git clone https://github.com/hipay/hipay-wallet-cashout-mirakl-integration hipay_mirakl`

5. Go to the project directory: `cd hipay_mirakl`.

6. Install the project dependencies using Composer:

	`$ composer install` 
	
	This step may take a few minutes to complete as the project and its dependencies are being downloaded and configured.
7. (Optional) If you use the default logs file (`/var/log/hipay.log`), run the following commands: 
        
        `$ touch /var/log/hipay.log`

        `$ chmod 755 /var/log/hipay.log`

During the installation, Composer will ask you to provide some parameters, including your HiPay account credentials and your Mirakl API credentials. Please go to the [Prerequisites and recommendations](https://developer.hipay.com/doc/hipay-marketplace-cashout-mirakl-integration/#prerequisites-and-recommendations) section if you need more information about these parameters.

## HiPay cash-out notifications

This section describes how to provide HiPay with information on how to reach your server in order to automatically inform your application upon transaction status update (for example, when a withdrawal has been validated). HiPay notifies your application through HTTP requests.

1. Configure your web server so that HiPay can reach the `web/index.php` through HTTP. This configuration is beyond the scope of the present guide and depends on the web server software you rely on (Apache, Nginx, etc.).

2. Note the URL from which the `web/index.php` is reachable (example: `https://cashout.merchant-example.com/index.php`). Then, [contact HiPay's Business IT Services](https://support.hipay.com) by submitting a request to configure this URL as your marketplace notification URL.

## Initialization and final check

Go to the project directory: 

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

### Documents

| Field            | Type         | Null | Key | Default | Extra          |
|------------------|--------------|------|-----|---------|----------------|
| id               | int(11)      | NO   | PRI | NULL    | auto_increment |
| vendor_id        | int(11)      | YES  |     | NULL    |                |
| miraklDocumentId | int(11)      | YES  |     | NULL    |                |
| miraklUploadDate | datetime     | NO   |     | NULL    |                |
| documentType     | varchar(255) | NO   |     | NULL    |                |

### Batches

| Field            | Type         | Null | Key | Default | Extra          |
|------------------|--------------|------|-----|---------|----------------|
| id               | int(11)      | NO   | PRI | NULL    | auto_increment |
| name             | longtext     | NO   |     | NULL    |                |
| error            | longtext     | YES  |     | NULL    |                |
| started_at       | datetime     | NO   |     | NULL    |                |
| ended_at         | datetime     | YES  |     | NULL    |                |

### log_general

| Field            | Type         | Null | Key | Default | Extra          |
|------------------|--------------|------|-----|---------|----------------|
| id               | int(11)      | NO   | PRI | NULL    | auto_increment |
| miraklId         | int(11)      | YES  |     | NULL    |                |
| action           | longtext     | YES  |     | NULL    |                |
| message          | longtext     | NO   |     | NULL    |                |
| context          | longtext     | NO   |     | NULL    |                |
| level            | smallint(6)  | NO   |     | NULL    |                |
| level_name       | varchar(50)  | NO   |     | NULL    |                |
| extra            | longtext     | NO   |     | NULL    |                |
| created_at       | datetime     | NO   |     | NULL    |                |

### log_operations

| Field            | Type         | Null | Key | Default | Extra          |
|------------------|--------------|------|-----|---------|----------------|
| id               | int(11)      | NO   | PRI | NULL    | auto_increment |
| miraklId         | int(11)      | YES  |     | NULL    |                |
| hipayId          | int(11)      | YES  |     | NULL    |                |
| paymentVoucher   | varchar(255) | NO   |     | NULL    |                |
| dateCreated      | datetime     | YES  |     | NULL    |                |
| amount           | double       | NO   |     | NULL    |                |
| statusTransferts | int(11)      | YES  |     | NULL    |                |
| statusWithDrawal | int(11)      | YES  |     | NULL    |                |
| message          | varchar(255) | YES  |     | NULL    |                |
| balance          | varchar(255) | YES  |     | NULL    |                |

### log_vendors

| Field               | Type         | Null | Key | Default | Extra          |
|---------------------|--------------|------|-----|---------|----------------|
| id                  | int(11)      | NO   | PRI | NULL    | auto_increment |
| miraklId            | int(11)      | NO   |     | NULL    |                |
| hipayId             | int(11)      | NO   |     | NULL    |                |
| login               | varchar(255) | YES  |     | NULL    |                |
| statusWalletAccount | int(11)      | NO   |     | NULL    |                |
| status              | int(11)      | NO   |     | NULL    |                |
| message             | varchar(255) | YES  |     | NULL    |                |
| nbDoc               | int(11)      | NO   |     | NULL    |                |
| date                | datetime     | NO   |     | NULL    |                |

To verify that your software is properly installed and configured, please go to the URL you sent to HiPay (example: `https://cashout.merchant-example.com/index.php`). **The login page of the dashboard should be displayed**. If that's not the case, make sure that you properly configured the `parameters.yml` file and that all the information provided in it is correct.
