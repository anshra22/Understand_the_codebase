# The Dependency Audit Rule (Step 0 for Architecture Diagrams)

Before drawing any architectural diagram (Level 1 or Level 2), you MUST perform a systematic Code Audit of the target feature. 

This rule prevents drawing theoretical diagrams and forces you to map the **actual physical dependencies** found in the codebase.

When instructed to map or analyze a feature, you must execute this audit and output a structured **Dependency Report** before proceeding to any diagram generation.

---

### The Audit Execution Checklist

For every file within the target feature's directory (`view/`, `controller/`, `models/`), you must perform the following checks:

#### 1. Audit the Presentation Layer (`view/` directory)
*   **Direct Database Access (Anti-Pattern Check):** 
    *   Search for: `Hive.openBox(...)` or `HiveOperations()`.
    *   *Why:* UI files should not talk directly to the database. If found, this must be explicitly mapped as a UI-to-Database dependency.
*   **Global State Dependency:**
    *   Search for: `SharedPreferences.getInstance()`.
    *   *Why:* Indicates the UI's layout or logic changes based on global app variables (e.g., User ID, Locale, or Global Tax Flags).
*   **External Logic Borrowing:**
    *   Search for: `Get.find<[External]Controller>()`.
    *   *Why:* Proves the UI relies on business logic from a completely different feature to function.
*   **Security & Auth Rules:**
    *   Search for: `PermissionHelper` or Auth/Role checks.
    *   *Why:* Proves the UI is dynamically hiding/showing elements based on the current user's role.

#### 2. Audit the Business Logic Layer (`controller/` directory)
*   **Database Mutation Boundaries:**
    *   Search for: Every instance of `Hive[Name]Controller().[method]()`.
    *   *Why:* This explicitly defines exactly which database tables (Boxes) this controller reads from or writes to.
*   **Data Contract Dependencies:**
    *   Search for: External model imports (e.g., `import '.../models/tax_group.dart';`).
    *   *Why:* Proves this controller cannot function without understanding the data structure of another feature.
*   **Global State & Settings:**
    *   Search for: `SharedPreferences` or `BusinessSettings` checks.
    *   *Why:* Proves the business logic executes differently based on overarching master configurations.

---

### The Required Output Format (The Dependency Report)

Before generating any Mermaid diagrams, the AI must output a report formatted exactly like this example:

**Target Feature:** `[Feature Name]`

**1. Presentation Layer (Views) Audit**
*   `file_name.dart`:
    *   **External Logic:** Injects `[ControllerName]`.
    *   **Direct DB Access:** Opens `[HiveBoxName]` to check `[Setting/Rule]`.
    *   **Global State:** Reads `[PreferenceKey]` from SharedPreferences.
    *   **Security:** Checks `[PermissionName]`.

**2. Business Logic Layer (Controllers) Audit**
*   `controller_name.dart`:
    *   **DB Mutations:** Calls `[HiveMethod]()` in `[HiveFile.dart]`.
    *   **Data Contracts:** Imports `[ExternalModelName]`.
    *   **Global State:** Reads `[PreferenceKey]` to dictate logic.

*Only after this report is generated and reviewed should the actual C4 or Flow diagrams be constructed.*