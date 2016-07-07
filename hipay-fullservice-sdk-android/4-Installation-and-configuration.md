Before starting the installation, please read all instructions and make sure you've gone through the [[Prerequisites and recommendations]] section. 

It's recommended to use *Gradle* to install the HiPay Fullservice SDK for Android.

# Installation using Gradle (recommended)

Add this line to your project's `build.gradle`:

	dependencies { 
		compile 'com.hipay.hipayfullservice.api:hipayfullservice:1+'
	}

And then synchronize the project with Gradle files. This will install the core wrapper components as well as the built-in payment screen and other utility components. 

# Configure the SDK

Then, you need to provide the SDK with a few parameters, such as credentials and targeted environement.

## Credentials

Get a valid HiPay Fullservice API username and password. If you don't have them, refer to the [[Prerequisites and recommendations]] page.

## Set up the configuration

The following code allows you to configure the SDK. We recommend you to put it in your *App Launcher Activity*'s `onCreate()` method implementation.

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
 }

```

Do not forget to **replace the username and password arguments with your API username and password**.

Once your app goes live, you need to set the environment to `Environment.Production`.

# Next step
Once your app is properly installed, go to the next section: [[Usage]]
