# Section 1: Magento Architecture & Customization Techniques (33%)

## 1.1 Describe the Magento module-based architecture

> ### 1.1.0 Overview

* There are five areas: `adminhtml`, `frontend`, `base`, `webapi_rest`, `webapi_soap` and `cron`. Not all areas are 
always available. For instance, the cron area is only used when running cron jobs.
* The three necessary files to **bootstrap** a module are `registration.php`, `etc/module.xml` and `composer.json`.

> ### 1.1.1 What are the significant steps to add a new module?

1. Create `composer.json`
2. Create `registration.php`
3. Create `etc/module.xml`
4. Enable module via console command `php bin/magento module:enable Module_Name`
5. Run console command `php bin/magento setup:upgrade`

---

> ### 1.1.2 What are the different Composer package types?

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

> ### 1.1.3 When would you place a module in the app/code folder versus another location?

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

> ### 1.2.1 Describe the Magento directory structure.

The whole Magento structure can be divided into the following types:

* Magento root structure
* Modules structure
* Themes structure

---

> ### 1.2.2 What are the naming conventions, and how are namespaces established?

The root Magento directory contains these primary folders:

* `app/code`: where your custom modules are found. Hopefully, very few 3rd- party modules are installed here because it 
is difficult to remember to keep these up-to-date. Installing modules through Composer provides the module vendor with 
* an easier way to automatically update their modules and dependencies.

* `app/design/[frontend or adminhtml]`: this is where themes are stored.

* `app/i18n`: this is where translation (language) packages are stored.

* `app/etc/`: this is where the configuration files are stored:

* `app/etc/env.php`: this is the configuration file that is per environment. This contains your database, redis, and 
queueing (Advanced Message Queuing Protocol or AMQP) configuration, among other things. This file should NOT be in your 
* Git repository as it contains sensitive information.

* `app/etc/config.php`: this file should be committed to your Git repository. It ultimately merges with the env.php 
file. This holds the configuration for enabling/disabling modules and can include defaults for Store Configuration and 
theming.

* `bin/`: you will type the command bin/magento thousands of times in your tenure as Magento developer … so this is 
where this file is stored.

* `dev/`: this contains the configuration for built-in tools, such as Grunt. It also contains Magento’s test suite.

* `generated/`: files in this folder are created by Magento. While it is technically safe to delete in production, it’s 
a bad idea as it will result in downtime or a slowdown on the website while Magento rebuilds this directory. This is 
where Magento stores factory classes, interceptor classes (used to make plugins work), proxy classes (used to lazy-load 
methods to prevent the constructor from triggering too early and causing a problem elsewhere in the system), and 
extension attribute interfaces and classes.

* `lib/`: this folder contains the internal libraries on which Magento relies. Note that jQuery is found in 
`lib/web/jquery/`.

