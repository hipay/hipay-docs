![](images/media/image1.jpg){width="8.286897419072616in"
height="4.4375in"}![](images/media/image2.png){width="3.5388593613298336in"
height="0.4847222222222222in"}![](images/media/image3.png){width="8.334722222222222in"
height="1.9951388888888888in"}![](images/media/image2.png){width="3.5388593613298336in"
height="0.4847222222222222in"}

Settlement server-to-server notifications 3

What is a server-to-server notification? 3

Setup 3

Configuration parameters 4

Response fields 4

Examples 5

<span id="_Toc453598093" class="anchor"></span>\
<span id="_Toc453601293" class="anchor"><span id="_Toc453696830"
class="anchor"></span></span>Settlement server-to-server notifications

<span id="_Toc243383271" class="anchor"><span id="_Toc453601294"
class="anchor"></span></span>

<span id="_Toc453696831" class="anchor"></span>What is a
server-to-server notification?

  Description   In order to notify financial events related to your payment system, such as a new settlement file created, the HiPay platform can send to your application a server-to-server notification.
  ------------- ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

<span id="_Toc243383272" class="anchor"><span id="_Toc453601295"
class="anchor"><span id="_Toc453696832"
class="anchor"></span></span></span>Setup

  Procedure              To set up your Financial Feedback Notification URL, you must login into your HiPay Fullservice back office and go to *“Integration -&gt; Notifications -&gt; Financial Feedback*”.
  ---------------------- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Configuration Screen   ![](images/media/image4.jpg){width="3.383293963254593in" height="4.779024496937883in"}

<span id="_Toc243383273" class="anchor"><span id="_Toc453601296"
class="anchor"></span></span>

<span id="_Toc453696833" class="anchor"></span>Configuration parameters

  -----------------------------------------------------------------------------------------------------------------------------------
  Field name              Description
  ----------------------- -----------------------------------------------------------------------------------------------------------
  Notification URL        The URL or IP on which you want to receive financial server-to-server notifications.

  Request method          The method used to send you the requests:
                          
                          -   XML
                          
                          -   JSON
                          
                          -   HTTP POST
                          
                          

  Hash algorithm          The algorithm used to hash the password that will sign all sent notifications.

  Password                The password used to generate a unique character string (signature) hashed with the defined algorithm.
                          
                          The security level of the password depends on the length of the password. A long password is more secure.

  Desired notifications   Financial notifications you want to receive.
  -----------------------------------------------------------------------------------------------------------------------------------

<span id="_Toc243383274" class="anchor"><span id="_Toc453601297"
class="anchor"></span></span>

<span id="_Toc453696834" class="anchor"></span>Response fields

The following table lists and describes the response fields.

  -------------------------------------------------
  **Field name**           **Description**
  ------------------------ ------------------------
  notification\_type\      Type of notification
  (only in POST request)   

  account                  Account ID

  reference                Settlement ID

  sales                    Sales amount

  refunds                  Refunds amount

  fees                     Fees amount

  chargeback               Chargebacks amount

  rolling                  Rolling reserve amount

  other                    Other amount

  amount                   Settlement amount

  currency                 Settlement currency
  -------------------------------------------------

<span id="_Toc453601298" class="anchor"><span id="_Toc453696835"
class="anchor"></span></span>Examples

The following examples are XML, JSON and HTTP Post responses.

  --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Response in XML Format         &lt;?xml version="1.0" encoding="UTF-8"?&gt;
                                 
                                 &lt;settlement&gt;
                                 
                                 &lt;account&gt;987654&lt;/account&gt;
                                 
                                 &lt;reference&gt;123456&lt;/reference&gt;
                                 
                                 &lt;sales&gt;2839.000&lt;/sales&gt;
                                 
                                 &lt;refunds&gt;0&lt;/refunds&gt;
                                 
                                 &lt;fees&gt;90.040&lt;/fees&gt;
                                 
                                 &lt;chargeback&gt;0&lt;/chargeback&gt;
                                 
                                 &lt;deferred&gt;0&lt;/deferred&gt;
                                 
                                 &lt;rolling&gt;0&lt;/rolling&gt;
                                 
                                 &lt;other&gt;0&lt;/other&gt;
                                 
                                 &lt;amount&gt;2748.040&lt;/amount&gt;
                                 
                                 &lt;currency&gt;&lt;!\[CDATA\[EUR\]\]&gt;&lt;/currency&gt;
                                 
                                 &lt;/settlement&gt;
  ------------------------------ -------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Response in JSON Format        {"settlement":{"account":"987654","reference":"123456","sales":2839,"refunds":0,"fees":90.040,"chargeback":0,"rolling":0,"other":0,"amount":2748.040,"currency":"EUR"}}

  Response in HTTP POST Format   notification\_type=settlement&account=987654&reference=123456&sales=2839&refunds=0&fees=90&chargeback=0&rolling=0&other=2748.040&amount=0.000&currency=EUR
  --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


