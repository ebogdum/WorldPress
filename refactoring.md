# PHP Refactoring Plan for AI-Friendliness

This document outlines a series of refactoring tasks to modernize the PHP codebase and make it more modular, maintainable, and easier for AI tools to understand and work with.

## 1. Introduce a Dependency Injection (DI) Container

**Problem:** The codebase heavily relies on global variables and functions, making dependencies implicit and hard to track. This is a major obstacle for static analysis and understanding the code's structure.

**Task:**
-   Introduce a modern DI container (e.g., `PHP-DI`, `Symfony DI`).
-   Gradually refactor the code to explicitly define and inject dependencies instead of using globals like `$wpdb`, `$wp_query`, etc.
-   **Example:** In `wp-load.php`, instead of global variables, dependencies should be instantiated and passed to the components that need them.

## 2. Enforce Strict Typing

**Problem:** The lack of type hints makes function signatures ambiguous and prone to errors.

**Task:**
-   Enable strict types (`declare(strict_types=1);`) in all new and refactored PHP files.
-   Add scalar type hints (`int`, `string`, `bool`, `float`) and return type declarations to all functions and methods where possible.
-   Use `?` for nullable types where appropriate.
-   **Example:** Refactor functions in `wp-includes/functions.php` to include type hints.

## 3. Improve Modularity and Code Organization

**Problem:** Many files are very large and contain a mix of unrelated functions and logic (e.g., `wp-settings.php`, `wp-admin/admin.php`).

**Task:**
-   Break down large files into smaller, more focused modules (e.g., classes with a single responsibility).
-   Group related functions into classes. For example, database-related functions could be grouped into a `Database` class.
-   Organize files into a more modern directory structure, possibly following PSR-4 for autoloading.
-   **Example:** Analyze `wp-settings.php` and identify blocks of logic that can be extracted into separate classes or services.

## 4. Adopt PSR-4 Autoloading

**Problem:** The codebase uses `require` and `include` statements to manually load files, which is brittle and inefficient.

**Task:**
-   Introduce a `composer.json` file if one doesn't exist.
-   Configure PSR-4 autoloading to map namespaces to directories.
-   Replace manual `require`/`include` statements with `use` statements for classes.
-   **Example:** Create a `src/` directory and move refactored classes into it, following a namespace structure like `WordPress\Core\`.

## 5. Refactor Global State and Functions

**Problem:** The extensive use of global variables (`global $...;`) and globally-scoped functions pollutes the global namespace and creates tight coupling.

**Task:**
-   Identify all uses of the `global` keyword.
-   Refactor the code to pass dependencies via constructors or method arguments.
-   Wrap global functions in classes as static or instance methods.
-   **Example:** In `wp-admin/` files, many scripts use `global $pagenow;`. This should be passed as a parameter to the relevant functions or classes.

## 6. Standardize Code Style

**Problem:** The code style may be inconsistent across the codebase.

**Task:**
-   Adopt a modern coding standard like PSR-12.
-   Use a tool like `PHP-CS-Fixer` to automatically format the code.
-   Create a configuration file for the chosen code style to ensure consistency.

## 7. Improve Error Handling

**Problem:** Error handling might be inconsistent, relying on `wp_die()` or other custom mechanisms.

**Task:**
-   Use modern exception-based error handling.
-   Define custom exception classes for different error types.
-   Replace calls to `wp_die()` with thrown exceptions that can be caught by a central error handler.

## 8. Refactor the Routing/Request Handling

**Problem:** The request handling logic is spread across multiple files and relies on global state.

**Task:**
-   Implement a modern routing component (e.g., from Symfony or Laravel) to handle incoming requests.
-   Define explicit routes that map to controller classes and methods.
-   This will make the application's entry points and flow much clearer.
-   **Example:** The logic in `index.php` that loads `wp-blog-header.php` could be replaced by a router that dispatches the request to a specific controller.
