Before starting the installation, please read all instructions and make sure you've gone through the [[Prerequisite and recommendations]] section. 

It's recommended to use CocoaPods to install the HiPay Fullservice SDK for iOS.

# Using CocoaPods (recommended)

Add this line to your project's `Podfile`:

	pod 'HiPayFullservice'

Then, run the following command in the same directory as your `Podfile`:

	$ pod install

This will install the core wrapper components as well as the built-in payment screen and other utility components. 

## Customizing the CocoaPods installation (advanced)
You can customize the installation if you don't need all the components. For example, if you don't need the built-in the payment screen and just want to install the core wrapper, you may lighten this installation by only adding the following line to your `Podfile`:

	pod 'HiPayFullservice/Core'

# Manual installation (advanced)

TODO

# Next step
Once you app is properly installed, go to the next section: [[Usage]]