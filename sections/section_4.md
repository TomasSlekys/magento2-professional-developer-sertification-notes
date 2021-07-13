# Section 4: Working with Databases in Magneto (18%)

## 4.1 Describe the basic concepts of models, resource models, and collections

---

> ### 4.1.1 What are the responsibilities of each of the ORM object types? How do they relate to one another?

---

## 4.2 Describe how entity load and save occurs

---

> ### 4.2.1 How do you use the native Magento save/load process in the development process?

---

## 4.3 Describe how to filter, sort, and specify the selected values for collections and repositories

### **Collections**

> #### Filter
>
> ```php
> $collection->addFieldToFilter();
> ```

> #### Sort
>
> ```php
> $collection->addOrder();
> ```

> #### Select Column
>
> ```php
> $collection->addFieldToSelect();
> ```

> #### Pagination
>
> ```php
> $collection->setPageSize();
> $collection->setCurPage();
> ```

### **Repositories**

> #### Filter
>
> The `Filter` class is the smallest part of a Search Criteria. It allows you to add a custom field, value, and condition type to the criteria.
>
> Example of how to define a Filter:
>
> ```php
> $filter
>     ->setField("url")
>     ->setValue("%magento.com")
>     ->setConditionType("like");
> ```
> 
> This filter will find all urls with the suffix of “magento.com”.

> #### Filter Group
>
> The `FilterGroup` class acts like a collection of Filters that apply one or more criteria to a search.
> 
> The boolean `OR` statement joins Filters inside a single Filter Group.
> 
> The boolean `AND` statement joins Filter Groups inside a Search Criteria.
>
> For example:
>
> ```php
> $filter1
>     ->setField("url")
>     ->setValue("%magento.com")
>     ->setConditionType("like");
>
> $filter2
>     ->setField("store_id")
>     ->setValue("1")
>     ->setConditionType("eq");
>
> $filterGroup1->setFilters([$filter1, $filter2]);
>
> $filter3
>     ->setField("url_type")
>     ->setValue(1)
>     ->setConditionType("eq");
>
> $filterGroup2->setFilters([$filter3]);
> 
> $searchCriteria->setFilterGroups([$filterGroup1, $filterGroup2]);
>```
>
> The code above creates a Search Criteria with the Filters put together in the following way:  
> `(url like %magento.com OR store_id eq 1) AND (url_type eq 1)`

> #### Sort
> 
> To apply sorting to the Search Criteria, use the `SortOrder` class.
>
> Field and direction make up the two parameters that define a Sort Order object. 
> The field is the name of the field to sort. The direction is the method of sorting whose value can be `ASC` or `DESC`.
>
> The example below defines a Sort Order object that will sort the customer email in ascending order:
> 
> ```php
> $sortOrder
>    ->setField("email")
>    ->setDirection("ASC");
> 
> $searchCriteria->setSortOrders([$sortOrder]);
> ```


> #### Pagination
> 
> The `setPageSize` function paginates the Search Criteria by limiting the amount of entities it retrieves:
>
> ```php
> $searchCriteria->setPageSize(20); //retrieve 20 or less entities
> ```

Source: https://devdocs.magento.com/guides/v2.2/extension-dev-guide/searching-with-repositories.html

---

> ### 4.3.1 How do you select a subset of records from the database?

---

## 4.4 Demonstrate an ability to use declarative schema

> ### 4.4.1 How do you add a column using declarative schema?

The following example adds the `date_closed` column.

```diff
<schema xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:Setup/Declaration/Schema/etc/schema.xsd">
    <table name="declarative_table">
+       <column xsi:type="timestamp" name="date_closed" padding="10" comment="Time of event"/>
    </table>
</schema>
```

Source: https://devdocs.magento.com/guides/v2.4/extension-dev-guide/declarative-schema/db-schema.html#add-a-column-to-table

---

> ### 4.4.2 How do you modify a table added by another module?

The following example adds the `footer_content` column to the `cms_page` table.

```xml
<schema xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:Setup/Declaration/Schema/etc/schema.xsd">
    <table name="cms_page" resource="default" engine="innodb" comment="CMS Page Table">
        <column xsi:type="mediumtext" name="footer_content" nullable="true" comment="Page Footer Content"/>
    </table>
</schema>
```

To rename a column, delete the original column declaration and create a new one. In the new column declaration, use the
`onCreate` attribute to specify which column to migrate data from. Use the following construction to migrate data from
the same table.

The following example renames the column `description` to `body`.

