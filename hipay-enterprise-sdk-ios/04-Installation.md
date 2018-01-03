# Installation

Before starting the installation, please read all instructions and make sure you've gone through the [Prerequisites and recommendations](#prerequisites-and-recommendations) section. 

You have to use CocoaPods to install the HiPay Enterprise SDK for iOS.

## Using CocoaPods

Add this line to your project's `Podfile`:

	pod 'HiPayFullservice'
	
Optionally, if you want the SDK to calculate the Device Fingerprint, add this line as well:

	pod 'HiPayFullservice/Device-Print'

Then, run the following command in the same directory as your `Podfile`:

	$ pod install

This will install the core wrapper components as well as the built-in payment screen and other utility components. 

### Customizing the CocoaPods installation (advanced)
You can customize the installation if you don't need all the components. For example, if you don't need the built-in payment screen and just want to install the core wrapper, you may only add the following line to your `Podfile`:

	pod 'HiPayFullservice/Core'

### Using a physical payment terminal

The SDK can be used for physical payments on a mobile point of sale (mPOS) payment terminal (such as the Datecs' BluePad-500).

If you want to use the SDK in a mobile app installed on an iOS device plugged into a BluePad-500 payment terminal, then you have to customize the installation. 

The underlying framework allowing the BluePad-500 payment terminal to process payments is not included by default. 
In order to use the SDK for physical payments through such a terminal, add these two lines into the Podfile (instead of the default installation line `pod 'HiPayFullservice'`):

	pod 'HiPayFullservice/Core'
	pod 'HiPayFullservice/Datecs-POS'

Note that **Git LFS** is needed to install the Datecs-POS pod.
[https://help.github.com/articles/installing-git-large-file-storage/](https://help.github.com/articles/installing-git-large-file-storage/)