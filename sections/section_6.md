# Section 6: Customizing Magneto Business Logic (16%)

## 6.1 Identify / describe standard product types

> ### 6.1.1 Overview

| Product Type | Description |
| ------------ | ----------- |
| Simple | A simple product is a physical item with a single SKU. Simple products have a variety of pricing and of input controls which makes it possible to sell variations of the product. Simple products can be used in association with grouped, bundle, and configurable products. |
| Configurable | A configurable product appears to be a single product with lists of options for each variation. However, each option represents a separate, simple product with a distinct SKU, which makes it possible to track inventory for each variation. |
| Grouped | A grouped product presents multiple, standalone products as a group. You can offer variations of a single product, or group them for a promotion. The products can be purchased separately, or as a group. |
| Bundle | A bundle product let customers “build their own” from an assortment of options. The bundle could be a gift basket, computer, or anything else that can be customized. Each item in the bundle is a separate, standalone product. |
| Virtual | Virtual products are not tangible products, and are typically used for products such as services, memberships, warranties, and subscriptions. Virtual products can be used in association with grouped and bundle products. |
| Downloadable | A digitally downloadable product that consists of one or more files that are downloaded. The files can reside on your server or be provided as URLs to any other server. |
| Gift Card | There are three kinds of gift cards: virtual gift cards which are sent by email, physical gift cards which are shipped to the recipient, and combined gift cards which are a combination of the two. Each has a unique code, that is redeemed during checkout. Gift cards can also be included in a grouped product. |

---

> ### 6.1.2 How would you obtain a product of a specific type?

To obtain a specific product type, build search criteria and retrieve them from the product repository:

```php
use Magento\Catalog\Api\Data\ProductInterface;
use Magento\Bundle\Model\Product\Type;

...

public function getBundleProducts()
{
    $searchCriteria = $this->searchCriteriaBuilder
        ->addFilter(ProductInterface::TYPE_ID, Type::TYPE_CODE)
        ->create();
    
    return $this->productRepository->getList($searchCriteria)->getItems();
}
```

---

> ### 6.1.3 What tools (in general) does a product type model provide?

The product type model is responsible for:

* Loading and configuring product options
* Preparing the product for the cart
* Processing product data to / from the database
* Checking whether the product can be sold
* Loading child products, when applicable

Examples from `\Magento\Catalog\Model\Product\Type\AbstractType`:

```php
    /**
     * Delete data specific for this product type
     *
     * @param \Magento\Catalog\Model\Product $product
     * @return void
     */
    abstract public function deleteTypeSpecificData(\Magento\Catalog\Model\Product $product);
```

```php
    /**
     * Retrieve product type attributes
     *
     * @param \Magento\Catalog\Model\Product $product
     * @return \Magento\Eav\Model\Entity\Attribute\AbstractAttribute[]
     */
    public function getEditableAttributes($product)
```

```php
    /**
     * Retrieve product attribute by identifier
     *
     * @param  int $attributeId
     * @param  \Magento\Catalog\Model\Product $product
     * @return \Magento\Catalog\Model\ResourceModel\Eav\Attribute|null
     */
    public function getAttributeById($attributeId, $product)
```

```php
    /**
     * Check is product available for sale
     *
     * @param \Magento\Catalog\Model\Product $product
     * @return bool
     */
    public function isSalable($product)
```

```php
    /**
     * Prepare product and its configuration to be added to some products list.
     *
     * Perform standard preparation process and then prepare options belonging to specific product type.
     *
     * @param  \Magento\Framework\DataObject $buyRequest
     * @param  \Magento\Catalog\Model\Product $product
     * @param  string $processMode
     * @return array|string
     * @SuppressWarnings(PHPMD.CyclomaticComplexity)
     */
    protected function _prepareProduct(\Magento\Framework\DataObject $buyRequest, $product, $processMode)
```

```php
    /**
     * Initialize product(s) for add to cart process
     *
     * @param \Magento\Framework\DataObject $buyRequest
     * @param \Magento\Catalog\Model\Product $product
     * @return array|string
     */
    public function prepareForCart(\Magento\Framework\DataObject $buyRequest, $product)
```

```php
    /**
     * Prepare additional options/information for order item which will be created from this product
     *
     * @param \Magento\Catalog\Model\Product $product
     * @return array
     */
    public function getOrderOptions($product)
```

