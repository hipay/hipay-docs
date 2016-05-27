## PrestaShop module management

### Module management in the back office

#### Generate a ZIP from GitHub

Download the latest release in the release section: https://github.com/hipay/hipay-fullservice-sdk-prestashop/releases.

1. Open your ZIP and extract the project on your desktop.
2. Go to the "_src_" folder in the project root folder.
3. Create a ZIP file entitled "_hipay tpp_".
4. Done! You now have a "_hipay_tpp.zip_" ZIP file.

OR

Download the ZIP package available in the project **hipay/hipay-fullservice-sdk-prestashop** "_package-ready-for-prestashop/hipay_tpp_1-X-X.zip_".

#### Upload the module via PrestaShop module management

To install it in your PrestaShop administrator back office, click on "_Modules -> Modules -> Add a new module_".

Choose the package and click on "_Upload this module_".

[[images/upload-module.png]]

### Upload the module via FTP (SFTP)

You must have a file transfer software like "_FileZilla_" for example.

1. Open your software and connect to your FTP (SFTP).
2. Go to the root of your PrestaShop project.
3. Transfer the "_hipay tpp_" source module in the "_/modules/_" folder.
4. Add write permissions recursively to the folder "hipay_tpp" (766).

[[images/filezilla.png]]

### Install the module in the PrestaShop back office 

Once the module is successfully downloaded:

1. Go to "_Modules -> Modules_". 
2. Search for the "_HiPay Fullservice_" module.
3. Click on “Install” in the module description.

[[images/list-module-hipay-1.png]]

When the installation request is executed, a window shows up. It's simply a PrestaShop verification warning for the module. Click on "Proceed with the installation":

[[images/list-module-hipay-2.png]]

### Installation with Docker for testing

If you are a developer or a QA developer, you can use this project with Docker and Docker Compose. 

Requirements for your environment:

- Git (https://git-scm.com/)
- Docker (https://docs.docker.com/engine/installation/)
- Docker Compose (https://docs.docker.com/compose/)

Here is the procedure to be applied to a Linux environment:

- Open a terminal and select the folder of your choice. 
- Clone the HiPay Fullservice PrestaShop project in your environment with Git:

```
$ git clone https://github.com/hipay/hipay-fullservice-sdk-prestashop.git
```

- Go in the project root folder and enter this command:

```
$ docker-compose up -d
```

- Your container is loading: wait for a few seconds while Docker installs PrestaShop and the HiPay module.*
- You can now test the HiPay Fullservice module in a browser with this URL:

```
 http://localhost:8085/
```

- To connect to the back office, go to this URL:

```
 http://localhost:8085/admin-hipay/
```

- The login and password are demo@hipay.com / hipay123.
- You can test the module with your account configuration.


### Next step
When you're done with this part, go to the next section: [[Module configuration]].