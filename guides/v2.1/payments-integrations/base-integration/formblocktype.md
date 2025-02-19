---
layout: default
group: payments-integrations
subgroup: B_integration
title: Payment info rendering in Admin checkout
menu_title: Payment info rendering in Admin checkout
menu_order: 4
version: 2.1
github_link: payments-integrations/base-integration/formblocktype.md
---

The payment information form rendering in {% glossarytooltip 29ddb393-ca22-4df9-a8d4-0024d75739b1 %}Admin{% endglossarytooltip %} order creation is defined by the block class, its template and {% glossarytooltip 73ab5daa-5857-4039-97df-11269b626134 %}layout{% endglossarytooltip %}. 

`formBlockType` is one of the arguments you must [configure for the payment method facade]({{page.baseurl}}payments-integrations/base-integration/facade-configuration.html). 

This block is used to display payment form on billing form in Admin panel. In
most cases it is enough to use the `\Magento\Payment\Block\Form\Cc`. All payments details are displayed according to specified keys in `paymentInfoKeys` option in {% glossarytooltip c1e4242b-1f1a-44c3-9d72-1d5b1435e142 %}module{% endglossarytooltip %} configuration.

## Example of custom formBlockType: Braintree payment method implementation

In the integration with the Braintree payment provider, we use the `\Magento\Braintree\Block\Form` class as `formBlockType`. It allows displaying only card types available for configured countries:
{% highlight php startinline=1 %}
class Form extends Cc
{
    /**
     * Get list of available card types of order billing address country
     * @return array
     */
    public function getCcAvailableTypes()
    {
        $configuredCardTypes = $this->getConfiguredCardTypes();
        $countryId = $this->sessionQuote->getQuote()->getBillingAddress()->getCountryId();
        return $this->filterCardTypesForCountry($configuredCardTypes, $countryId);
    }

    /**
     * Get card types available for Braintree
     * @return array
     */
    private function getConfiguredCardTypes()
    {
        $types = $this->ccType->getCcTypeLabelMap();
        $configCardTypes = array_fill_keys($this->gatewayConfig->getAvailableCardTypes(), '');

        return array_intersect_key($types, $configCardTypes);
    }

    /**
     * Filter card types for specific country
     * @param array $configCardTypes
     * @param string $countryId
     * @return array
     */
    private function filterCardTypesForCountry(array $configCardTypes, $countryId)
    {
        $filtered = $configCardTypes;
        $countryCardTypes = $this->gatewayConfig->getCountryAvailableCardTypes($countryId);
        // filter card types only if specific card types are set for country
        ...

        return $filtered;
    }
}
{% endhighlight %}


## Template

For creating a template for the payment information rendering class, you can use the default [cc.phtml]({{site.mage2100url}}app/code/Magento/Payment/view/adminhtml/templates/form/cc.phtml) as example.

Then add the the template to the billing form layout `sales_order_create_index.xml`.

The following example adds the Braintree-specific template [`app/code/Magento/Payment/view/adminhtml/templates/form/cc.phtml`]({{site.mage2100url}}app/code/Magento/Payment/view/adminhtml/templates/form/cc.phtml) to the [billing form layout in the Braintree module]({{site.mage2100url}}app/code/Magento/Braintree/view/adminhtml/layout/sales_order_create_index.xml).

{% highlight xml %}
<page xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:View/Layout/etc/page_configuration.xsd">
    <body>
        <referenceBlock name="order_create_billing_form">
            <action method="setMethodFormTemplate">
				<!-- your method code and template -->
                <argument name="method" xsi:type="string">braintree</argument>
                <argument name="template" xsi:type="string">Magento_Braintree::form/cc.phtml</argument>
            </action>
        </referenceBlock>
    </body>
</page>
{% endhighlight %}


## What's next

- [Payment method facade]({{page.baseurl}}payments-integrations/base-integration/facade-configuration.html)
- [Add a gateway command]({{page.baseurl}}payments-integrations/base-integration/payment-action.html) 