# Testing the demo app

## Preamble

The HiPay Enterprise SDK for Android comes with a demo application which allows you to quickly test the built-in payment screen integration.

This demo application presents a screen that allows you to generate a payment screen based on many parameters such as: amount, payment product categories, etc.

This particular screen is not part of the SDK and your users will not see it. It is part of the demo application and has been created for testing purposes only.

## Installation guide

### Cloning the repository

First, you need to check out the repository. Go to your projects folder and type the following command:

	$ git clone https://github.com/hipay/hipay-fullservice-sdk-android.git

Then, import the `hipay-fullservice-sdk-android` project folder into Android Studio/Eclipse to open the demo project.

### Adding your credentials

Open the project, then try the demo application with your HiPay Enterprise API test credentials: open the `/example/src/main/res/values/credentials.xml` file and set your API username and password. 

![Setting API credentials for the demo app](images/demo/credentials.png)

### Setting the proper passphrase

The demo app uses a HiPay test web service which generates the signature based on a specific passphrase. Thus, in order to test the demo app, you must set the same specific passphrase on your HiPay Enterprise test account.

To set the passphrase, please sign in to your HiPay Enterprise back office. Then, go to the "Integration" section and click on "Security Settings". Finally, set the following string in the "Secret Passphrase" input field: 

	32JUWB3veDWWmHySNJvtvPyBnqrDFEHbaP3jr

**Warning: do NOT set this passphrase on your live/production account. This specific passphrase must be used for testing purposes only.**

You should get that on your screen:

![](images/demo/passphrase.png)

Don't forget to apply your changes.  


## Running the demo app

Finally, build and run the `example` module.
