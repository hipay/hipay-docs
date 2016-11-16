# Testing the demo app

## Preamble

The HiPay Fullservice SDK for iOS comes with a demo application which allows you to quickly test the built-in payment screen integration.

This demo application presents a screen that allows you to generate a payment screen based on many parameters such as: amount, payment product categories, etc.

This particular screen is not part of the SDK and your users will not see it. It is part of the demo application and has been created for testing purposes only.

## Installation guide

### Cloning the repository

First, you need to check out the repository. Go to your projects folder and type the following command:

	$ git clone https://github.com/hipay/hipay-fullservice-sdk-ios.git

Then, go to the project directory:

	$ cd hipay-fullservice-sdk-ios

### CocoaPods

This project has been built using [CocoaPods][cocoapods], the most popular package manager for Cocoa projects. If you have already installed CocoaPods on your workstation, you can skip this section.

If not, install it by typing the following command:

	$ sudo gem install cocoapods

### Setting up the project

First, go to the demo app directory:

	$ cd Example

To generate the project workspace, run the following command:

	$ pod install

### Opening the project

Open the `HiPayFullservice.xcworkspace` workspace by typing the following command:

	$ open HiPayFullservice.xcworkspace

### Adding your credentials

To try the demo application with your HiPay Fullservice API test credentials, open the `parameters.plist` file and set your API username and password, as follows:

![Setting API credentials for the demo app](images/demo/credentials.png)

To find your credentials, please refer to the [Prerequisites and recommendations](#prerequisites-and-recommendations) section.

### Setting the proper passphrase

The demo app uses a HiPay test web service which generates the signature based on a specific passphrase. Thus, in order to test the demo app, you must set the same specific passphrase on your HiPay Fullservice test account.

To set the passphrase, please sign in to your HiPay Fullservice back office. Then, go to the "Integration" section and click on "Security Settings". Finally, set the following string in the "Secret Passphrase" input field: 

	32JUWB3veDWWmHySNJvtvPyBnqrDFEHbaP3jr

**Warning: do NOT set this passphrase on your live/production account. This specific passphrase must be used for testing purposes only.**

You should get that on your screen:

![](images/demo/passphrase.png)

Don't forget to apply your changes.  

### Running the demo app

Finally, build and run the `HiPayFullservice-Example` target.

[repo]: https://github.com/hipay/hipay-fullservice-sdk-ios
[cocoapods]: https://cocoapods.org/