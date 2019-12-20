# Platform configuration

Before using the HiPay Enterprise module for Magento 2, the following settings are required and must be configured.
To do so, log in to your [HiPay Enterprise back office](https://merchant.hipay-tpp.com).

## Configure your secret passphrase

A secret passphrase is required to allow notifications.  
In your HiPay Enterprise back office, click on the “_Integration_” tab, then on “_Security Settings_”. Enter your secret passphrase in the “_Data Verification_” section.

![legend](images/secret_passphrase.png)

## Allow your servers’ IP address(es)

When a request is sent to the HiPay Enterprise servers, the IP address or IP address range from where the connection was made is verified. If it matches with the IP address previously provided by the merchant, the request will be processed. In case of missing or incorrect information, the server will respond with an appropriate error message, indicating the error in the request.

In your HiPay Enterprise back office, click on the “_Integration_” tab, then on “_Security Settings_” and enter your IP address(es) in the “_IP Restriction_” section.

> **Important!**
>
> When changing your IP addresses, make sure that all the new ones are configured for your account. If not, your server requests will be rejected.


## Configure redirect URLs

To use the HiPay Enterprise module for Magento 2, you **need to configure** default redirect URLs in your HiPay Enterprise back office in “_Integration_” -> “_Redirect Pages_”.   
Yet, **these are not used** by the HiPay Enterprise module for Magento 2 as it sends its own URLs.

- Accept Page:    `http://notused.com/accept`
- Decline Page:   `http://notused.com/decline`
- Pending Page:   `http://notused.com/pending`
- Cancel Page:    `http://notused.com/cancel`
- Exception Page: `Empty`

Also, check the “_Feedback Parameters_” box and apply changes.

![legend](images/redirect_pages.png)

## Configure the notification URL

In your HiPay Enterprise back office, click on the “_Integration_” tab, then on “_Notifications_”.

- Notification URL:    http://www.{your-domain.com}/hipay/notify/index
- Request method:      HTTP POST
- I want to be notified for the following transaction statuses: ALL

Don’t forget to replace {your-domain.com} with your own domain.

![legend](images/notification_url.png)

## Customize notifications

If you choose to customize the fields sent in the notifications, please be aware that the *“operation”* field is mandatory. Refunds depend on it.

