# How to Trace Feature Dependencies (The 3-Step Strategy)

In a large, offline-first application using the GetX and Hive architecture (like the Bookz App), you cannot understand a feature just by looking at its own folder. You must map out its **System Context**—how it interacts with the rest of the application.

To accurately build **Box 4 (External Mutators)** and **Box 5 (External Readers)** of our architecture diagrams, we must treat the codebase like an investigation.

Here is the exact, 3-step search strategy used by Senior Software Architects to find every hidden connection and dependency a feature has across the entire app.

---

### Step 1: Search the GetX Controllers (The UI Integrations)
**Goal:** Find out which external UI screens trigger or rely on this feature's business logic.
*   **What to search for globally (excluding the feature's own folder):** 
    *   `Get.put(TargetFeatureController())`
    *   `Get.find<TargetFeatureController>()`
*   **Example (Products Feature):** Searching for `ProductsListController` outside the `/products/` folder reveals that `Sales`, `Purchases`, and `Estimates` inject this controller.
*   **What this proves:** These external features rely on the target feature to display data to the user (e.g., Sales popping open a "Select Product" list). 
*   **Maps to:** **Box 5 (External Readers)**.

### Step 2: Search the Models (The Data Structure Integrations)
**Goal:** Find out which external controllers or services need to understand the structural blueprint of this feature's data.
*   **What to search for globally:** The import path of the target feature's primary Hive models.
    *   `import 'package:bookz/app/hive/models/target_model.dart';`
*   **Example (Products Feature):** Searching for `models/products.dart` reveals 47 imports across the app, including inside `add_exp_product_controller.dart`.
*   **What this proves:** These external features need to read or manipulate the target feature's data structure (e.g., an Expense needing to know what a Product's 'price' field looks like to calculate a total).
*   **Maps to:** **Box 5 (External Readers)** and is a prerequisite for Box 4.

### Step 3: Search the Hive Operations (The Hidden Mutators)
**Goal:** Find out which external features bypass the target feature's UI/Logic entirely and write directly to its database. This is the most critical and dangerous type of dependency.
*   **What to search for globally:** The instantiation of the target feature's specific Hive Database class, or the exact names of its Write/Delete methods.
    *   `HiveTargetFeatureController()`
    *   `updateTargetTable(`
    *   `deleteTarget(`
*   **Example (Products Feature):** Searching for `updateProductTbl` reveals that the `Expenses` and `Sales` features call this exact database method directly.
*   **What this proves:** These external features are secretly modifying the target feature's data in the background (e.g., Sales automatically reducing a Product's inventory stock). If you change how the database saves a product, you will break the Sales feature.
*   **Maps to:** **Box 4 (External Mutators)**.

---

### Summary

If you execute these three searches:
1. **The Logic/UI Map (GetX)**
2. **The Data Contract Map (Models)**
3. **The Mutation Map (Hive)**

...it is mathematically impossible to miss a dependency in this architecture. You will have a 100% accurate, complete map of every single module that interacts with the target feature.