* `pub/`: this is the directory that should be HTTP root (i.e., the directory that is browsed when you go to 
https://bkaushik.com) for the webserver. While the root Magento folder would work, you risk exposing folders such as 
your var folder with a misconfiguration. When you browse to a Magento website, the webserver first checks to see if 
that exact path exists as a file. If it doesn’t, the .htaccess or Nginx configuration rewrites the request to 
`pub/index.php`, which begins the normal Magento request.

* `pub/media/`: this is where the website’s images are stored.

* `pub/static/`: this contains the .css, .js and .html files that have been processed and are ready for the end-user to 
download. If a path is requested but doesn’t exist as a file in this folder, Magento tries to create it when not in 
production mode (or if you add a store with a new language). If your website is in developer mode, and you 
run `bin/magento dev:source-theme:deploy`, most files in this directory will be symlinked to their sources throughout 
the Magento codebase. In production mode, you run bin/magento setup:static-content:deploy to process and copy files 
to this directory.

*What’s the difference?* dev:source-theme:deploy creates symlinks from module’s LESS files into the pub/static 
directory. 
This allows LESS files changes, in modules, to always be of the latest version. setup:static-content:deploy copies 
all necessary files (JS, HTML and LESS compiled to CSS) to the pub/static directory. Once this command is run, 
changes to your LESS and JS files in your module will not be visible on the frontend.

* `setup/`: this directory contains important files related to installing Magento. If your webserver is configured so 
that the document root matches the Magento root directory (ie, not /pub), you can browse to your website /setup URL. 
This configuration should be very temporary, and production’s document root should always be /pub. Why? Because a 
simple misconfiguration (face it, this can happen) can expose sensitive information in the var/ or other directories. 
Magento provides an easy way to avoid this issue by ensuring a correct document root. 
But what if I need to install Magento? Either use the /setup URL (by changing your application’s document root) 
temporarily. Or, better yet, use the CLI command: bin/magento setup:install.

* `var/`: this directory contains files that are temporarily used by the Magento application or results from using it. 
For example, logs are in var/log/. Error reports are in var/report. If you are using the file-system cache, this is 
stored in var/cache and var/page_cache. You should only use file- system caches in your development environment and 
never on production. The reason is that file-system storage prevents horizontal scaling (scaling Magento to multiple 
stand-alone instances)

* `vendor/`: this is where all of the Composer-installed modules are located.
This directory can be deleted and re-created at any time on your development machine (doing this in production would 
result in your store being down). To create this directory, run composer install (this command is different than 
composer update in that install doesn’t change any versions and only installs what is specified in composer.lock).
Because of this, it goes without saying that code in the vendor/ directory should never be modified. Doing so will be 
temporary and these changes will be destroyed next time you run composer update or composer install.

Source: https://bkaushik.com/magento2/description-of-magento-2-directory-structure/?fbclid=IwAR2ACc5TsjIRrL7_ui2vsize5S7ZQbW9Q3MASPFsjEML_BQkLfiTO3xsaWI

---

> ### 1.2.3 How can you identify the files responsible for some functionality?

To find the files responsible for certain functionality, search among di.xml files (Dependency Injection configuration). 
Another way is to search in a module or theme layout files, if you need to find certain blocks. The hints, enabled in 
the store settings (Stores -> Configuration -> Advanced -> Developer), can also prove helpful for this type of search.

For actions search, use the directories structure, because actions are located at the module directory the following 
way: `<module_dir>/Controllers/<Controller>/<Action>`). Front Name is specified in `<module_dir>/etc/<area>/routes.xml`, 
so finding it via the file search is a relatively simple task.

To search for the model functionality, use the full class or interface name (including namespace). Conduct the file 
search, specifying the full name of the class or the interface (without leading backlash).

---

## 1.3 Utilize configuration and configuration variables scope

---

> ### 1.3.1 Determine how to use configuration files in Magento.

One can modify the configuration from the admin panel only when it is stored in `core_config_data` and is not overridden 
in `app/etc/config.php`, `app/etc/env.php` or via environment variables. In case it is overridden, it is disabled at the 
admin panel.

`app/etc/config.php` and `app/etc/env.php` files contain Magento basic configuration (for instance, modules list, 
scopes, themes, database credentials, cache config, override `core_config_data` config and other). They are generated at 
the Magento 2 installation. `app/etc/config.php` file has shared configuration settings, while `app/etc/env.php` 
contains settings that are specific to the installation environment. 

As of the 2.2 release, the `app/etc/config.php` file is no longer an entry in the .gitignore file. This was done to 
facilitate pipeline deployment.

---

> ### 1.3.2 Which configuration files are important in the development cycle?

In Magento 2, the configuration is stored at the following locations:

* In xml files of modules, themes, languages and app/etc folder.
* In the database in core_config_data table.
* In `app/etc/config.php` and `app/etc/env.php` files.
* In the framework variables.

Below is the list of xml files inside the module:

* etc/config.xml – contains the default values of the options from `Stores > Configuration` in admin panel menu, as well 
as other options, like class names (for instance, `<model>Amazon\Payment\Model\Method\AmazonLoginMethod</model>`) and 
attributes (for example, `<account backend_model=”Magento\Config\Model\Config\Backend\Encrypted” />`).
* etc/di.xml and etc/<area>/di.xml – contains the configuration for dependency injection
* etc/events.xml and etc/<area>/events.xml – the list of the events and observers
* etc/<area>/routes.xml – routers’ list.
* etc/acl.xml – adds module resources into the resource tree, allowing to set up access for different users.
* etc/crontab.xml – adds and configures tasks for cron.
* etc/module.xml – declares module name and version, as well as its dependencies from other modules.
* etc/widget.xml – stores widget configuration.
* etc/indexer.xml – declares a new indexation type. There, view_id is specified, which denotes the views described in 
`etc/mview.xml`.
* etc/mview.xml – is used to track database changes for a certain entity.
* etc/webapi.xml – stores configurations for WEB API (REST/SOAP).
* etc/view.xml – stores product images' values.
* etc/product_types.xml – describes product types in a store.
* etc/product_options.xml – describes the types of options that products can have and the classes that render options in 
the admin.
* etc/extension_attributes.xml – the ability to add custom attribute, introduced in Magento 2 version. The file 
describes the attribute and its type, which can be simple, or complex, or have the form of an interface.
* etc/catalog_attributes.xml – adds attributes to the groups. `quote_item`, `wishlist_item`, `catalog_product`, 
`catalog_category`, `unassignable`, `used_in_autogeneration` are the standard groups. To learn more, follow the 
link: https://www.atwix.com/magento-2/how-to-access-custom-catalog-attributes/
* etc/adminhtml/system.xml – can relate to admin section solely, adds `Stores > Configuration` settings and describes 
form sections and fields.
* etc/adminhtml/menu.xml – can relate to admin area solely, adding the menu option in the admin panel.

