# Build settings

The iOS target version is 7.0, which means that the SDK won't build on applications targeting a lower version of iOS.

# Credentials

You need to generate *API credentials* in order for the SDK to send requests to the HiPay Fullservice platform. To do so, go in the "Integration" section of the HiPay Fullservice back office and then go to "Security Settings".

# Server integration

You don't need a server integration in order for the SDK to work properly. However, you will eventually need to process orders server-side. When a transaction is successfully completed client-side, on the SDK, it will eventually result in a HTTP noticiation call to your server, performed by HiPay Fullservice platform. Order confirmation should be processed upon HiPay HTTP server-to-server notification.

# Next step
When you're done with this part, go to the next section: [[Installation]]