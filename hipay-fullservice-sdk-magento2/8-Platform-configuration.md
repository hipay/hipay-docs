
## Allow the IP addresses of your servers

When a request is sent to the HiPay Fullservice servers, the IP address or IP address range from where the connection was made is verified. If it matches with the IP address previously provided by the merchant, the request will be processed. In case of missing or incorrect information, the server will respond with an appropriate error message, indicating the error in the request.

To do this, you must log in your HiPay Fullservice back office (https://merchant.hipay-tpp.com), click on the "_Integration_" tab, then on "_Security Settings_" and enter your IP address(es) in the "_IP Restriction_" section.

> **Important!**
>
> When changing your IP addresses, make sure that all the new ones are configured for your account. If not, your server requests will be rejected.

[[images/ip-restriction.png]]

## Configure redirect URLs

To use the HiPay Fullservice module, you need to configure the redirect URLs in your HiPay Fullservice back office under "_Integration -> Redirect Pages_".

- Accept Page:    http://www.{your-domain.com}/accept
- Decline Page:   http://www.{your-domain.com}/decline
- Pending Page:   http://www.{your-domain.com}/pending
- Cancel Page:    http://www.{your-domain.com}/cancel
- Exception Page: Empty

Also, check the "_Feedback Parameters_" box and apply the changes.

Click on the "_Integration_" tab, then on "_Notifications_"

- Notification URL:    http://www.{your-domain.com}/hipay/notify/index
- Request method:      HTTP POST
- I want to be notified for the following transaction statuses: ALL

Don’t forget to replace {your-domain.com} by your own domain.


### Next step
When you're done with this part, go to the next section: [[Module installation]].