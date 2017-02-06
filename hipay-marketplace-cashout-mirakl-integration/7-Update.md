# Update

This section is useful if you already have the HiPay Marketplace cash-out integration for Mirakl installed on your server and want to update it.

Remember: the project HiPay Marketplace cash-out **integration** for Mirakl relies on a ***core library*** which is named [HiPay Marketplace cash-out **library** for Mirakl](https://github.com/hipay/hipay-wallet-cashout-mirakl-library). You can either update the full project or the core library only. In most cases, you will only need to update the core library. Update the full project only if new versions contain features you need or if the HiPay's Business IT Services recommended you to do it.

Both procedures are documented below:

## Core library update procedure (common use case)

Go in the root directory of the project (where `composer.json` is) and type the following command:

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

### 1. Backup

Make sure you have a backup before upgrading the full project. You may copy the full project directory if you're not sure. For example, if your project directory is named `hipay_mirakl`:

	$ cp -R hipay_mirakl hipay_mirakl_backup

### 2. Check if Git is initialized

Go in the root directory of the project (where `composer.json` is) and check if there is a `.git` directory by typing the following command:

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

**If you get an output like this, then go to the next part ("Update the project").**

If you get an error message like `ls: cannot access .git: No such file or directory`, then type the following commands to init Git:

	git init
	git remote add origin https://github.com/hipay/hipay-wallet-cashout-mirakl-integration

### 3. Update the project

First, fetch the new tags available:

	$ git fetch

Then, figure out the version number to which you want to upgrade. Check out the [releases page](https://github.com/hipay/hipay-wallet-cashout-mirakl-integration/releases) for more information. 

**When upgrading to a major version (example: from v1.x.x to v2.x.x), make sure that you know the upgrading details.** Do not hesitate to contact HiPay's Business IT Services on our [Support Center](http://help.hipay.com/) if you need details. You can check the version of your installation by typing `cat composer.json | grep version`. You should get an output similar to: 
> "version": "2.0.3",

When you figured out the version number to which you want to upgrade, type the following command, **replacing xxx by the version number**:

	$ git checkout tags/xxx --force

For example, if you want to update to version 2.1.0, you will have `tags/2.1.0`.

### 4. Install the dependencies

Install the dependencies using Composer:

	$ composer install
	
If new parameters were added into the project, you will be asked to provide values for them.

You may check if the upgrade was successful by trying a simple command, for example: 

	$ php bin/console vendor:process "7 days ago"

### 5. Update the database

Finally, go in the project directory: 

	$ cd hipay_mirakl

Run the following command:

	$ php bin/console orm:schema-tool:update --dump-sql --force
