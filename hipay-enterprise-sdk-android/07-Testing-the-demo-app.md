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

### Adding the test credentials

Open the project, then try the demo application with your HiPay Enterprise API test credentials: open the `/example/src/main/res/values/credentials.xml` file and set these API username and password :

`94666899.stage-secure-gateway.hipay-tpp.com`  
`Test_6j6SVHimlA5o0NIBdBQH6SVm`

![Setting API credentials for the demo app](images/credentials.png)


## Running the demo app

Finally, build and run the `example` module.
