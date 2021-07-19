# Section 1: Magento Architecture & Customization Techniques (33%)

## 1.1 Describe the Magento module-based architecture



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

---

> ### 1.2.2 What are the naming conventions, and how are namespaces established?

---

> ### 1.2.3 How can you identify the files responsible for some functionality?

---

## 1.3 Utilize configuration and configuration variables scope

---

> ### 1.3.1 Determine how to use configuration files in Magento.

---

> ### 1.3.2 Which configuration files are important in the development cycle?

---

> ### 1.3.3 Describe development in the context of website and store scopes.

---

> ### 1.3.4 How do you identify the configuration scope for a given variable?

---

> ### 1.3.5 How do native Magento scopes (for example, price or inventory) affect development and decision-making processes?

---

> ### 1.3.6 Demonstrate an ability to add different values for different scopes.

---

> ### 1.3.7 How can you fetch a system configuration value programmatically?

---

> ### 1.3.8 How can you override system configuration values for a given store using XML configuration?

---

## 1.4 Demonstrate how to use dependency injection (DI)

---

> ### 1.4.1 Demonstrate the ability to use the dependency injection concept in Magento development.

---

> ### 1.4.2 How are objects realized in code?

---

> ### 1.4.3 Why is it important to have a centralized object creation process?

---

> ### 1.4.4 Identify how to use DI configuration files for customizing Magento.

---

> ### 1.4.5 How can you override a native class, inject your class into another object, and use other techniques available in di.xml (for example, virtualTypes)?

---

> ### 1.4.6 Given a scenario, determine how to obtain an object using the ObjectManager object.

---

> ### 1.4.7 How would you obtain a class instance from different places in the code?

---

## 1.5 Demonstrate ability to use plugins

> ### 1.5.1 Demonstrate an understanding of plugins.

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

> ### 1.5.2 How are plugins used in core code?

---

> ### 1.5.3 How can they be used for customizations?

---

## 1.6 Configure event observers and scheduled jobs

---

> ### 1.6.1 Demonstrate how to create a customization using an event observer.

---

> ### 1.6.2 How are observers registered? How are they scoped for frontend or backend?

---

> ### 1.6.3 How are automatic events created, and how should they be used?

---

> ### 1.6.4 How are scheduled jobs configured?

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