```php
    /**
     * Save type related data
     *
     * @param \Magento\Catalog\Model\Product $product
     * @return $this
     */
    public function save($product)
```

```php
    /**
     * Default action to get sku of product
     *
     * @param \Magento\Catalog\Model\Product $product
     * @return string
     */
    public function getSku($product)
```

```php
    /**
     * Default action to get weight of product
     *
     * @param \Magento\Catalog\Model\Product $product
     * @return float
     */
    public function getWeight($product)
```

```php
    /**
     * Return true if product has options
     *
     * @param \Magento\Catalog\Model\Product $product
     * @return bool
     */
    public function hasOptions($product)
```

---

## 6.2 Describe category properties in Magento

---

> ### 6.2.1 How do you create and manage categories?

Categories can be made in the admin panel under Category menu item and is a tree-link structure. 
Categories can be made programmatically using `CategoryInterfaceFactory` object and saving 
with `CategoryInterfaceRepository`.

---

## 6.3 Define how products are related to the category

---

> ### 6.3.1 How do you assign and unassign products to categories?

Navigate to `Catalog > Inventory > Products` in Magento Admin, select the product you wish to add to a category, and 
use the `Categories` drop down selection options to assign it to categories.

---

## 6.4 Describe the difference in behavior of different product types in the cart

---

> ### 6.4.1 How are configurable and bundle products rendered?

`view/frontend/layout/checkout_cart_item_renderers.xml` define how products are rendered in the cart.

**Configurable**

Configurable products are shown as a single line item. It is shown with the parent product's title and the simple 
product option selected by the customer.

**Bundle**

Each product in the Bundle product is rendered as a single line item.

---

> ### 6.4.2 How can you create a custom shopping cart renderer?

You can customize the shopping cart by overriding `vendor/module/view/frontend/layout/checkout_cart_item_renderers.xml`. 

The block class depends upon the `Magento\Checkout\Block\Cart\Item\Renderer` class and includes Actions like 
`/Renderer/Action/Edit` and `/Renderer/Action/Remove`.

---

## 6.5 Describe native shipment functionality in Magento

---

> ### 6.5.1 How do you customize the shipment step of order management?

Basic steps of customizing shipping includes adding an `etc/config.xml` file and an `etc/adminhtml/system.xml` file. 

In order to extend and customize shipping, you must extend `\Magento\Shipping\Model\Carrier\AbstractCarrier` and 
implement `\Magento\Shipping\Model\Carrier\CarrierInterface`.

---

## 6.6 Describe and customize operations available in the customer account area

---

> ### 6.6.1 How would you add another tab in the “My Account” section?

To add an additional menu tab in the customer "My Account" area create the layout file in 
`Your_Company/Your_Module/view/frontend/layout/customer_account.xml`:

```xml
<body>
    <referenceBlock name="customer_account_navigation">
        <block class="Magento\Customer\Block\Account\SortLinkInterface" 
               name="yourBlockName"
               after="customer-account-navigation-address-link">
            <arguments>
                <argument name="label" xsi:type="string" translate="true">Your Label</argument>
                <argument name="path" xsi:type="string">path/to/your/module/view/</argument>
                <argument name="sortOrder" xsi:type="number">10</argument>
            </arguments>
        </block>
    </referenceBlock>
</body>
```

---

> ### 6.6.2 How do you customize the order history page?

To customise the "Order History" page create the layout file in 
`Your_Company/Your_Module/view/frontend/layout/sales_order_history.xml`. 

The `sales.order.history.info` container is a common location for modifications to be made.

---

## 6.7 Add or modify customer attributes

---

> ### 6.7.1 How do you add or modify customer attributes in a setup script?

Create `Your_Company/Your_Module/Setup/UpgradeData.php` with an `upgrade()` function:

```php
use Magento\Customer\Model\Customer;

// ...

$attribute = $this->eavConfig->getAttribute(Customer::ENTITY, Attribute::CUSTOMER_PROMOTION_PREFERENCE);
$attribute->setData('used_in_forms', ['adminhtml_customer', 'customer_account_edit']);
$attribute->save();
```

---

## 6.8 Customize the customer address

---

> ### 6.8.1 How do you add another field to the customer address entity using a setup script?

To add another field to customer address using a setup script, you use `setup/InstallData.php` while using EAV.

Make sure the attribute is assigned to a form (such as `customer_form_attribute` table) to make sure it is saveable. 

The customer address field must be manually added just like other attributes.

---