---

> ### 1.3.3 Describe development in the context of website and store scopes.

---

> ### 1.3.4 How do you identify the configuration scope for a given variable?

---

> ### 1.3.5 How do native Magento scopes (for example, price or inventory) affect development and decision-making processes?

---

> ### 1.3.6 Demonstrate an ability to add different values for different scopes.

In Magento 2, XML configuration files have areas: `global`, `frontend` and `adminhtml`, `crontab`, `graphql`, 
`webapi_rest`, `webapi_soap`. You can find a list of them at `Magento\Framework\App\AreaList` class, defined via 
`di.xml`. Certain xml files can be specified for each area separately and some may not. For instance, `event.xml` file can 
be specified for each area (`global`, `frontend`, `adminhtml`, `etc`.), while module.xml can be specified only for 
global.

If config file is located at the module `etc` directory, its values are located in the global area. To specify 
configuration area, place the config file into `etc/<area>` folder. This is a new concept, introduced in Magento 2. 
Previously, in the first version of Magento, the visibility area was defined by a separate branch in XML file. 
This introduction allows loading configurations for various visibility areas separately. If the same parameters are 
specified for global and non-global areas (for instance, frontend or adminhtml), they will be merged and loaded 
together. The parameters, specified in non-global area, or located in `etc/<area>` folder, have the priority.

Configuration upload is executed in three steps:

System level configurations upload. Loading of the files, necessary for Magento 2 launch (like `config.php`).
Global area configurations upload. Loading of the files, located in `app/etc/` Magento 2 directory, such as `di.xml`, as 
well as files that relate to the global area and are directly located in modules’ etc/ folders.
Specific areas configurations upload. Loading of the files, located at `etc/adminhtml` or `etc/frontend` folders.

---

> ### 1.3.7 How can you fetch a system configuration value programmatically?

You can use the functions contained within the `Magento\Framework\App\Config\ScopeConfigInterface` class, passing the 
configuration path as the minimum required parameter e.g. `web/secure/base_url`.

---

> ### 1.3.8 How can you override system configuration values for a given store using XML configuration?

---

## 1.4 Demonstrate how to use dependency injection (DI)

---

> ### 1.4.1 Demonstrate the ability to use the dependency injection concept in Magento development.

---

> ### 1.4.2 How are objects realized in code?

Since dependency injection happens automatically through the constructor, Magento must handle class creation - either 
at the time of injection or via a factory.


#### Class Creation During Injection

First the object manager locates the proper class type. If an interface is requested, hopefully an entry in `di.xml` 
will provide a concrete class for the interface (if not, an exception will be thrown).

Then the parameters for the constructor are loaded and recursively parsed meaning that the dependencies for the 
initially requested class are loaded as well as the dependencies of those dependencies as well.

The deploy mode (`bin/magento deploy:mode:show`) determines which class loader is used:

* `vendor/magento/framework/ObjectManager/Factory/Dynamic/Developer.php`
* `vendor/magento/framework/ObjectManager/Factory/Dynamic/Production.php`

#### Class Creation Via Factories

Inject a class factory by adding `Factory` to the end of a class' name when injecting via constructor. Doing so, a 
factory class will be created in the `generated` directory. Now we can use the `create()` method of this class to create
our real class.

---

> ### 1.4.3 Why is it important to have a centralized object creation process?

Having a centralised process to create objects makes testing much easier. It also provides a simple interface to 
substitute objects as well as modify existing ones.

---

> ### 1.4.4 Identify how to use DI configuration files for customizing Magento.

Each module can have a global and an area-specific di.xml file. Area-specific di.xml files are recommended to be applied 
for dependencies configuration for presentation layer, while global file – for all the rest.

---

> ### 1.4.5 How can you override a native class, inject your class into another object, and use other techniques available in di.xml (for example, virtualTypes)?

#### Overriding Native Classes