```diff
<?xml version="1.0"?>
<schema xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:Setup/Declaration/Schema/etc/schema.xsd">
	<table name="custom_table">
-		<column xsi:type="varchar" name="description" nullable="false" length="255" comment="Description"/>
+		<column xsi:type="varchar" name="body" onCreate="migrateDataFrom(description)" nullable="false" length="255" comment="Body"/>
	</table>
</schema>
```

Source: https://www.dckap.com/blog/declarative-schema-magento-2/

---

> ### 4.4.3 How do you delete a column?

To drop a column in the same module, remove the node from `db_schema.xml`. The following example removes the `date_closed` column by deleting its `column` node:

```diff
<schema xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:Setup/Declaration/Schema/etc/schema.xsd">
    <table name="declarative_table">
        <column xsi:type="int" name="id_column" padding="10" unsigned="true" nullable="false" comment="Entity Id"/>
        <column xsi:type="int" name="severity" padding="10" unsigned="true" nullable="false" comment="Severity code"/>
        <column xsi:type="varchar" name="title" nullable="false" length="255" comment="Title"/>
        <column xsi:type="timestamp" name="time_occurred" padding="10" comment="Time of event"/>
-       <column xsi:type="timestamp" name="date_closed" padding="10" comment="Time of event"/>
        <constraint xsi:type="primary" referenceId="PRIMARY">
            <column name="id_column"/>
        </constraint>
    </table>
</schema>
```

To drop a column declared in another module, redeclare it with the `disabled` attribute set to `true`:

```xml
<schema xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:Setup/Declaration/Schema/etc/schema.xsd">
    <table name="declarative_table">
        <column name="date_closed" disabled="true" />
    </table>
</schema>
```

It is possible to drop a column only if it exists in the `db_schema_whitelist.json` file.

Source: https://devdocs.magento.com/guides/v2.4/extension-dev-guide/declarative-schema/db-schema.html#drop-a-column-from-a-table

---

> ### 4.4.4 How do you add an index or foreign key using declarative schema?

**Add an index**  
Use the `index` node.  
The following example adds the `INDEX_SEVERITY` index to the `declarative_table` table.

```diff
<schema xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                 xsi:noNamespaceSchemaLocation="urn:magento:framework:Setup/Declaration/Schema/etc/schema.xsd">
    <table name="declarative_table">
        <column xsi:type="int" name="id_column" padding="10" unsigned="true" nullable="false" comment="Entity Id"/>
        <column xsi:type="int" name="severity" padding="10" unsigned="true" nullable="false" comment="Severity code"/>
        <column xsi:type="text" name="title" nullable="false" length="255" comment="Title"/>
        <column xsi:type="timestamp" name="time_occurred" padding="10" comment="Time of event"/>
        <constraint xsi:type="primary" referenceId="PRIMARY">
            <column name="id_column"/>
        </constraint>
+       <index referenceId="INDEX_SEVERITY" indexType="btree">
+           <column name="severity"/>
+       </index>
    </table>
</schema>
```

Source: https://devdocs.magento.com/guides/v2.4/extension-dev-guide/declarative-schema/db-schema.html#add-an-index

**Create a foreign key**  
Use the `constraint` node  
In the following example, the selected `constraint` node defines the characteristics of the `FL_ALLOWED_SEVERITIES` foreign key.

```diff
<schema xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                 xsi:noNamespaceSchemaLocation="urn:magento:framework:Setup/Declaration/Schema/etc/schema.xsd">
    <table name="declarative_table">
        <column xsi:type="int" name="id_column" padding="10" unsigned="true" nullable="false" comment="Entity Id"/>
        <column xsi:type="int" name="severity" padding="10" unsigned="true" nullable="false" comment="Severity code"/>
        <column xsi:type="varchar" name="title" nullable="false" length="255" comment="Title"/>
        <column xsi:type="timestamp" name="time_occurred" padding="10" comment="Time of event"/>
        <constraint xsi:type="primary" referenceId="PRIMARY">
            <column name="id_column"/>
        </constraint>
+       <constraint xsi:type="foreign" referenceId="FL_ALLOWED_SEVERITIES" table="declarative_table"
+               column="severity" referenceTable="severities" referenceColumn="severity_identifier"
+               onDelete="CASCADE"/>
    </table>
</schema>
```

Source: https://devdocs.magento.com/guides/v2.4/extension-dev-guide/declarative-schema/db-schema.html#create-a-foreign-key

---

> ### 4.4.5 How do you manipulate data using data patches?

**What is a data patch?**  
A data patch is a class that contains data modification instructions. It is defined in a `<Vendor>/<Module_Name>/Setup/Patch/Data/<Patch_Name>.php` file and implements `\Magento\Framework\Setup\Patch\DataPatchInterface`.

The following code sample defines a data patch class that has a dependency.

