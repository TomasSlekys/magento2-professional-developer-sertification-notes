# Section 6: Customizing Magneto Business Logic (16%)

## 6.1 Identify / describe standard product types

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

> ### 6.1.1 How would you obtain a product of a specific type?

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

> ### 6.1.2 What tools (in general) does a product type model provide?

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

---

## 6.3 Define how products are related to the category

---

> ### 6.3.1 How do you assign and unassign products to categories?

---

## 6.4 Describe the difference in behavior of different product types in the cart

---

> ### 6.4.1 How are configurable and bundle products rendered?

---

> ### 6.4.2 How can you create a custom shopping cart renderer?

---

## 6.5 Describe native shipment functionality in Magento

---

> ### 6.5.1 How do you customize the shipment step of order management?

---

## 6.6 Describe and customize operations available in the customer account area

---

> ### 6.6.1 How would you add another tab in the “My Account” section?

---

> ### 6.6.2 How do you customize the order history page?

---

## 6.7 Add or modify customer attributes

---

> ### 6.7.1 How do you add or modify customer attributes in a setup script?

---

## 6.8 Customize the customer address

---

> ### 6.8.1 How do you add another field to the customer address entity using a setup script?

---