Preferences are used to substitute entire classes. They can also be used to specify concrete classes for interfaces:

```xml
<preference for="Magento\GoogleTagManager\Block\ListJson" type="YourCompany\YourModule\Path\To\Your\Class"/>
```

#### Injecting Your Class into Other Objects

Specify a `<type>` entry with your class as an `<argument>`:

```xml
<type name="Path\To\Your\Class\To\Inject\Into">
    <arguments>
        <argument xsi:type="object">
            Path\To\Your\Injected\Class
        </argument>
    </arguments>
</type>
```

#### Virtual Types

A virtual type allows you to create an instance of an existing class that has custom constructor arguments. This is 
useful in cases where you need a "new" class only because the constructor arguments need to be changed. This is used 
frequently in Magento to reduce redundant PHP classes.

---

> ### 1.4.6 Given a scenario, determine how to obtain an object using the ObjectManager object.

```php
$jsonClass = Magento\Framework\App\ObjectManager::getInstance()->get(Magento\Framework\Serialize\Serializer\Json::class);
```

---

> ### 1.4.7 How would you obtain a class instance from different places in the code?

Always use the `__construct` method to obtain dependencies. If you are working with a `.phtml` template, use a 
`ViewModel` (which implements `ArgumentInterface`).

---

## 1.5 Demonstrate ability to use plugins

> ### 1.5.1 Demonstrate an understanding of plugins.

A plugin, or interceptor, is a class that modifies the behavior of public class functions by intercepting a function
call and running code before, after, or around that function call. This allows you to *substitute* or *extend* the 
behavior of original, public methods for any *class* or *interface*.

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

> ### 1.5.2 How are plugins used in core code?

---

> ### 1.5.3 How can they be used for customizations?

---

## 1.6 Configure event observers and scheduled jobs

---

> ### 1.6.1 Overview.

Events are commonly used in applications to handle external actions or input. Each action is interpreted as an event.

Events are part of the Event-Observer pattern. This design pattern is characterized by objects (subjects) and their list
of dependents (observers). It is a very common programming concept that works well to decouple the observed code from
the observers. Observer is the class that implements Magento\Framework\Event\ObserverInterface interface.

According to https://devdocs.magento.com/guides/v2.3/coding-standards/technical-guidelines.html:

All values (including objects) passed to an event MUST NOT be modified in the event observer. Instead, plugins SHOULD BE
used for modifying the input or output of a function.

Therefore, if there is a need to modify the input data, use plugins instead of events.

---

> ### 1.6.2 Demonstrate how to create a customization using an event observer.

---

> ### 1.6.3 How are observers registered? How are they scoped for frontend or backend?

---

> ### 1.6.4 How are automatic events created, and how should they be used?

---

> ### 1.6.5 How are scheduled jobs configured?

To demonstrate how to configure a scheduled job, we create in the module a `crontab.xml` file with the similar content:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:module:Magento_Cron:etc/crontab.xsd">
    <group id="default">
        <job name="my_module_cron_job" instance="Vendor\Module\Model\Cron" method="run">
            <!-- Use schedule or config_path, not both -->
            <schedule>0 * * * *</schedule>
            <config_path>my_module/my_group/my_setting</config_path>
        </job>
    </group>
</config>
```

Group element determines to which group cron jobs should be tied. Group is declared in `cron_groups.xml` file and contains 
group configurations. The events inside the group have a general queue, while several groups can be launched 
simultaneously. Example:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:module:Magento_Cron:etc/cron_groups.xsd">
    <group id="default">
        <schedule_generate_every>15</schedule_generate_every>
        <schedule_ahead_for>20</schedule_ahead_for>
        <schedule_lifetime>15</schedule_lifetime>
        <history_cleanup_every>10</history_cleanup_every>
        <history_success_lifetime>60</history_success_lifetime>
        <history_failure_lifetime>4320</history_failure_lifetime>
        <use_separate_process>0</use_separate_process>
    </group>
</config>
```

Job element contains name (name of the job), instance (job class name) and method (job method name in the class) 
attributes. Also, job contains schedule element (http://www.nncron.ru/help/EN/working/cron-format.htm) or `config_path` 
(configuration path to the schedule value).

---

## 1.7 Utilize the CLI

---

> ### 1.7.1 Describe the usage of bin/magento commands in the development cycle.

---

> ### 1.7.2 Which commands are available?

---

> ### 1.7.3 How are commands used in the development cycle?

---

## 1.8 Describe how extensions are installed and configured

---

> ### 1.8.1 How would you install and verify an extension by a customer’s request?

---