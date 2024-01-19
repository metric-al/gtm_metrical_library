# Installing Metrical for Site & Cart Abandonment - GTM Implementation

This document details the steps necessary for the Installation of Metrical on your e-commerce web site via Google Tag Manager. Should you have any questions or concerns, please don't hesitate to reach out at support@getMetrical.com.

For general help setting up Google Tag Manager, see the Google developers [GTM guide](https://developers.google.com/tag-platform/tag-manager).

**Note: You will need a Metrical site name and site code to complete this process.**

## Contents

[Customer Checklist](#customer-checklist)

[General Info](#general-info)

- [Template Installation](#template-installation)

- [Deploying to Test/Staging & Production](#deploying-to-teststaging--production)

- [Tag Fields](#tag-fields)

[Metrical Library Tag](#metrical-library-tag)

[Metrical Action Tags](#metrical-action-tags)

- [Metrical Action - Page Load Tag](#metrical-action---page-load-tag)

- [Metrical Action - Product Information Tag](#metrical-action---product-information-tag)

- [Metrical Action - View Cart Tag](#metrical-action---view-cart-tag)

- [Metrical Action - Add to Cart Tag](#metrical-action---add-to-cart-tag)

- [Metrical Action - Remove From Cart Tag](#metrical-action---remove-from-cart-tag)

- [Metrical Action - Replace Cart Tag](#metrical-action---replace-cart-tag)

- [Metrical Action - Clear Cart Tag](#metrical-action---clear-cart-tag)

- [Metrical Action - Start Checkout Tag](#metrical-action---start-checkout-tag)

- [Metrical Action - Order Placed Tag](#metrical-action---order-placed-tag)

[Miscellaneous](#miscellaneous)

- [Custom Environment Selection](#custom-environment-selection)

- [Domain Whitelist](#domain-whitelist)

- [Metrical and Cookies](#metrical-and-cookies)

- [Testing/Verification](#testingverification)

## Customer Checklist

1. Install the [Metrical Library Tag](#metrical-library-tag) within GTM so that it's firing on each page of the site. Metrical will provide the site name and site code to use, as well as any override values if necessary.

2. Review the list of [Metrical Action Tags](#metrical-action-tags) below and see how they align with your data layer events.

3. Make sure the list of domains that Metrical uses are whitelisted. Those can be added to CSP. See [Domain Whitelist](#domain-whitelist).

4. Alert Metrical that this tag is in place and then Metrical will enable the configuration after review and start the process for additional configuration for the site.

5. Metrical must fire on every page on the site. Make sure Metrical fires on all pages on the site (e.g. home, search results, category/gallery pages, product pages, cart, checkout, and order confirmation).
If there are certain pages where this won’t be possible, please bring this up with your Metrical contact.
Ensure that Metrical isn’t blocked by a CSP (Content Security Policy) on any page.

6. Provide analytics/reporting access

    - Google Analytics - Grant “Analyst” access to analytics@metric.al. See Google's directions on [adding a guest user](https://support.google.com/analytics/answer/1009702?hl=en#Add&zippy=%2Cin-this-article).

    - Adobe Analytics/Experience Cloud - Please create an [Analysis Workspace of your ecommerce funnel](https://experienceleague.adobe.com/docs/analytics/analyze/analysis-workspace/curate-share/share-projects.html?lang=en#share-public-link) and share w/ analytics@metric.al.

## General Info

### Template Installation

1. Download the `template.tpl` file found within the template's GitHub repository. You may find a link to a template's GitHub repo directly under its section heading in the current document.

2. Once you have the template file, create a new template within GTM and select 'import' from the vertical three dot menu. Select the template file you downloaded.

3. After creating the template, create a new tag and select the template you just created as the tag's configuration.

4. Finally, follow the instructions below for each tag to ensure that the tag's fields and triggers are properly configured.

### Deploying to Test/Staging & Production

Please work with your Metrical point of contact (PoC) to ensure you’re using the correct endpoints when deploying your software changes. All Metrical implementations must first take place in a Test/Staging environment. Once your Metrical PoC has ensured that all data transmitted to Metrical is formatted correctly and persisting as expected, you may then move your changes to a production environment.

### Tag Fields

For each of the tags below, you will find a table containing the fields available when setting them up. Each field will fall into one of the following categories:

- Basic Text: These fields are primarily used for configuring. They require an input that is static text and are not connected to a javascript or GTM variable.

- Checkbox: These fields are simple checkboxes that represent a boolean data type.

- GTM Variable: For each tag template field marked as a GTM variable, you will need to ensure that your GTM implementation has a data layer variable set up that gets pushed to your data layer for the event that you want to capture. The field's input should be the name of your GTM variable wrapped in double brackets, such as `{{variableName}}`. The value data type column in the fields table tells you what type the GTM variable's value should be. For further help, see Google's [data layer](https://support.google.com/tagmanager/answer/6164391?hl=en), [user defined variables](https://support.google.com/tagmanager/topic/9125128?hl=en&ref_topic=7683268&sjid=13085019183759399743-NA), and [custom event trigger](https://support.google.com/tagmanager/answer/7679219?hl=en) documentation.

- Object Key: These fields represent the names of keys within an object in your data layer. For example, the [Metrical Event - Add to Cart Tag](#metrical-event---add-to-cart) asks for an object key representing the item ID in the cart object being added via the "Item Object ID Variable Name" field. Let's suppose you have a custom event that publishes the following to the data layer when a customer adds items to their cart:

        window.dataLayer.push({
            event: 'cart_add',
            itemsAdded: [
                {
                    prodId: products[0].id,
                    prodQuant: cart[products[0].id].quantity,
                    prodPrice: products[0].price,
                    prodSku: products[0].selectedVariant.sku
                },
                {
                    prodId: products[1].id,
                    prodQuant: cart[products[1].id].quantity,
                    prodPrice: products[1].price,
                    prodSku: products[1].selectedVariant.sku
                }
            ]
        })

    Your input to the "Item Object ID Key Name" field should be `prodId`. Note that there are no double curly braces surrounding the input like there were in the GTM variable example. These fields are not GTM variables. Metrical needs to know how to access the data within objects when fields are repeatable and can't be represented by GTM variables. This is what the object key category of fields is for. The value data type column in the fields table tells you what type the key's associated value should be.

## Metrical Library Tag

[GitHub](https://github.com/metric-al/gtm_metrical_library)

Primary tag that will load the Metrical Cart Abandonment library onto your site. This tag must load on every page of your site.

#### Fields

| Field | Description | Category | Value Data Type |
| :-: | - | :-: | :-: |
| Site Name | Metrical name for your site. Contact your PoC for details. | Basic Text | N/A | Yes
| Site Code | Code for site's endpoint. Your PoC will tell you if this is required. | Basic Text | N/A | No
| Metrical ID Override | Custom input for Metrical ID. Your PoC will tell you if this is required. | Basic Text | N/A | No
| AC Library Source Override | Custom override for library endpoint. Your PoC will tell you if this is required. | Basic Text | N/A | No
| Use Stage Environment | Static text representing whether Metrical will use your stage environment. If value is "true" Metrical will use your stage environment. If value is "false" Metrical will use your production environment.
Can also be configured to use a dynamic variable. If using a GTM variable, the variable's value should be a boolean. See [Custom Environment Selection](#custom-environment-selection). | Basic Text or GTM Variable | N/A or Boolean | No

See [Tag Fields](#tag-fields) for information on the type of input desired for each category.

#### Sequencing

The Metrical Library Tag must be set to fire immediately before each and every Metrical Action tag. For help, see Google's [tag sequencing](https://support.google.com/tagmanager/answer/6238868?hl=en) documentation.

#### Triggers

If you are also installing the [Metrical Event - Page Load Tag](#metrical-event---page-load), then the Metrical Library Tag must be set to trigger immediately before it. To do this, set the Metrical Event - Page Load Tag to trigger on the Page View - DOM Ready trigger type. If you are selecting this option, do not set an independent trigger for the Metrical Library Tag.

If you are not installing the Metrical Event - Page Load Tag, then the Metrical Library Tag should be set to trigger on the Page View - DOM Ready trigger type.

## Metrical Action Tags

Metrical’s operation through GTM has two parts:
1. The [Metrical Library Tag](#metrical-library-tag) loads and prepares Metrical to operate on the site.

2. The Metrical Action tags are then triggered by the tag manager to let Metrical know when those events take place and what kind of supporting data is supplied with those events.

In order to use any Metrical Action tags, the Metrical Library Tag must be loaded first.

You may not have all of these events or all the information available that a given event asks for. In most cases, this is ok.  The key events that Metrical wants to be notified about are:

- [Metrical Action - Page Load](#metrical-action---page-load-tag)
- [Metrical Action - Order Placed](#metrical-action---order-placed-tag)
- [Metrical Action - Add to Cart](#metrical-action---add-to-cart-tag)
- [Metrical Action - Remove From Cart](#metrical-action---remove-from-cart-tag)

If you don’t have a specific piece of data that is highlighted in the example call, you can omit that key/value pair or just set the value to null.


### Metrical Action - Page Load Tag

[GitHub](https://github.com/metric-al/gtm_page_load)

Indicates a new page or view of the website was loaded.  A fair bit of context can be provided for this event if it’s available when the event fires in the tag manager.

Metrical Library Tag must be installed to use this tag.

#### Fields

| Field | Description | Category | Value Data Type |
| :-: | - | :-: | :-: |
| Language | ISO code for language used on the page. | GTM Variable | String | No
| Currency | ISO Code for currency used on the page. | GTM Variable | String | No
| Customer ID | Identifier for the current customer. | GTM Variable | String | No
| Is Logged In | Represents whether the current customer is logged in. | GTM Variable | Boolean | No
| Customer Page Type | The category of the current page. | GTM Variable | String | No
| Item ID | If the page loaded is a PDP, the identifier for your product. | GTM Variable | String |
| SKU | If the page loaded is a PDP, the list of available SKUs for your product. | GTM Variable | Array |
| Item Name | If the page loaded is a PDP, the name of your product. | GTM Variable | String |
| Item Price | If the page loaded is a PDP, the price of your product. | GTM Variable | Number |
| Item MSRP Price | If the page loaded is a PDP, the original price of your product. | GTM Variable | Number |
| Item Sale Price | If the page loaded is a PDP, the sale price of your product if there's an additional discount. | GTM Variable | Number |
| Item Category | If the page loaded is a PDP, the category of your product. | GTM Variable | String |
| Item Subcategory | If the page loaded is a PDP, the subcategory of your product. | GTM Variable | String |
| Item Images | If the page loaded is a PDP, the list of image URLs for your product. | GTM Variable | Array |
| Item Featured Image | If the page loaded is a PDP, the URL of your product's main image. | GTM Variable | String |
| Item Gender | If the page loaded is a PDP, the gender alignment of your product. | GTM Variable | String
| Item Vendor | If the page loaded is a PDP, the name of your product's vendor. | GTM Variable | String |
| Item Brand | If the page loaded is a PDP, the name of your product's brand. | GTM Variable | String |
| Cart | List representing the current customer's cart. Variable's value should be an array of objects, with each object including values that correspond to the following keys: id, quantity, price, and sku. Enter the names of the keys representing those values in the fields below. | GTM Variable | Array
| Item Object ID Key Name | Name of the key on each item object in your cart representing the item's ID. | Object Key | String |
| Item Object Quantity Key Name | Name of the key on each item object in your cart representing the item's quantity. | Object Key | Number |
| Item Object Price Key Name | Name of the key on each item object in your cart representing the item's price. | Object Key | Number |
| Item Object SKU Key Name | Name of the key on each item object in your cart representing the item's SKU. | Object Key | String |

See [Tag Fields](#tag-fields) for information on the type of input desired for each category.

#### Sequencing

The Metrical Library Tag must be set to fire immediately before each and every Metrical Action tag. For help, see Google's [tag sequencing](https://support.google.com/tagmanager/answer/6238868?hl=en) documentation.

#### Triggers

It is recommended that you trigger Metrical Action - Page Load Tag on a Page View - DOM Ready trigger type. However, if you have a custom event that fires when the DOM has loaded, you may opt to trigger the Metrical Action - Page Load Tag with that event.


### Metrical Action - Product Information Tag

[GitHub](https://github.com/metric-al/gtm_product_information)

If your tag manager processes and prepares product/PDP information asynchronous to a page load event, you can use this tag to supply just the product information the page is displaying.

Metrical Library Tag must be installed to use this tag.

#### Fields

| Field | Description | Category | Value Data Type |
| :-: | - | :-: | :-: |
| Item ID | If the page loaded is a PDP, the identifier for your product. | GTM Variable | String |
| SKU | If the page loaded is a PDP, the list of available SKUs for your product. | GTM Variable | Array |
| Item Name | If the page loaded is a PDP, the name of your product. | GTM Variable | String |
| Item Price | If the page loaded is a PDP, the price of your product. | GTM Variable | Number |
| Item MSRP Price | If the page loaded is a PDP, the original price of your product. | GTM Variable | Number |
| Item Sale Price | If the page loaded is a PDP, the sale price of your product if there's an additional discount. | GTM Variable | Number |
| Item Category | If the page loaded is a PDP, the category of your product. | GTM Variable | String |
| Item Subcategory | If the page loaded is a PDP, the subcategory of your product. | GTM Variable | String |
| Item Images | If the page loaded is a PDP, the list of image URLs for your product. | GTM Variable | Array |
| Item Featured Image | If the page loaded is a PDP, the URL of your product's main image. | GTM Variable | String |
| Item Gender | If the page loaded is a PDP, the gender alignment of your product. | GTM Variable | String
| Item Vendor | If the page loaded is a PDP, the name of your product's vendor. | GTM Variable | String |
| Item Brand | If the page loaded is a PDP, the name of your product's brand. | GTM Variable | String |

See [Tag Fields](#tag-fields) for information on the type of input desired for each category.

#### Sequencing

The Metrical Library Tag must be set to fire immediately before each and every Metrical Action tag. For help, see Google's [tag sequencing](https://support.google.com/tagmanager/answer/6238868?hl=en) documentation.

#### Triggers

This tag should be triggered by a custom event configured to fire when your product information becomes available.


### Metrical Action - View Cart Tag

[GitHub](https://github.com/metric-al/gtm_view_cart)

For use if you have an event that fires when the customer displays their cart information. This does not require a payload object.

Metrical Library Tag must be installed to use this tag.

#### Fields

This tag has no fields.

#### Sequencing

The Metrical Library Tag must be set to fire immediately before each and every Metrical Action tag. For help, see Google's [tag sequencing](https://support.google.com/tagmanager/answer/6238868?hl=en) documentation.

#### Triggers

This tag should be triggered by a custom event that is configured to fire when the customer views their cart information.


### Metrical Action - Add to Cart Tag

[GitHub](https://github.com/metric-al/gtm_add_to_cart)

This can be used when an add to cart event is fired. The payload contains the information about the item(s) being added.

Metrical Library Tag must be installed to use this tag.

#### Fields

| Field | Description | Category | Value Data Type |
| :-: | - | :-: | :-: |
| Item(s) Added | Item added or list of items added. <br> If there will always be only one item added at a time via this action, then the variable's value should be an object representing the item added that includes values that correspond to the following keys: id, quantity, price, and sku. <br> If there may be multiple items added at a time, then the variable's value should be an array of objects, with each object including id, quantity, price, and sku. <br> Regardless of whether one or multiple items may be added at a time, enter the names of the keys representing the values just mentioned in the fields below. | GTM Variable | Object or Array |
| Item Object ID Key Name | Name of the key on each item object representing the item's ID. | Object Key | String |
| Item Object Quantity Key Name | Name of the key on each item object representing the item's quantity. | Object Key | Number |
| Item Object Price Key Name | Name of the key on each item object representing the item's price. | Object Key | Number |
| Item Object SKU Key Name | Name of the key on each item object representing the item's SKU. | Object Key | String |

See [Tag Fields](#tag-fields) for information on the type of input desired for each category.

#### Sequencing

The Metrical Library Tag must be set to fire immediately before each and every Metrical Action tag. For help, see Google's [tag sequencing](https://support.google.com/tagmanager/answer/6238868?hl=en) documentation.

#### Triggers

This tag should be triggered by a custom event that is configured to fire when an item or items are added to a customer's cart.


### Metrical Action - Remove From Cart Tag

[GitHub](https://github.com/metric-al/gtm_remove_from_cart)

This can be used when a remove from cart event is fired. The payload contains the information about the item(s) being removed.

Metrical Library Tag must be installed to use this tag.

#### Fields

| Field | Description | Category | Value Data Type |
| :-: | - | :-: | :-: |
| Item(s) Removed | Item removed or list of items removed. <br> If there will always be only one item removed at a time via this action, then the variable's value should be an object representing the item removed that includes values that correspond to the following keys: id, quantity, price, and sku. <br> If there may be multiple items removed at a time, then the variable's value should be an array of objects, with each object including id, quantity, price, and sku. <br> Regardless of whether one or multiple items may be removed at a time, enter the names of the keys representing the values just mentioned in the fields below. | GTM Variable | Object or Array |
| Item Object ID Key Name | Name of the key on each item object representing the item's ID. | Object Key | String |
| Item Object SKU Key Name | Name of the key on each item object representing the item's SKU. | Object Key | String |

See [Tag Fields](#tag-fields) for information on the type of input desired for each category.

#### Sequencing

The Metrical Library Tag must be set to fire immediately before each and every Metrical Action tag. For help, see Google's [tag sequencing](https://support.google.com/tagmanager/answer/6238868?hl=en) documentation.

#### Triggers

This tag should be triggered by a custom event that is configured to fire when an item or items are removed from a customer's cart.


### Metrical Action - Replace Cart Tag

[GitHub](https://github.com/metric-al/gtm_replace_cart)

There may be events or points in a customer’s session where you generate or have the actual cart state available. You can use this tag to tell Metrical about the actual cart state and Metrical can normalize its internal cart tracker to this information.

Metrical Library Tag must be installed to use this tag.

#### Fields

| Field | Description | Category | Value Data Type |
| :-: | - | :-: | :-: |
| Cart | List representing the current customer's cart. Variable's value should be an array of objects, with each object including values that correspond to the following keys: id, quantity, price, and sku. Enter the names of the keys representing those values in the fields below. | GTM Variable | Array
| Item Object ID Key Name | Name of the key on each item object in your cart representing the item's ID. | Object Key | String |
| Item Object Quantity Key Name | Name of the key on each item object in your cart representing the item's quantity. | Object Key | Number |
| Item Object Price Key Name | Name of the key on each item object in your cart representing the item's price. | Object Key | Number |
| Item Object SKU Key Name | Name of the key on each item object in your cart representing the item's SKU. | Object Key | String |

See [Tag Fields](#tag-fields) for information on the type of input desired for each category.

#### Sequencing

The Metrical Library Tag must be set to fire immediately before each and every Metrical Action tag. For help, see Google's [tag sequencing](https://support.google.com/tagmanager/answer/6238868?hl=en) documentation.

#### Triggers

This tag should be triggered by a custom event that is configured to fire when you want to update state about the cart.


### Metrical Action - Clear Cart Tag

[GitHub](https://github.com/metric-al/gtm_clear_cart)

If you have an event that triggers if the system or customer clears their cart, you can use this tag to have Metrical do the same to its internal cart state.

Metrical Library Tag must be installed to use this tag.

#### Fields

This tag has no fields.

#### Sequencing

The Metrical Library Tag must be set to fire immediately before each and every Metrical Action tag. For help, see Google's [tag sequencing](https://support.google.com/tagmanager/answer/6238868?hl=en) documentation.

#### Triggers

This tag should be triggered by a custom event that is configured to fire when the system or customer clears their cart.


### Metrical Action - Start Checkout Tag

[GitHub](https://github.com/metric-al/gtm_start_checkout)

Metrical likes to know when a customer enters the checkout workflow. This tag can be fired when that occurs. Payload contains the current state of the cart if available.

Metrical Library Tag must be installed to use this tag.

#### Fields

| Field | Description | Category | Value Data Type |
| :-: | - | :-: | :-: |
| Cart | List representing the current customer's cart. Variable's value should be an array of objects, with each object including values that correspond to the following keys: id, quantity, price, and sku. Enter the names of the keys representing those values in the fields below. | GTM Variable | Array
| Item Object ID Key Name | Name of the key on each item object in your cart representing the item's ID. | Object Key | String |
| Item Object Quantity Key Name | Name of the key on each item object in your cart representing the item's quantity. | Object Key | Number |
| Item Object Price Key Name | Name of the key on each item object in your cart representing the item's price. | Object Key | Number |
| Item Object SKU Key Name | Name of the key on each item object in your cart representing the item's SKU. | Object Key | String |

See [Tag Fields](#tag-fields) for information on the type of input desired for each category.

#### Sequencing

The Metrical Library Tag must be set to fire immediately before each and every Metrical Action tag. For help, see Google's [tag sequencing](https://support.google.com/tagmanager/answer/6238868?hl=en) documentation.

#### Triggers

This tag should be triggered by a custom event that is configured to fire when a customer enters the checkout workflow.


### Metrical Action - Order Placed Tag

[GitHub](https://github.com/metric-al/gtm_order_placed) | [Community Template Gallery]()

This event is fired when a purchase takes place on the site. This is a fairly key event for Metrical to receive. Ideally, all work flows that result in an order being placed should trigger this tag and provide as much of the context in the payload as possible.

Metrical Library Tag must be installed to use this tag.

#### Fields

| Field | Description | Category | Value Data Type |
| :-: | - | :-: | :-: |
| Order ID | Identifier for the order placed. | GTM Variable | String |
| Currency | ISO Code for currency used on the page. | GTM Variable | String |
| Total Cart Value | Final value of the transaction. | GTM Variable | Number |
| Subtotal Cart Value | Total value of the order's items. | GTM Variable | Number |
| Tax | Total value of tax charged. | GTM Variable | Number |
| Shipping Method Cost | Total value of shipping cost charged. | GTM Variable | Number |
| Coupon Code(s) | Coupon code(s) applied. If there is a single coupon code, the variable's value should be a string. Otherwise, the variable's value should be an array of strings. | GTM Variable | String or Array |
| Discount Value | Total amount discounted. | GTM Variable | Number |
| Discounts Applied | Whether or not a discount was applied. | GTM Variable | Boolean |
| Payment Method | Payment method used to complete transaction. | GTM Variable | String |
| Shipping Method | Order's shipping method. | GTM Variable | String |
| Billing City | City of the customer's billing address. | GTM Variable | String |
| Billing State | State of the customer's billing address. | GTM Variable | String |
| Billing County | County of the customer's billing address. | GTM Variable | String |
| Cart | List representing the current customer's cart. Variable's value should be an array of objects, with each object including values that correspond to the following keys: id, quantity, price, and sku. Enter the names of the keys representing those values in the fields below. | GTM Variable | Array
| Item Object ID Key Name | Name of the key on each item object in your cart representing the item's ID. | Object Key | String |
| Item Object Quantity Key Name | Name of the key on each item object in your cart representing the item's quantity. | Object Key | Number |
| Item Object Price Key Name | Name of the key on each item object in your cart representing the item's price. | Object Key | Number |
| Item Object SKU Key Name | Name of the key on each item object in your cart representing the item's SKU. | Object Key | String |

See [Tag Fields](#tag-fields) for information on the type of input desired for each category.

#### Sequencing

The Metrical Library Tag must be set to fire immediately before each and every Metrical Action tag. For help, see Google's [tag sequencing](https://support.google.com/tagmanager/answer/6238868?hl=en) documentation.

#### Triggers

This tag should be triggered by custom events that fire when an order is placed.


## Miscellaneous

### Custom Environment Selection

Environment selection (stage, production) is handled through the [Metrical Library Tag](#metrical-library-tag) "Use Stage Environment" field. Generally, this is a static checkbox that is selected when setting up the tag.

However, you may opt to implement dynamic environment selection on your site. In order to do this, you will need to establish a GTM data layer variable that gets pushed to the data layer before the GTM Page Load - DOM Ready trigger fires. The variable's value should be true when the stage environment should be used and false when the production environment should be used.

When setting up the Metrical Library Tag, select your custom GTM variable as the input to the "Use Stage Environment" field.

### Domain Whitelist

For a wildcard whitelist, use:

- https://*.getmetrical.com

If you require all the FQDN, then here is the current list of domains used:

- <div style="display: inline">https://content.getmetrical.com</div>
- <div style="display: inline">https://api-v2.getmetrical.com</div>
- <div style="display: inline">https://content-stage.getmetrical.com</div>
- <div style="display: inline">https://api-v2-stage.getmetrical.com</div>

If you can add these, then add them, but they are not required:
- <div style="display: inline">https://api.getmetrical.com</div>
- <div style="display: inline">https://phoneservices.getmetrical.com</div>

This applies to javascript and CSS files from Metrical’s systems.

### Metrical and Cookies

Metrical’s client Javascript library can leverages cookies as part of its operation, but by default does not

If cookies are required to make Metrical work on the site, it sets first party cookies, under the domain of the customer. Some sites run cookie scrubbers/governors/whitelist tools. If you leverage such a tool on your site, you’ll need to whitelist all Metrical cookies. A general pattern match is:
- _Metrical*
- _MSS*

### Testing/Verification

#### How to verify the tool is running:

1. Verify that the library has loaded in your network tab.

    ![Library Load Verification Screenshot](https://raw.githubusercontent.com/metric-al/images/main/image2.png)

2. In console, run: _MetricalAbandonCart.initTimestamp

3. Verify that an epoch (in milliseconds) is returned.

    ![Epoch Return Verification Screenshot](https://raw.githubusercontent.com/metric-al/images/main/image1.png)


#### How to verify a set of data being sent:

1. Look for a network call to the interaction endpoint to see the data that is sent to Metrical:

    ![Interaction Verification Screenshot](https://raw.githubusercontent.com/metric-al/images/main/image4.png)
