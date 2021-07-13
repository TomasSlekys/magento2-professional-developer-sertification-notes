## Progress ![Progress](https://progress-bar.dev/11/?title=8/76)

# Section 1: Magento Architecture & Customization Techniques (33%)

## 1.1 Describe the Magento module-based architecture

---
> ### Describe module architecture. 
---
> ### What are the significant steps to add a new module? 
---
> ### What are the different Composer package types? 

| Name | Package Type | Description |
| :--: | :----------: | :---------- |
| Metapackage | metapackage | Technically, a Composer package type, not a Magento component type. A metapackage consists of only a `composer.json` file that specifies a list of components and their dependencies. For example, both Magento Open Source and Magento Commerce are metapackages. |
| Module | magento2-module | Code that modifies Magento application behavior. You can upload a single module to the Magento Marketplace or your module can be dependent on some parent package. |
| Theme | magento2-theme | Code that modifies the look and feel of the storefront or Magento Admin. |
| Language Package | magento2-language | Translations for the storefront or Admin. |
| Library | magento2-library | Support for libraries located in `lib/internal` instead of in the `vendor` directory. |
| Component | magento2-component | The package formed of the files that must be located in root (`.htaccess`, etc). This includes dev/tests and setup as well for now. |

Source: https://devdocs.magento.com/guides/v2.4/extension-dev-guide/prepare/dev-modtypes.html

---
> ### When would you place a module in the app/code folder versus another location?

Use `app/code/` for development or project-specific modules. Custom built modules and modules you build to extend other functionality.

Use `vendor/` for public modules (version controlled)

Root directory location  
A component’s root directory matches the component’s name and contains all its subdirectories and files. Based on how you installed Magento, you can put your component’s root directory in one of two places:

`<Magento install directory>/app`: This is the recommended location for component development. You can set up this environment by Cloning the Magento 2 GitHub repository.

* For modules, use `app/code`.
* For storefront themes, use `app/design/frontend`.
* For Admin themes, use `app/design/adminhtml`.
* For language packages, use `app/i18n`.

`<Magento install directory>/vendor`: You will find this location for installations that use the composer create-project to install the Magento 2 metapackage (which downloads the CE or EE code).

Magento installs third-party components in the `<Magento install directory>/vendor` directory. But we recommend adding your components to the `<Magento install directory>/app/code` directory. If you add your component to the `<Magento install directory>/vendor` directory, Git will ignore it because Magento adds the vendor directory to the `<Magento install directory>/.gitignore` file.

Source: https://devdocs.magento.com/guides/v2.4/extension-dev-guide/prepare/prepare_file-str.html

---

## 1.2 Describe the Magento directory structure

---

> ### Describe the Magento directory structure. 
> 
---

> ### What are the naming conventions, and how are namespaces established? 
> 
---

> ### How can you identify the files responsible for some functionality?

---

## 1.3 Utilize configuration and configuration variables scope

---

> ### Determine how to use configuration files in Magento. 

---

> ### Which configuration files are important in the development cycle?

---

> ### Describe development in the context of website and store scopes. 

---

> ### How do you identify the configuration scope for a given variable? 

---

> ### How do native Magento scopes (for example, price or inventory) affect development and decision-making processes?

---

> ### Demonstrate an ability to add different values for different scopes. 

---

> ### How can you fetch a system configuration value programmatically? 

---

> ### How can you override system configuration values for a given store using XML configuration?
> 
---

## 1.4 Demonstrate how to use dependency injection (DI)

---

> ### Demonstrate the ability to use the dependency injection concept in Magento development. 

---

> ### How are objects realized in code? 

---

> ### Why is it important to have a centralized object creation process?

---

> ### Identify how to use DI configuration files for customizing Magento. 

---

> ### How can you override a native class, inject your class into another object, and use other techniques available in di.xml (for example, virtualTypes)?

---

> ### Given a scenario, determine how to obtain an object using the ObjectManager object. 

---

> ### How would you obtain a class instance from different places in the code?

---

## 1.5 Demonstrate ability to use plugins

> ### Demonstrate an understanding of plugins. 

A plugin, or interceptor, is a class that modifies the behavior of public class functions by intercepting a function
call and running code before, after, or around that function call. This allows you to *substitute* or *extend* the behavior
of original, public methods for any *class* or *interface*.

Extensions that wish to intercept and change the behavior of a public method can create a `Plugin` class.

This interception approach reduces conflicts among extensions that change the behavior of the same class or method. 
Your `Plugin` class implementation changes the behavior of a class function, but it does not change the class itself. 
Magento calls these interceptors sequentially according to a configured sort order, so they do not conflict with one 
another.

**Limitations**  
Plugins can not be used on following:

* Final methods
* Final classes
* Non-public methods
* Class methods (such as static methods)
* `__construct` and `__destruct`
* Virtual types
* Objects that are instantiated before `Magento\Framework\Interception` is bootstrapped

Source: https://devdocs.magento.com/guides/v2.4/extension-dev-guide/plugins.html

---

> ### How are plugins used in core code? 

---

> ### How can they be used for customizations?

---

## 1.6 Configure event observers and scheduled jobs

---

> ### Demonstrate how to create a customization using an event observer. 

---

