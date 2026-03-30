# Refactoring Tasks for index.php

This is the main entry point for the application. It should be refactored to be a simple front controller.

## 1. Front Controller Pattern

**Problem:** The file contains logic to define the `WP_USE_THEMES` constant and then includes `wp-blog-header.php`.

**Task:**
-   The `index.php` file should be simplified to be a pure front controller.
-   It should initialize the application (e.g., autoloader, dependency injection container, error handling).
-   It should then hand off the request to a router component to handle the request.

## 2. Remove Global Constants

**Problem:** The file defines the `WP_USE_THEMES` constant, which is a global state.

**Task:**
-   This constant should be removed and replaced with a configuration value that is passed to the application.

## 3. Use an Autoloader

**Problem:** The file manually includes `wp-blog-header.php`.

**Task:**
-   Use a PSR-4 autoloader to automatically load the required classes.

