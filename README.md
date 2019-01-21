### How to create / edit documentation file ?

Documentation files are edited in Markdown language. Please, check on [this link](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet) in order to learn about Markdown syntax.

Then, you can use any editing software to write your documentation. Here is an [Markdown online editor](https://stackedit.io).

Next, push your changes from your corresponding HiPay's repository on GitHub.

Last step, please check your edited documentation files via the [HiPay Developer Portal](https://developer.hipay.com). In fact, according to the HiPay's GitHub repository, follow the corresponding link :

 
| hipay-docs-dev | hipay-docs-preprod | hipay-docs |
|--|--|--|
| [Dev DevPortal](http://dev-developer.hipay.com/doc-branch) | [Preprod DevPortal](https://preprod-developer.hipay.com/doc-branch) | **[Prod DevPortal](https://developer.hipay.com/doc-branch)** |

Now, from this page, please specify in URL the matching text elements as the following template :
developer.hipay.com/doc-branch/`integrationName`/`fileName`?branch=`branchName`
Or this template :
developer.hipay.com/doc-branch/`integrationName`/`chapterName`/`fileName`?branch=`branchName`

Where :
 - `integrationName` represents the name of the documentation folder. Ex : hipay-enterprise-sdk-prestashop_1-6-1-7
 - `chapterName` (optionnal) represents the name of a chapter inside the integration folder (specified as the first parameter). Ex : Integration-Guide
 - `fileName` represents the name of a file inside the integration folder. If `chapterName` is specified, the file should be inside the chapter folder. Including the file's extension for this parameter is optionnal. Ex : 1-Integration-Guide
 - `branchName` represents the name of the branch you want to position yourself in the GitHub repository. Ex : feature/MOTO_PS

Full example : http://dev-developer.hipay.com/doc-branch/hipay-enterprise-sdk-js_3/Integration-Guide/1-Integration-Guide/?branch=sdkjs-v3