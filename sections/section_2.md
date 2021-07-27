# Section 2: Request Flow Processing (7%)

## 2.1 Describe how to use Magento modes

---

> ### 2.1.1 Understand the pros and cons of using developer mode or production mode.

**Default mode**

Enables you to deploy the Magento application on a single server without changing any settings. However, default mode is not optimized for production.

To deploy the Magento application on more than one server or to optimize it for production, change to one of the other modes.

* Static view file caching is enabled
* Exceptions are not displayed to the user; instead, exceptions are written to log files.
* Hides custom `X-Magento-*` HTTP request and response headers


**Developer mode**

Intended for development only, this mode:

* Disables static view file caching
* Provides verbose logging
* Enables automatic code compilation
* Enables enhanced debugging
* Shows custom `X-Magento-*` HTTP request and response headers
* Results in the slowest performance
* Shows errors on the frontend

**Production mode**

Intended for deployment on a production system, this mode:

* Does not show exceptions to the user (exceptions are written to logs only).
* Serves static view files from cache only.
* Prevents automatic code file compilation. New or updated files are not written to the file system.
* **Does not allow you to enable or disable cache types in Magento Admin.**


**Maintenance mode**

Intended to prevent access to a Magento Commerce site while it is being updated or reconfigured, this mode:

* Redirects site visitors to a default `Service Temporarily Unavailable` page.
* When the site is in maintenance mode, the `var/` directory contains the `.maintenance.flag` file.
* You can configure maintenance mode to allow visitor access from a specified list of IP addresses.

---

> ### 2.1.2 How do you enable/disable maintenance mode?

To enable/disable maintenance mode either: 
* Create/delete the `.maintenance.flag` file under `var` at the root of the magento installation
* Run the `bin/magento maintenance:enable` / `bin/magento maintenance:disable` command

If you are using Cloud for Adobe Commerce, the Magento application runs in maintenance mode during the deploy phase. 
When the deployment completes successfully, Magento returns to running in production mode.

---

## 2.2 Demonstrate the ability to create a frontend controller with different response types (HTML / JSON / redirect)

---

> ### 2.2.1 How do you identify which module/controller corresponds to a given URL?

---

> ### 2.2.2 What would you do to create a given URL?

---

## 2.3 Demonstrate how to use URL rewrites for a catalog product view to a different URL

---

> ### How is the user-friendly URL of a product or category defined? How can you change it?

---

> ### How do you determine which page corresponds to a given user-friendly URL?

---