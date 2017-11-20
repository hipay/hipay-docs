# Update

This section is useful if you already have the HiPay Marketplace cash-out integration for Mirakl installed on your server and want to update it.

Please remember that the HiPay Marketplace cash-out **integration** for Mirakl project relies on a ***core library*** which is named [HiPay Marketplace cash-out **library** for Mirakl](https://github.com/hipay/hipay-wallet-cashout-mirakl-library). You can either update the full project or the core library only. In most cases, you will only need to update the core library. Only update the full project if new versions contain features you need or on the recommendation of HiPay's Business IT Services.

Both procedures are documented below.

## Core library update procedure (common use case)

Go to the root directory of the project (where `composer.json` is) and run the following command:

	$ composer update hipay/hipay-wallet-cashout-mirakl-library

This will install the new version of the HiPay Marketplace cash-out integration for Mirakl and its core library.

The output should look like:

```
Loading composer repositories with package information
Updating dependencies (including require-dev)
  - Updating hipay/hipay-wallet-cashout-mirakl-library
    Checking out 2f7f50131839e6c568b9a903ba7e31c6c5fc8847
```

## Full project update procedure

For more safety, it is highly recommanded to make a daily backup of your database.

There are 3 different ways to perform a full project update.

### Update through the GUI project

Please see the [dashboard section](#dashboard-settings-update-the-application).

Note: Update through GUI requires that the "Github token" setting has been set. Please refer to [this section](#dashboard-settings-update-settings) to set your Github token.

### Update through command line

You can update your application by running the following command: 
        
        $ php bin/console app:update

The command will perform the following actions:

- Back up current project files, 
- Back up database (schema & data),
- Update project files with the latest source,
- Update dependencies,
- Update database schema.

Note: Update through command line requires that the "Github token" setting has been set. Please refer to [this section](#dashboard-settings-update-settings) to set your Github token.

### Manual update

#### 1. Make a backup

Make sure you have a backup before updating the full project. You may copy the full project directory if you're not sure. For example, if your project directory is named `hipay_mirakl`:

	$ cp -R hipay_mirakl hipay_mirakl_backup

#### 2. Check if Git is initialized

Go to the root directory of the project (where `composer.json` is) and check if there is a `.git` directory by running the following command:

	ls -al .git
	
The output should look like:

````
drwxr-xr-x  8 root root 4096 May  4 08:16 .
drwxr-xr-x 10 root root 4096 May  4 08:13 ..
-rw-r--r--  1 root root   41 May  4 08:13 HEAD
drwxr-xr-x  2 root root 4096 May  4 08:13 branches
-rw-r--r--  1 root root  227 May  4 08:16 config
-rw-r--r--  1 root root   73 May  4 08:13 description
drwxr-xr-x  2 root root 4096 May  4 08:13 hooks
...
````

**If you get an output like this, go to the next section ("Update the project").**

If you get an error message like `ls: cannot access .git: No such file or directory`, run the following commands to initialize Git:

	git init
	git remote add origin https://github.com/hipay/hipay-wallet-cashout-mirakl-integration

#### 3. Update the project

First, fetch the new tags available:

	$ git fetch

Then, determine the version number to which you want to upgrade. Check out the [releases page](https://github.com/hipay/hipay-wallet-cashout-mirakl-integration/releases) for more information.

**When upgrading to a major version (example: from v1.x.x to v2.x.x), make sure that you know the upgrading details.** Do not hesitate to contact HiPay's Business IT Services on our [Support Center](https://support.hipay.com) if you need more information. You can check the version of your installation by typing `cat composer.json | grep version`. You should get an output similar to: 
> "version": "2.0.3".

When you have determined the version number to which you want to upgrade, run the following command, **replacing xxx by the version number**:

	$ git checkout tags/xxx --force

For example, if you want to update to version 2.1.0, you will have `tags/2.1.0`.

#### 4. Install the dependencies

Install the dependencies using Composer:

	$ composer install
	
If new parameters were added to the project, you will be asked to provide values for them.

#### 5. Update the database

Go to the project directory:

	$ cd hipay_mirakl

Run the following command:

	$ php bin/console orm:schema-tool:update --dump-sql --force

#### 6. Change permissions

Run the following commands:

	$ chmod 777 /var/log/hipay.log

	$ chmod 777 -R var/

#### 7. Recover vendor logs (optional)

If there are existing vendors in the database but no logs linked to them, you can generate logs by running the following command:
    
        $ php bin/console logs:vendors:recover

You may check if the upgrade was successful by trying a simple command, for example: 

	$ php bin/console vendor:process "7 days ago"
