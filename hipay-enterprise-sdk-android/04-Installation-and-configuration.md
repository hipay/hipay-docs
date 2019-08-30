# Installation

Before starting the installation, please read all instructions and make sure you've gone through the [Prerequisites and recommendations](#prerequisites-and-recommendations) section.

It's recommended to use *Gradle* to install the HiPay Enterprise SDK for Android.

## Using Gradle (recommended)

Add this line to your project's `build.gradle`:

	dependencies { 
		compile 'com.hipay.fullservice:hipayfullservice:1+'
	}

Then synchronize the project with Gradle files. This will install the core wrapper components as well as the built-in payment screen and other utility components.

# PSD2 and Strong Customer Authentication

Given the strong growth of the e-commerce in Europe, the Payment Services Directive (PSD2) redefines the security standards for online payments aiming to increase the security during the payment process, while fighting more actively against fraud attempts. For more details on the regulations, we invite you to read the [documentation provided by Hipay](https://developer.hipay.com/psd2-and-strong-customer-authentication-3-d-secure-2-compliance-and-guidance/)
 
As of September 14, 2019, the issuer will decide if a payment is processed depending on the analysis of more than 150 data collected during the purchasing process. Thanks to our Android SDK we handle most of the data without you having to code anything. You can see all the new parameters on [our explorer API](https://developer.hipay.com/doc-api/enterprise/gateway/#/payments/requestNewOrder).

 
## Adding or overriding PSD2 data
 
The accuracy of the information sent is key for making sure that your customers have a frictionless payment process. That’s why we provide you the possibility to add or override all the information related to the DSP2.

We added 3 new variables in `PaymentPageRequest` object :

- `merchant_risk_statement`
- `previous_auth_info`
- `account_info`

Each variables is a string type corresponding to a JSON Object. Your server send user informations to your Android application, so you just have to retrieve these JSON data and convert them in string format and set the specific variable.
If you don’t set `account_info` variable, we automatically generate it with 3 fields (`name_indicator` / `enrollment_date` / `card_stored_24h`) inside for you because they are required.

# Configuration

You need to provide the SDK with a few parameters, such as the credentials and targeted environment.

## Credentials

Get a valid HiPay Enterprise API username and password. If you don't have any, please refer to the [Prerequisites and recommendations](#prerequisites-and-recommendations) section.

## Setting up the configuration

The following code allows you to configure the SDK. We recommend putting it in your *App Launcher Activity*'s `onCreate()` method implementation.

```Java
public class DemoActivity extends AppCompatActivity {
     ...

     protected void onCreate(Bundle savedInstanceState) {
         super.onCreate(savedInstanceState);
         
         ClientConfig.getInstance().setConfig(
         
                ClientConfig.Environment.Stage,
                "YOUR API USERNAME",
                "YOUR API PASSWORD"
        );
     }

     //optional features

     ClientConfig.getInstance().setPaymentCardStorageEnabled(true);

     ClientConfig.getInstance().setPaymentCardScanEnabled(true);

     ClientConfig.getInstance().setPaymentCardNfcScanEnabled(true);
}

```

Do not forget to **replace the username and password arguments with your API username and password**.

The Payment card storage option is also called "One-click payment".
You can find more information in the [Card storage feature](#usage-making-payments-core-wrapper-advanced-integration-card-storage-feature) page. 

The **camera scan** and **NFC scan** options help create a more fluid mobile payment process.    

 
Once your app goes live, you need to set the environment to `Environment.Production`.
