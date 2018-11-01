# Magento Integration

<aside class = "notice">
  Caution: If you are using the Idev OneStepCheckout module, please note that this module is only compatible with Idev OneStepCheckout 3.1 and higher
</aside>

Copy the contents of the Zip file to the directory where your Magento is installed.
<aside class = "notice">
  Note that you must also install the WLPayment and Fraud Control module.
</aside>

Create the log folder inside the Magento var directory, it will become <magentodir>/var/log. This directory must have write general permissions (777).

## Setup in the Magento admin panel

1. Go to the Magento admin page.

2. Go to System> Configuration> Cache Management.
In the "All Cache" field, choose "Update"
Then click "Save cache settings"

3. In the iPag module options, complete the fields as shown below:
    * Enabled: Yes
    * Title: Credit Cards
    * New Order Status: Processing or Pending
    * Code/Login iPag: Fill in the field with the login used to access our panel
    * Environment: Production
    * Interest rate (%): Monthly interest rate. Leave blank if you do not want to charge interest
    * Number of installments for cash value: Maximum number of installments for which no interest will be charged.
    * Payment allowed countries: All countries allowed
    * Debug: No

<aside class = "notice">
  Be sure to copy the app/design/frontend/base/default/template/librepag/directory to the corresponding directory of the template being used.
</aside>

## Success and Failure Pages
<aside class = "warning">
    Make sure you've copied the files below for your template.
These files contain changes so that the success and failure pages display information about the transaction.
Without this, the operator will not approve the store.
</aside>
*app/design/frontend/default/default/template/checkout/onepage/failure.phtml*
*app/design/frontend/default/default/template/checkout/success.phtml*


If you are using the Idev OneStepCheckout module, also update the summary.phtml file that is in the directory below

*app/design/frontend/default/default/template/onestepcheckout/summary.phtml*

Updating this file is optional, but ensures that the value of the parcels is displayed correctly at checkout.

## Note
In Magento version 1.4, to improve the look of Checkout, you may need to enter the commands in the styles.css file

## Setting Payment Methods
Sign in to your <a href="https://painel-sandbox.ipag.com"> iPag Dashboard </a> and set up payment methods.
<aside class = "notice">
    If you have any questions, please contact us by sending an email to suporte@ipag.com.br
</aside>