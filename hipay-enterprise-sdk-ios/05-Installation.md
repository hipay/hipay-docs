# Installation

Before starting the installation, please read all instructions and make sure you've gone through the [Prerequisites and recommendations](#prerequisites-and-recommendations) section.

You have to use [CocoaPods](https://cocoapods.org/) to install the HiPay Enterprise SDK for iOS.

## Using CocoaPods

Add this line to your project's `Podfile`:

	pod 'HiPayFullservice'

Optionally, if you want the SDK to calculate the [Device Fingerprint](https://en.wikipedia.org/wiki/Device_fingerprint) (useful for fraud), add this line as well:

	pod 'HiPayFullservice/Device-Print'

Then, run the following command in the same directory as your `Podfile`:

	$ pod install

This will install the core wrapper components as well as the built-in payment screen and other utility components.

### Using advanced integration

You can customize the installation if you don't need all the components. For example, if you don't need the built-in payment screen and just want to install the core wrapper, you may only add the following line to your `Podfile`:

	pod 'HiPayFullservice/Core'

Please refer to the [Advanced integration](#usage-making-payments-advanced-integration) section.

### Using a physical payment terminal

The SDK can be used for physical payments on a mobile point of sale (mPOS) payment terminal (such as the Datecs' BluePad-500).

If you want to use the SDK in a mobile app installed on an iOS device plugged into a BluePad-500 payment terminal, then you have to customize the installation.

The underlying framework allowing the BluePad-500 payment terminal to process payments is not included by default.
In order to use the SDK for physical payments through such a terminal, add these two lines into the Podfile (instead of the default installation line `pod 'HiPayFullservice'`):

	pod 'HiPayFullservice/Core'
	pod 'HiPayFullservice/Datecs-POS'

Note that [Git LFS](https://help.github.com/articles/installing-git-large-file-storage/) is needed to install the Datecs-POS pod.

# PSD2 and Strong Customer Authentication

Given the strong growth of the e-commerce in Europe, the Payment Services Directive (PSD2) redefines the security standards for online payments aiming to increase the security during the payment process, while fighting more actively against fraud attempts. For more details on the regulations, we invite you to read the [documentation provided by Hipay](https://developer.hipay.com/psd2-and-strong-customer-authentication-3-d-secure-2-compliance-and-guidance/)
 
As of September 14, 2019, the issuer will decide if a payment is processed depending on the analysis of more than 150 data collected during the purchasing process. Thanks to our Android SDK we handle most of the data without you having to code anything. You can see all the new parameters on [our explorer API](https://developer.hipay.com/doc-api/enterprise/gateway/#/payments/requestNewOrder).

 
## Adding or overriding PSD2 data
 
The accuracy of the information sent is key for making sure that your customers have a frictionless payment process. That’s why we provide you the possibility to add or override all the information related to the DSP2.

We added 3 new variables in `HPFPaymentPageRequest` object :

- `merchantRiskStatement`
- `previousAuthInfo`
- `accountInfo`

Each variables is a string type corresponding to a JSON Object. Your server send user informations to your Android application, so you just have to retrieve these JSON data and convert them in string format and set the specific variable.
If you don’t set `accountInfo` variable, we automatically generate the associated NSDictionary with 3 values (`name_indicator` / `enrollment_date` / `card_stored_24h`).