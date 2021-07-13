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

---

> ### What are the naming conventions, and how are namespaces established?

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