```php
<?php
    /**
     * Copyright © Magento, Inc. All rights reserved.
     * See COPYING.txt for license details.
     */

    namespace Magento\DummyModule\Setup\Patch\Data;

    use Magento\Framework\Setup\Patch\DataPatchInterface;
    use Magento\Framework\Setup\Patch\PatchRevertableInterface;

    /**
     */
    class DummyPatch
        implements DataPatchInterface,
        PatchRevertableInterface
    {
        /**
         * @var \Magento\Framework\Setup\ModuleDataSetupInterface
         */
        private $moduleDataSetup;

        /**
         * @param \Magento\Framework\Setup\ModuleDataSetupInterface $moduleDataSetup
         */
        public function __construct(
            \Magento\Framework\Setup\ModuleDataSetupInterface $moduleDataSetup
        ) {
            /**
             * If before, we pass $setup as argument in install/upgrade function, from now we start
             * inject it with DI. If you want to use setup, you can inject it, with the same way as here
             */
            $this->moduleDataSetup = $moduleDataSetup;
        }

        /**
         * {@inheritdoc}
         */
        public function apply()
        {
            $this->moduleDataSetup->getConnection()->startSetup();
            //The code that you want apply in the patch
            //Please note, that one patch is responsible only for one setup version
            //So one UpgradeData can consist of few data patches
            $this->moduleDataSetup->getConnection()->endSetup();
        }

        /**
         * {@inheritdoc}
         */
        public static function getDependencies()
        {
            /**
             * This is dependency to another patch. Dependency should be applied first
             * One patch can have few dependencies
             * Patches do not have versions, so if in old approach with Install/Ugrade data scripts you used
             * versions, right now you need to point from patch with higher version to patch with lower version
             * But please, note, that some of your patches can be independent and can be installed in any sequence
             * So use dependencies only if this important for you
             */
            return [
                SomeDependency::class
            ];
        }

        public function revert()
        {
            $this->moduleDataSetup->getConnection()->startSetup();
            //Here should go code that will revert all operations from `apply` method
            //Please note, that some operations, like removing data from column, that is in role of foreign key reference
            //is dangerous, because it can trigger ON DELETE statement
            $this->moduleDataSetup->getConnection()->endSetup();
        }

        /**
         * {@inheritdoc}
         */
        public function getAliases()
        {
            /**
             * This internal Magento method, that means that some patches with time can change their names,
             * but changing name should not affect installation process, that's why if we will change name of the patch
             * we will add alias here
             */
            return [];
        }
    }
```

Source: https://devdocs.magento.com/guides/v2.4/extension-dev-guide/declarative-schema/data-patches.html

---

> ### 4.4.6 What is the purpose of schema patches?

**What is a schema patch?**  

A class that contains custom schema modification instructions. Schema patches are used along with declarative schema, but these patches allow complex operations such as:  
* Adding triggers, stored procedures, or functions
* Performing data migration with inside DDL operations
* Renaming tables, columns, and other entities
* Adding partitions and options to a table

It is defined in a `<Vendor>/<Module_Name>/Setup/Patch/Schema/<Patch_Name>.php` file and implements `\Magento\Framework\Setup\Patch\SchemaPatchInterface`.

Source: https://devdocs.magento.com/guides/v2.4/extension-dev-guide/declarative-schema/data-patches.html

Schema patch example (Source: https://store.magenest.com/blog/schema-patch/):

Patch file `MyModule/Setup/Patch/Schema/AddColumn.php`.  
Teh following patch adds the `name` column to the `intray_table2` table.

```php
<?php
/**
* Copyright © 2019 Magenest. All rights reserved.
* See COPYING.txt for license details.
*/
namespace Magenest\MyModule\Setup\Patch\Schema;


use Magento\Framework\DB\Ddl\Table;
use Magento\Framework\Setup\Patch\SchemaPatchInterface;
use Magento\Framework\Setup\ModuleDataSetupInterface;


class AddColumn implements SchemaPatchInterface
{
   private $moduleDataSetup;


   public function __construct(
       ModuleDataSetupInterface $moduleDataSetup
   ) {
       $this->moduleDataSetup = $moduleDataSetup;
   }


   public static function getDependencies()
   {
       return [];
   }


   public function getAliases()
   {
       return [];
   }


   public function apply()
   {
       $this->moduleDataSetup->startSetup();


       $this->moduleDataSetup->getConnection()->addColumn(
           $this->moduleDataSetup->getTable('intray_table2'),
           'name',
           [
               'type' => Table::TYPE_TEXT,
               'length' => 255,
               'nullable' => true,
               'comment'  => 'Name',
           ]
       );


       $this->moduleDataSetup->endSetup();
   }
}
```

---