> ### How are observers registered? How are they scoped for frontend or backend? 

---

> ### How are automatic events created, and how should they be used? 

---

> ### How are scheduled jobs configured?

---

## 1.7 Utilize the CLI

---

> ### Describe the usage of bin/magento commands in the development cycle. 

---

> ### Which commands are available? 

---

> ### How are commands used in the development cycle?

---

## 1.8 Describe how extensions are installed and configured

---

> ### How would you install and verify an extension by a customer’s request?

---

# Section 2: Request Flow Processing (7%)

## 2.1 Describe how to use Magento modes

---

> ### Understand the pros and cons of using developer mode or production mode. 

---

> ### How do you enable/disable maintenance mode?

---

## 2.2 Demonstrate the ability to create a frontend controller with different response types (HTML / JSON / redirect)

---

> ### How do you identify which module/controller corresponds to a given URL?

---

> ### What would you do to create a given URL?

---

## 2.3 Demonstrate how to use URL rewrites for a catalog product view to a different URL

---

> ### How is the user-friendly URL of a product or category defined? How can you change it?

---

> ### How do you determine which page corresponds to a given user-friendly URL?

---

# Section 3: Customizing the Magento UI (15%)

## 3.1 Demonstrate the ability to customize the Magento UI using themes

---

> ### Demonstrate the ability to customize the Magento UI using themes. 

---

> ### When would you create a new theme? How do you define theme hierarchy for a project?

---

## 3.2 Demonstrate an ability to create UI customizations using a combination of a block and template

---

> ### How do you assign a template to a block? 

---

> ### How do you assign a different template to a native block?

---

## 3.3 Identify the uses of different types of blocks

---

> ### When would you use non-template block types?

---

## 3.4 Describe the elements of the Magento layout XML schema, including the major XML directives

---

> ### How do you use layout XML directives in your customizations? 

---

> ### How do you register a new layout file?

---

## 3.5 Create and add code and markup to a given page

---

> ### How do you add new content to existing pages using layout XML?

---

# Section 4: Working with Databases in Magneto (18%)

## 4.1 Describe the basic concepts of models, resource models, and collections

---

> ### What are the responsibilities of each of the ORM object types? How do they relate to one another?

---

## 4.2 Describe how entity load and save occurs

---

> ### How do you use the native Magento save/load process in the development process?

---

## 4.3 Describe how to filter, sort, and specify the selected values for collections and repositories

---

> ### How do you select a subset of records from the database?

---

## 4.4 Demonstrate an ability to use declarative schema

> ### How do you add a column using declarative schema? 

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

> ### How do you modify a table added by another module? 

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

> ### How do you delete a column? 

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

Source: https://devdocs.magento.com/guides/v2.4/extension-dev-guide/declarative-schema/db-schema.html#drop-a-column-from-a-table

---

> ### How do you add an index or foreign key using declarative schema? 

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

> ### How do you manipulate data using data patches? 

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

> ### What is the purpose of schema patches?

**What is a schema patch?**  
A schema patch contains custom schema modification instructions. These modifications can be complex. It is defined in a `<Vendor>/<Module_Name>/Setup/Patch/Schema/<Patch_Name>.php` file and implements `\Magento\Framework\Setup\Patch\SchemaPatchInterface`.

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

# Section 5: Developing with Adminhtml (11%)

## 5.1 Create a controller for an admin router

---

> ### How would you create an admin controller?

---

> ### How do you ensure the right level of security for a new controller?

---

## 5.2 Define basic terms and elements of system configuration, including scopes, website, store, store view

---

> ### How would you add a new system configuration option? 

---

> ### What is the difference in this process for different option types (secret, file)?

---

## 5.3 Define / identify basic terms and elements of ACL

---

> ### How would you add a new ACL resource to a new entity?

---

> ### How do you manage the existing ACL hierarchy?

---

## 5.4 Set up a menu item

---

> ### How do you add a new menu item to a given tab? 

---

> ### How do you add a new tab to the Admin menu?

---

## 5.5 Create appropriate permissions for users

---

> ### How are menu items related to ACL permissions? 

---

> ### How do you add a new user with given set of permissions?

---

# Section 6: Customizing Magneto Business Logic (16%)

## 6.1 Identify/describe standard product types (simple, configurable, bundled, etc.)

---

> ### How would you obtain a product of a specific type, and what tools (in general) does a product type model provide?

---

## 6.2 Describe category properties in Magento

---

> ### How do you create and manage categories?

---

## 6.3 Define how products are related to the category

---

> ### How do you assign and unassign products to categories?

---

## 6.4 Describe the difference in behavior of different product types in the cart

---

> ### How are configurable and bundle products rendered? 

---

> ### How can you create a custom shopping cart renderer?

---

## 6.5 Describe native shipment functionality in Magento

---

> ### How do you customize the shipment step of order management?

---

## 6.6 Describe and customize operations available in the customer account area

---

> ### How would you add another tab in the “My Account” section? 

---

> ### How do you customize the order history page?

---

## 6.7 Add or modify customer attributes

---

> ### How do you add or modify customer attributes in a setup script?

---

## 6.8 Customize the customer address

---

> ### How do you add another field to the customer address entity using a setup script?

---