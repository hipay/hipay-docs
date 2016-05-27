# Preamble

The HiPay Fullservice SDK for iOS is shipped with a demo application which allows you to quickly test the built-in payment screen integration.

This demo application presents a screen that allows you to generate a payment screen based on many parameters such as: amount, payment product categories, etc.

This particular screen in not part of the SDK and your users will not see it. It's part of the demo application and has been created for testing purposes only.

# Installation guide

## Clone the repository

First, you need to check out the repository. Go in your projects folder and type the following command:

	$ git clone https://github.com/hipay/hipay-fullservice-sdk-ios.git

Then, go in the project directory:

	$ cd hipay-fullservice-sdk-ios

## CocoaPods

This project is built using [CocoaPods][cocoapods], the most popular package manager for Cocoa projects. If you've already installed CocoaPods on your workstation, skip this section.

If you did not install CocoaPods yet, install it by typing the following command:

	$ sudo gem install cocoapods

## Set up the project

First, go in the demo app directory:

	$ cd Example

To generate the project workspace, run the following command:

	$ pod install

## Open the project

Open the `HiPayFullservice.xcworkspace` workspace by typing the following command:

	$ open HiPayFullservice.xcworkspace

## Add your credentials

To try the demo application with your HiPay Fullservice API test credentials, open the `parameters.plist` file and set your API username and password, as below:

![Setting API credentials for the demo app](images/demo/credentials.png)

## Run the demo app

Finally, build and run the `HiPayFullservice-Example` target.

[repo]: https://github.com/hipay/hipay-fullservice-sdk-ios
[cocoapods]: https://cocoapods.org/