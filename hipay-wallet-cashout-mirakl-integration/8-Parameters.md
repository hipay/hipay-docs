The following table describes the data in `parameters.yml`. The user you run the commands with should have write access to all the paths you use in this file.

# Parameter categories

## Mirakl web service

| Name | Type | Default value | Description
|------|:----:|:-------------:|------------
| `mirakl.frontKey`     	    |  String       	                    |               	                                    | Mirakl API Front key (provided by Mirakl)
| `mirakl.operatorKey` 	        | String  	                    |               	                                    | Mirakl API Operator key (provided by Mirakl)

## Mirakl cycles

| Name | Type | Default value | Description
|------|:----:|:-------------:|------------
| `cycle.days`                  | Array 	                    |[1, 11, 21]                                          | Array of days (1 to 28) on which the cycle is run in the month
| `cycle.hour`                  | Integer	                    |0               	                                    | Hour on which the cycle is run
| `cycle.minute`                | Integer	                    |0               	                                    | Minute on which the cycle is run
| `cycle.interval.before`       | String	                          	  |12 hours               	                            | Time to subtract from the cycle time to form an interval from which to fetch the transactions from Mirakl
| `cycle.interval.after`        | String	                          	  |12 hours               	                            | Time to subtract from the cycle time to form an interval from which to fetch the transactions from Mirakl

## HiPay 

| Name | Type | Default value | Description
|------|:----:|:-------------:|------------
| `hipay.wsLogin`               | String	                    |               	                                    | HiPay web service login key for the technical account
| `hipay.wsPassword`            | String	                    |               	                                    | HiPay web service password key for the technical account
| `hipay.baseUrl`               | String	                    |               	                                    | Base URL to HiPay web service
| `hipay.entity`                | String	                    |               	                                    | Provided by HiPay
| `hipay.locale`                | String	                    |fr_FR               	                                | Locale of created accounts. Format should be: languageCode_ISO3166-1 alpha-2 country code
| `hipay.timezone`              | String	                    |Europe/Paris               	                        | Time zone of created accounts. Please see [the php manual](http://php.net/manual/en/timezones.php) for a list of time zones
| `hipay.merchantGroupId`       | Integer	                              |               	                                    | Provided by HiPay


## HiPay accounts


| Name | Type | Default value | Description
|------|:----:|:-------------:|------------
| `account.technical.email`     | String	                    |               	                                    | Email of the HiPay Wallet technical account
| `account.technical.hipayId`   | Integer	                    |               	                                    | ID of the HiPay Wallet technical account
| `account.operator.email`      | String	                    |               	                                    | Email of the HiPay Wallet operator's account
| `account.operator.hipayId`    | Integer	                    |               	                                    | ID of the HiPay Wallet operator's account

## HiPay labels

| Name | Type | Default value | Description
|------|:----:|:-------------:|------------
| `label.public`                | String	                    |Public {{miraklId}} – {{hipayId}} - {{cycleDate}}    | Public label for HiPay transfers. Please see hereafter for more details.
| `label.private`               | String	                    |Private {{miraklId}} – {{hipayId}} - {{cycleDate}}   | Private label for HiPay transfers. Please see hereafter for more details.
| `label.withdraw`              | String	                    |Withdraw {{miraklId}} – {{hipayId}} - {{cycleDate}}  | Withdrawal label for HiPay withdrawals. Please see hereafter for more details.

## Database

| Name | Type | Default value | Description
|------|:----:|:-------------:|------------
| `db.driver`                   | String	                    |pdo_mysql               	                            | PHP PDO driver. Please refer to [the php manual](http://php.net/manual/en/pdo.drivers.php) for a list of PDO drivers
| `db.host`                     | String	                    |               	                                    | DB hostname
| `db.username`                 | String	                    |               	                                    | DB username
| `db.password`                 | String	                    |               	                                    | DB password
| `db.name`                     | String	                    |               	                                    | DB name
| `db.port`                     | Integer	                    |               	                                    | DB port

## Email

| Name | Type | Default value | Description
|------|:----:|:-------------:|------------
| `mail.host`                   | String	                    |               	                                    | SMTP server hostname
| `mail.port`                   | Integer	                    |               	                                    | SMTP server port
| `mail.security`               | String	                    |               	                                    | SMTP server security. See [below](#e-mail-security) for more information. 
| `mail.username`               | String	                    |               	                                    | SMTP server username
| `mail.password`               | String	                    |               	                                    | SMTP server password
| `mail.subject`                | String	                    |               	                                    | Mail subject
| `mail.to`                     | String or array of strings	    |               	                                    | Mail recipients for various notifications (notably error notifications)
| `mail.from`                   | String or array of strings	    |               	                                    | Mail senders

### E-mail security

The e-mail security parameter can be `null` or another transport wrapper. As pointed out in the [Swift Mailer documentation](http://swiftmailer.org/docs/sending.html#encrypted-smtp), your PHP installation must have appropriate OpenSSL transports wrappers in order to use values such as `tls` or `ssl`. In order to check if TLS or SSL are available, run this command:

       $ php -r "var_export(stream_get_transports());"

You should get something like this:

```
root@ab19320254c7:/var/www/html# php -r "var_export(stream_get_transports());"
array (
  0 => 'tcp',
  1 => 'udp',
  2 => 'unix',
  3 => 'udg',
  4 => 'ssl',
  5 => 'sslv3',
  6 => 'tls',
)
````

You can either provide the `tls` or `ssl` values for the `mail.security` parameter if they are present in the list.

## Miscellaneous

| Name | Type | Default value | Description
|------|:----:|:-------------:|------------
| `debug`                       | Boolean	                    |false               	                                | Activates Debug mode
| `default.tmp.path`            | String	                    | /tmp              	                | Default path to save the downloaded zip file
| `log.file.path`               | String	                    |/var/log/hipay.log               	                | Path to the log file
| `cashout.transactionFilterRegex`               | String	                    | null | Used to filter cash-out transactions. Let it `null` if you don't know what to do with it.

# Labels

Labels give a description of the transaction (transfer or withdrawal) and appear on the account statement.
For label parameters (label.public, label.private, label.withdraw), the following placeholder strings can be used, which will be replaced before sending the data to HiPay.
Each string must be surrounded by {{ and }} to be replaced.

| Placeholder     | Description                                                             |
|-----------------|-------------------------------------------------------------------------|
| `miraklId`      | Shop ID if it exists, or operator ID in case of an operator’s operation |
| `amount`        | Amount of the operation                                                 |
| `hipayId`       | HiPay Wallet account ID                                                 |
| `cycleDate`     | Cycle date of the operation                                             |
| `cycleDateTime` | Cycle date and time of the operation                                    |
| `cycleTime`     | Cycle time of the operation                                             |
| `date`          | Execution date of the operation                                         |
| `dateTime`      | Execution date and time of the operation                                |
| `time`          | Execution time of the operation                                         |